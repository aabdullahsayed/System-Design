# Tools & Stack

## Metrics and monitoring tools
| Category | Tools |
|----------|-------|
| Time-series DB / collection | **Prometheus**, VictoriaMetrics, Thanos, Mimir, InfluxDB, Graphite |
| Dashboards | **Grafana**, Kibana, vendor UIs |
| Alert routing / on-call | Alertmanager, PagerDuty, Opsgenie, Grafana OnCall |
| Instrumentation standard | **OpenTelemetry** (metrics, logs, traces), Micrometer (Java), prom-client (Node), prometheus_client (Python) |
| Infra exporters | node_exporter, cAdvisor, kube-state-metrics, blackbox_exporter |
| Full-stack SaaS | Datadog, New Relic, Dynatrace, Splunk Observability, Honeycomb |
| Cloud-native | AWS CloudWatch, Azure Monitor, Google Cloud Monitoring |
| Tracing | Jaeger, Tempo, Zipkin |
| Uptime / synthetic | Pingdom, UptimeRobot, Checkly, Grafana Synthetic Monitoring |
| Error tracking | Sentry, Rollbar |

## Classic open-source stack
```
App (/metrics) -> Prometheus (scrape + store) -> Alertmanager -> PagerDuty/Slack
                          |
                          +-> Grafana (dashboards)
Logs   -> Loki ----------+
Traces -> Tempo/Jaeger --+
```
Long-term storage / scale: Thanos, Mimir, or VictoriaMetrics.

## Prometheus quick reference
- Data model: metric name + labels; pull-based scraping on `/metrics`.
- Query language **PromQL**:
```promql
# requests per second over 5 minutes
sum(rate(http_requests_total[5m])) by (service)

# error ratio
sum(rate(http_requests_total{status=~"5.."}[5m]))
/ sum(rate(http_requests_total[5m]))

# p95 latency from histogram
histogram_quantile(0.95, sum by (le) (rate(http_request_duration_seconds_bucket[5m])))

# memory usage per pod
container_memory_working_set_bytes{namespace="prod"}
```
- Key concepts: `rate()` for counters, `increase()`, `histogram_quantile()`, `sum by`, `for:` in alerts, recording rules to precompute expensive queries.

## Instrumentation example (Python)
```python
from prometheus_client import Counter, Histogram, start_http_server

REQUESTS = Counter("http_requests_total", "Total requests", ["method", "status"])
LATENCY = Histogram("http_request_duration_seconds", "Request latency", ["endpoint"])

def handle(request):
    with LATENCY.labels(endpoint=request.path).time():
        response = process(request)
    REQUESTS.labels(method=request.method, status=response.status).inc()
    return response

start_http_server(8000)  # exposes /metrics
```

## Observability with OpenTelemetry
- Vendor-neutral APIs/SDKs for metrics, logs, and traces.
- **OTel Collector** receives, processes, and exports telemetry to any backend.
- Use **semantic conventions** for consistent attribute names.
- Correlate signals through shared `trace_id`.

## Distributed tracing in one minute
- A **trace** = end-to-end request journey; a **span** = one operation within it.
- Context propagated via headers (W3C `traceparent`).
- Use **sampling** (head-based or tail-based) to control cost; keep errors and slow traces.
- Great for finding which service/call causes latency.
