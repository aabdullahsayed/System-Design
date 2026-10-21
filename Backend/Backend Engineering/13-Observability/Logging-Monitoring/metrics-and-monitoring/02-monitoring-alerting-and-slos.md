# Monitoring, Alerting & SLOs

## Monitoring vs Observability
- **Monitoring**: watching known signals for known failure modes ("is CPU high?").
- **Observability**: ability to ask new questions about unknown problems using logs, metrics, and traces ("why are checkout requests from region X slow?").

## Types of monitoring
- **Infrastructure monitoring**: hosts, VMs, containers, network.
- **Application Performance Monitoring (APM)**: latency, errors, traces, dependencies.
- **Synthetic monitoring**: scripted probes simulating users (uptime, login flow).
- **Real User Monitoring (RUM)**: actual browser/mobile performance.
- **Log monitoring**: error patterns, security events.
- **Business monitoring**: orders/min, revenue, conversion.
- **Security monitoring**: anomalies, auth failures, audit trails.

## SLI, SLO, SLA, Error Budget
| Term | Meaning | Example |
|------|---------|---------|
| **SLI** (indicator) | A measured quality of service | % of requests succeeding under 300ms |
| **SLO** (objective) | Target for an SLI | 99.9% of requests succeed over 30 days |
| **SLA** (agreement) | Contract with consequences | 99.5% uptime or service credits |
| **Error budget** | Allowed unreliability = 100% - SLO | 0.1% of 30 days ≈ 43 min of downtime |

Uses: when the error budget is burned, slow feature releases and focus on reliability; when plenty remains, ship faster.

### Availability math
| Availability | Downtime per year | Per 30 days |
|--------------|------------------|-------------|
| 99% | ~3.65 days | ~7.2 hours |
| 99.9% | ~8.76 hours | ~43 minutes |
| 99.99% | ~52.6 minutes | ~4.3 minutes |
| 99.999% | ~5.26 minutes | ~26 seconds |

## Alerting principles
- **Alert on symptoms (user impact), not causes.** "Error rate > 2% for 5 min" beats "CPU > 80%".
- Every alert should be **actionable** and urgent; otherwise make it a dashboard or ticket.
- Avoid alert fatigue: tune thresholds, add `for:` durations, deduplicate, group.
- Include context: what's wrong, impact, dashboard link, runbook link, recent deploys.
- Use severity: **page** (wake someone) vs **ticket** (next business day) vs **info**.

### Burn-rate alerts (SLO-based)
Alert when the error budget burns too fast, e.g. fast burn (2% of monthly budget in 1 hour) pages; slow burn (10% in 3 days) creates a ticket. Use multi-window to cut noise.

### Static vs dynamic thresholds
- Static: simple, predictable (disk > 90%).
- Dynamic/anomaly detection: handles seasonality, but can be noisy and harder to explain.

## Example alert rules (Prometheus style)
```yaml
- alert: HighErrorRate
  expr: sum(rate(http_requests_total{status=~"5.."}[5m]))
        / sum(rate(http_requests_total[5m])) > 0.02
  for: 5m
  labels: { severity: page }
  annotations:
    summary: "5xx error rate above 2% for 5 minutes"
    runbook: "https://runbooks.example.com/high-error-rate"

- alert: HighP99Latency
  expr: histogram_quantile(0.99, sum by (le) (rate(http_request_duration_seconds_bucket[5m]))) > 1
  for: 10m
  labels: { severity: ticket }
```

## Dashboards
- Start with a **service overview**: Golden Signals (traffic, errors, latency, saturation) at the top.
- Then dependencies, resources, and business KPIs.
- Show deploy markers/annotations on graphs.
- Few, purposeful panels beat walls of charts; label units and thresholds.
- Link from alert -> dashboard -> logs -> traces.

## Health checks
- **Liveness**: is the process alive (restart if not)?
- **Readiness**: can it take traffic (remove from load balancer if not)?
- **Startup**: for slow-starting apps.
- Make health checks cheap; deep dependency checks belong in readiness with care to avoid cascading failures.
