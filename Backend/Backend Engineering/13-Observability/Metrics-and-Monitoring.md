# Metrics & Monitoring

## What is it?
**Metrics** are numeric measurements of system behavior over time (requests per second, error rate, CPU usage, response latency) — aggregated, sampled data, in contrast to logs' detailed, per-event records. **Monitoring** is the practice of continuously collecting these metrics and alerting when they cross concerning thresholds.

## Why care?
Metrics let you detect problems (and trends) BEFORE they become customer-visible incidents, and give you the quantitative picture logs alone can't easily provide (you can't easily compute "average response time over the last hour" by eyeballing log lines at scale).

## The four golden signals (Google SRE's widely-adopted framework)
1. **Latency**: how long requests take (track percentiles — p50, p95, p99 — not just averages, since averages hide tail-latency problems that a subset of real users actually experience).
2. **Traffic**: how much demand the system is receiving (requests/sec).
3. **Errors**: rate of failed requests (5xx errors, exceptions).
4. **Saturation**: how "full" the system is (CPU, memory, disk, connection pool usage) — a leading indicator of impending problems before they cause visible errors.

## Common metrics tools
- **Prometheus**: an open-source metrics collection and time-series database, the de facto standard in the Kubernetes/cloud-native ecosystem — applications expose a `/metrics` endpoint that Prometheus periodically "scrapes."
- **Grafana**: the standard visualization layer paired with Prometheus (and many other data sources) — builds dashboards from metrics data.
- **Managed alternatives**: Datadog, New Relic, CloudWatch (AWS) — provide both collection and visualization in one integrated product.

## Instrumenting your own code
```python
from prometheus_client import Counter, Histogram

request_count = Counter('http_requests_total', 'Total HTTP requests', ['method', 'endpoint', 'status'])
request_latency = Histogram('http_request_duration_seconds', 'Request latency')

@request_latency.time()
def handle_request(request):
    # ... handle the request ...
    request_count.labels(method='GET', endpoint='/orders', status='200').inc()
```

## Alerting (turning metrics into action)
Define thresholds that trigger a page/notification when crossed — e.g., "alert if error rate exceeds 5% over 5 minutes" or "alert if p99 latency exceeds 2 seconds." **Alert fatigue is a real, serious problem**: too many low-value alerts cause engineers to start ignoring them (including important ones) — good alerting design means alerting on symptoms that genuinely require human action, not every minor blip.

## SLIs, SLOs, and SLAs (a common framework, worth knowing the terms)
- **SLI** (Service Level Indicator): the actual measured metric (e.g., "99.95% of requests succeeded last month").
- **SLO** (Service Level Objective): your internal target for that metric (e.g., "we aim for 99.9% success rate").
- **SLA** (Service Level Agreement): an external, often contractual promise to customers (e.g., "99.5% uptime guaranteed, or you get a service credit") — usually set with more buffer than the internal SLO.

## Practical example
A dashboard showing p50/p95/p99 latency, request rate, and error rate for a checkout API, with an alert configured for "p99 latency > 3 seconds for 5+ minutes." During a traffic spike, p99 latency creeps up and the alert fires 10 minutes BEFORE customers start actually reporting timeouts — giving the on-call engineer a head start to scale up capacity proactively, rather than reactively scrambling after customers are already affected.
