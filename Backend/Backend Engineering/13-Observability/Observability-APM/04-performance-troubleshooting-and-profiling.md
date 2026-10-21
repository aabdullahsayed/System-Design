# Performance Troubleshooting & Profiling

## A systematic approach
1. **Define the symptom**: which signal is off (latency, errors, throughput, saturation)? Since when? Who is affected?
2. **Scope it**: one endpoint, region, tenant, version, or everything?
3. **Check what changed**: deploys, config, flags, dependency updates, traffic shape, infra events.
4. **Follow the data**: dashboard -> service map -> slow/error traces -> span -> logs -> profile.
5. **Form a hypothesis, test it** with a query or experiment.
6. **Mitigate first** (rollback, scale out, disable flag, shed load), then fix root cause.
7. **Verify** with metrics/SLIs, then write a blameless postmortem.

## Cohort analysis (observability's superpower)
Compare "slow" vs "normal" requests across many attributes (version, region, customer, DB shard, feature flag, user-agent) to find what's different, e.g. "all slow requests are from `build=2.14.1` on `shard=7`".

## Common performance problems and signatures
| Problem | Signature | Typical fix |
|---------|-----------|-------------|
| **N+1 queries** | Many tiny identical DB spans per request | Batch/join, eager loading, caching |
| **Slow query** | One long DB span; high DB time | Index, query rewrite, pagination, read replica |
| **Connection pool exhaustion** | Latency spikes, threads waiting on pool, timeouts | Tune pool, fix leaks, reduce hold time |
| **Thread pool / event-loop saturation** | Rising queue time, low CPU, high latency | Add capacity, remove blocking calls, async I/O |
| **Memory leak** | Heap grows steadily, frequent GC, OOM kills | Heap dumps/allocation profiling, fix retention |
| **GC pauses** | Periodic latency spikes aligned with GC | Tune heap/collector, reduce allocations |
| **Slow dependency** | Long external-call span; timeouts | Timeouts, retries with backoff, circuit breaker, cache, fallback |
| **Retry storm** | Traffic multiplies during partial failure | Backoff + jitter, retry budgets, circuit breakers |
| **Cache stampede / low hit rate** | DB load spikes after expiry | Single-flight, jittered TTLs, warm caches |
| **Lock contention** | High latency, low CPU, threads blocked | Reduce critical sections, sharding, lock-free designs |
| **CPU hotspot** | High CPU, flame graph wide box | Optimize algorithm, cache, parallelize |
| **Noisy neighbor / CPU throttling** | Container throttling metric, latency tied to cgroup limits | Adjust requests/limits, isolate workloads |
| **Slow cold start** | Latency on first requests/serverless | Provisioned concurrency, smaller bundles, warmups |
| **Large payloads** | Long serialization/network time | Pagination, compression, field selection |
| **Chatty services** | Many sequential calls in trace waterfall | Parallelize, batch, aggregate API |

## Reading a trace waterfall
- **Long span, no children** -> that operation itself is slow (CPU, I/O, lock).
- **Gaps between spans** -> time in uninstrumented code, queueing, or network.
- **Sequential spans that could be parallel** -> chance to fan out.
- **Many repeated spans** -> N+1 or chatty calls.
- **Retries** -> hidden latency and load amplification.

## Continuous profiling
Always-on, low-overhead sampling of CPU, memory (allocations/heap), locks, and wall-clock time, tagged by service/version.
- **Flame graph**: width = time (or samples), stack grows upward/downward; wide boxes = hot paths.
- Compare profiles **before/after a deploy** (diff flame graphs).
- Tools: Pyroscope, Parca, Datadog Profiler, Google Cloud Profiler, async-profiler (JVM), `py-spy` (Python), `pprof` (Go), `perf`/eBPF.

## Load and capacity testing (feeds observability)
- Tools: k6, Gatling, JMeter, Locust, wrk.
- Test with production-like data and traffic mixes; watch saturation signals and latency percentiles, not just averages.
- Find the **knee** of the curve where latency climbs sharply.
- Use findings to set autoscaling policies, resource requests/limits, and alert thresholds.
- Beware **coordinated omission** (load generators that wait for slow responses underreport tail latency).

## Database observability checklist
- Query latency (p95/p99), rows examined vs returned, slow query log
- Top queries by total time (not just single-slow ones)
- Lock waits, deadlocks, connection counts, replication lag
- Cache hit ratio, index usage, table bloat/vacuum health
- Link queries to the originating service/endpoint via trace context or query comments (sanitize SQL)

## Kubernetes/container signals
- Pod restarts, OOMKilled, CrashLoopBackOff
- CPU throttling (`container_cpu_cfs_throttled_*`), memory working set vs limit
- Pending pods, node pressure, HPA behavior
- Network errors/retransmits, DNS latency
- Correlate with app metrics by pod/namespace/version labels

## Debugging playbook: "the API is slow"
1. Alert/SLI shows p99 latency up on `/search`.
2. Check traffic and error rate: unchanged traffic, errors normal -> latency-only issue.
3. Cohort analysis: slow requests all hit `search-service v3.2` and `es-cluster-b`.
4. Trace: long `elasticsearch.query` span; many sequential calls (query per filter).
5. Deploy marker: v3.2 shipped 40 minutes ago.
6. Mitigate: roll back to v3.1 -> latency recovers.
7. Root cause: new code issued one ES query per facet; fix with a single multi-search request; add a regression test and a latency SLO alert on the endpoint.
