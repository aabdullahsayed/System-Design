# Practices, Incident Response & Cheat Sheet

## Best practices
- Instrument **before** you need it: standard middleware for RED metrics on every service.
- Standardize metric names, units (use base units: seconds, bytes), and labels.
- Keep label cardinality bounded.
- Monitor **dependencies** (DB, cache, queues, third-party APIs) and your **own pipeline** (is Prometheus/alerting itself healthy? use a "dead man's switch" / heartbeat alert).
- Define SLOs for user-critical journeys, then alert on burn rate.
- Every page has a **runbook**; review and prune alerts regularly.
- Tag deploys, feature flags, and config changes so they appear on graphs.
- Capacity planning: track growth trends; alert on saturation before limits.
- Retention tiers: raw data short-term, downsampled data long-term.
- Test alerts (fire drills, game days, chaos experiments).

## Common anti-patterns
- Alerting on every CPU spike or single-host blip
- Dashboards nobody looks at; no owner
- Averages instead of percentiles
- High-cardinality labels (user ID in metrics)
- Alerts with no runbook or clear action
- Monitoring only infrastructure, not user-facing outcomes
- No monitoring of the monitoring system
- Treating logs as the only source of truth

## Incident response flow
1. **Detect**: alert fires or user report.
2. **Triage**: assess impact and severity; assign incident commander.
3. **Mitigate**: stop the bleeding first (rollback, failover, scale, disable feature); root cause can wait.
4. **Investigate**: dashboards (what changed?) -> traces (where?) -> logs (why?).
5. **Communicate**: status page and stakeholder updates at regular intervals.
6. **Resolve & verify**: confirm metrics return to normal.
7. **Postmortem**: blameless; timeline, root cause, contributing factors, action items with owners.

## Key incident metrics
| Metric | Meaning |
|--------|---------|
| **MTTD** | Mean time to detect |
| **MTTA** | Mean time to acknowledge |
| **MTTR** | Mean time to recover/resolve |
| **MTBF** | Mean time between failures |
| **Change failure rate** | % of deployments causing incidents |

## Debugging slow/failing service: quick path
1. Check **Golden Signals** on the service dashboard: which one moved?
2. Did anything change? Check deploy/config markers.
3. Compare against dependencies: DB latency, cache hit ratio, queue lag.
4. Look at saturation: CPU throttling, memory/GC, thread/connection pools.
5. Pull a slow/error **trace**; find the slow span.
6. Read **logs** filtered by that `trace_id`.
7. Mitigate (rollback, scale, shed load), then fix root cause.

## Three pillars summary
| | Logs | Metrics | Traces |
|---|---|---|---|
| Question | Why did this happen? | Is it healthy / trending? | Where is the time spent? |
| Granularity | Per event | Aggregated | Per request |
| Storage cost | High | Low | Medium |
| Used for | Debugging, audit | Alerting, SLOs, capacity | Latency, dependency analysis |

## Interview one-liners
- *Monitoring vs observability?* Monitoring checks known failures; observability lets you investigate unknown ones.
- *Four golden signals?* Latency, traffic, errors, saturation.
- *Why percentiles?* Averages hide tail latency affecting real users.
- *SLO vs SLA?* SLO is an internal target; SLA is a contractual promise with penalties.
- *What's alert fatigue?* Too many noisy alerts leading people to ignore real ones; fix by alerting on symptoms and tuning.
- *Counter vs gauge?* Counter only increases (use rate); gauge goes up and down.
- *Why avoid high-cardinality labels?* They blow up time-series count, memory, and cost.
