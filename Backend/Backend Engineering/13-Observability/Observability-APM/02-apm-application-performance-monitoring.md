# Application Performance Monitoring (APM)

## What is APM?
APM monitors the **performance, availability, and user experience of software applications**. It focuses on the application layer: transactions, code-level behavior, dependencies, and errors. Modern APM overlaps heavily with observability; it is typically delivered as an agent/SDK plus a backend and UI.

## Core capabilities
| Capability | Description |
|------------|-------------|
| **Transaction / request monitoring** | Throughput, latency percentiles, error rates per endpoint/operation |
| **Distributed tracing** | Follow a request across services, queues, databases |
| **Service map / dependency map** | Auto-discovered topology with health on each edge |
| **Error tracking** | Grouped exceptions, stack traces, affected users, release regression |
| **Database monitoring** | Slow queries, N+1 patterns, query plans, connection pools |
| **External call monitoring** | Latency/errors for third-party APIs |
| **Code-level diagnostics / profiling** | Method-level hotspots, flame graphs |
| **Runtime metrics** | Heap, GC, threads, event loop lag, connection pools |
| **Real User Monitoring (RUM)** | Browser/mobile page loads, Core Web Vitals, JS errors, sessions |
| **Synthetic monitoring** | Scripted uptime and journey checks from global locations |
| **Infrastructure correlation** | Link app slowness to hosts, containers, pods |
| **Alerting & SLOs** | Thresholds, anomaly detection, burn-rate alerts |
| **Deployment tracking** | Compare performance before/after releases |

## Key APM metrics
| Metric | Meaning | Typical use |
|--------|---------|------------|
| **Response time / latency** (p50, p95, p99) | Time to serve a request | User experience; SLOs |
| **Throughput** (RPM / RPS) | Requests per unit time | Load, capacity |
| **Error rate** | % failed requests / exceptions | Reliability |
| **Apdex** | Satisfaction score (0-1) from satisfied/tolerating/frustrated buckets | Single-number UX indicator |
| **Saturation** | Thread pool, connection pool, queue depth, CPU | Early warning |
| **Time in dependencies** | DB, cache, external API share of request time | Bottleneck location |
| **GC pause / heap usage** | Runtime health | Memory/latency issues |
| **Availability / uptime** | % time service is reachable | SLAs |

### Apdex formula
```
Apdex = (Satisfied + Tolerating/2) / Total samples
Satisfied: response <= T          Tolerating: T < response <= 4T        Frustrated: > 4T
```
Example: T = 0.5s, 80 satisfied, 15 tolerating, 5 frustrated -> (80 + 7.5) / 100 = **0.875**.

## Frontend / RUM
- **Core Web Vitals**: LCP (loading), INP (interactivity), CLS (visual stability).
- Also TTFB, FCP, long tasks, JS errors, resource timings, session replay.
- Segment by browser, device, geography, network, release.
- Connect frontend traces to backend traces (propagate `traceparent` from browser fetch/XHR).

## Mobile APM
- App start time, crashes/ANRs, network request timing, battery/memory, release adoption, crash-free sessions/users.

## Synthetic monitoring
- **Uptime/ping**, **API checks**, **browser journeys** (login -> search -> checkout).
- Great for off-hours detection, SLA verification, and checking third-party flows.
- Complements RUM: synthetic = controlled and consistent, RUM = real and diverse.

## Agent models
| Model | How | Notes |
|-------|-----|-------|
| **Language agent (auto-instrumentation)** | Bytecode/monkey-patch libraries at startup (Java agent, Python, Node, .NET) | Fast to adopt, minimal code changes |
| **SDK / manual instrumentation** | Code calls the API to create spans/metrics | Full control, business-specific context |
| **eBPF-based** | Kernel-level, no code change | Language-agnostic; limited app-level context |
| **Sidecar / service mesh** | Proxy captures network telemetry | Good L7 visibility; no inner-code detail |

## Overhead considerations
- Agents add CPU/memory/latency overhead (usually low single-digit %). Measure in staging.
- Use **sampling**, limit span attributes, and cap payload sizes.
- Profiling should be low-overhead (sampling-based, e.g. 1-5% CPU).

## Popular APM / observability platforms
| Type | Tools |
|------|-------|
| Commercial SaaS | Datadog, New Relic, Dynatrace, Splunk Observability, AppDynamics, Honeycomb, Elastic Observability |
| Cloud-native | AWS X-Ray + CloudWatch, Azure Application Insights, Google Cloud Trace/Monitoring |
| Open source | Grafana LGTM (Loki, Grafana, Tempo, Mimir), Prometheus, Jaeger, Zipkin, SigNoz, Apache SkyWalking, OpenTelemetry |
| Error tracking | Sentry, Rollbar, Bugsnag |
| Profiling | Pyroscope, Parca, Datadog Continuous Profiler, async-profiler |
