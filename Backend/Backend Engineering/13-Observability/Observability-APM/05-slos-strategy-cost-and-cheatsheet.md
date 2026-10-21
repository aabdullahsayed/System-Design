# SLOs, Strategy, Cost & Cheat Sheet

## SLOs with APM data
1. Identify **critical user journeys** (login, search, checkout).
2. Choose **SLIs** from real telemetry:
   - Availability: good requests / total requests
   - Latency: % of requests faster than a threshold (e.g. 95% < 300 ms)
   - Quality/freshness/correctness where relevant
3. Set **SLOs** (e.g. 99.9% over 30 days) based on user needs and cost.
4. Track the **error budget**; use **burn-rate alerts** (fast burn pages, slow burn tickets).
5. Review regularly; tighten or relax based on reality and business value.

```
Error budget (99.9%/30d) = 0.1% * 30 d ≈ 43.2 min
Burn rate = (observed error rate) / (allowed error rate)
Burn rate 14.4 -> consumes 2% of monthly budget in 1 hour (page)
Burn rate 1    -> consumes budget exactly over the window
```

## Alerting guidance
- Alert on **symptoms/SLO burn**, not every cause.
- Each page has a runbook, dashboard link, owner, and recent-change context.
- Group, deduplicate, and set severity; route by service ownership.
- Use anomaly detection for seasonality, but keep critical paths on explicit SLOs.
- Alert on telemetry pipeline health too (dropped spans, collector failures, ingestion lag).

## Building an observability practice
| Step | Action |
|------|--------|
| 1. Standardize | OpenTelemetry, naming conventions, required attributes (`service.name`, `env`, `version`) |
| 2. Instrument | Auto-instrument first, then add business spans/metrics at key points |
| 3. Centralize | Common backend(s), consistent retention, access controls |
| 4. Correlate | Trace IDs in logs, exemplars, deploy markers, service ownership metadata |
| 5. Define SLOs | For critical journeys; link alerts to them |
| 6. Operationalize | Runbooks, on-call, dashboards-as-code, incident reviews |
| 7. Optimize | Sampling, cardinality control, retention tiers, cost reviews |
| 8. Culture | Observability in design reviews and definition of done |

## Dashboards that work
- **Service overview**: Golden Signals (latency, traffic, errors, saturation), SLO status, recent deploys.
- **Dependency view**: DB, cache, queues, external APIs.
- **Infra/Kubernetes view**: pods, nodes, throttling, OOMs.
- **Business view**: orders/min, conversion, revenue, failed payments.
- Use dashboards-as-code (Grafana JSON/Terraform, Jsonnet) and version control them.
- Link everything: alert -> dashboard -> trace -> logs -> profile.

## Cost management
Observability bills are often driven by **volume and cardinality**.
- **Sampling** traces (tail-based to keep errors/slow ones).
- **Drop/filter** noisy logs and health checks at the collector.
- Limit **metric label cardinality**; avoid unbounded IDs.
- Use **tiered retention**: hot (days), warm (weeks), cold/archive (months, object storage).
- **Aggregate/downsample** old metrics.
- Set **quotas per team/service**; show cost attribution (showback/chargeback).
- Prefer open standards (OTel) to avoid vendor lock-in and enable multi-backend routing.
- Review top contributors monthly.

## Security & privacy
- Never put secrets, tokens, or sensitive PII in spans, logs, or metric labels.
- Scrub/sanitize SQL, URLs, headers, and bodies at the SDK and collector.
- Encrypt telemetry in transit; RBAC on dashboards and raw data.
- Define retention aligned with GDPR/HIPAA/PCI requirements.
- Treat telemetry pipelines as production systems: authenticate, rate-limit, and monitor them.

## Build vs buy
| | Open source stack (Grafana/Prometheus/Tempo/Loki/OTel) | Commercial SaaS |
|---|---|---|
| Cost model | Infra + people | Per host/GB/event/user (can be high) |
| Control & customization | High | Limited by vendor |
| Time to value | Slower | Fast |
| Ops burden | You run and scale it | Vendor-managed |
| Lock-in risk | Low (with OTel) | Higher (proprietary agents) |
| Best for | Platform teams, cost-sensitive scale | Small teams, fast adoption, advanced AI/analytics features |

## Common pitfalls
- Instrumenting late; only dashboards, no traces
- No consistent `service.name`/version/env, so data can't be sliced
- Trace context lost across queues, threads, async boundaries, or proxies
- Alerting on causes (CPU) rather than user impact
- High-cardinality metric labels causing cost/performance blowups
- Sampling too aggressively and missing rare failures
- Siloed tools with no correlation between logs, metrics, and traces
- Ignoring frontend/RUM and only measuring server time
- Sensitive data leaking into telemetry
- Observability for the app but not for the telemetry pipeline itself

## Cheat sheet
| Need | Use |
|------|-----|
| "Is it up and healthy?" | Metrics + SLOs, synthetic checks |
| "What does the user experience?" | RUM, Core Web Vitals, session data |
| "Where is the latency?" | Distributed traces |
| "Why did this specific request fail?" | Logs/events correlated by `trace_id` |
| "Which code is expensive?" | Continuous profiling, flame graphs |
| "What changed?" | Deploy markers, feature-flag events, config diffs |
| "Who is affected?" | High-cardinality attributes, cohort analysis |
| "Are we meeting commitments?" | SLIs/SLOs and error budgets |

## Interview one-liners
- *Observability vs monitoring?* Monitoring checks known failure modes; observability lets you explore unknown ones from rich telemetry.
- *Three pillars?* Logs, metrics, traces; value comes from correlating them (plus profiles and events).
- *What is a span?* One timed operation in a trace, with parent-child links and attributes.
- *Head vs tail sampling?* Head decides up front (cheap, may miss rare issues); tail decides after completion (keeps errors/slow traces, needs buffering).
- *What is OpenTelemetry?* Vendor-neutral standard (API, SDK, OTLP, Collector) for generating and exporting telemetry.
- *What is Apdex?* Satisfaction score from satisfied/tolerating/frustrated response-time buckets.
- *Why p99 over average?* Averages hide tail latency that real users feel.
- *How do you find a bottleneck in microservices?* Service map -> slow traces -> longest span -> logs/profile of that service.
- *How to control observability costs?* Sampling, filtering, cardinality limits, tiered retention, quotas.
