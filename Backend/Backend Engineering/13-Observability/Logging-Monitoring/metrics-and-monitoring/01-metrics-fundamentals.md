# Metrics Fundamentals

## What are metrics?
Metrics are numeric measurements collected over time (time series). They answer **"is the system healthy, and how is it trending?"** They are cheap to store, fast to query, and ideal for dashboards and alerts.

A data point = **name + labels/tags + value + timestamp**
```
http_requests_total{service="api", method="GET", status="200"} 1027 @ 1729494900
```

## Metric types
| Type | Behavior | Examples |
|------|----------|----------|
| **Counter** | Only goes up (resets on restart) | Requests served, errors, bytes sent |
| **Gauge** | Goes up and down | Memory used, queue depth, active connections |
| **Histogram** | Distribution in buckets (can compute percentiles) | Request latency, payload size |
| **Summary** | Client-side calculated quantiles | Latency p50/p90/p99 (not aggregatable across instances) |

Tip: for counters, graph the **rate** (per second), not the raw total.

## Percentiles, not averages
Averages hide slow outliers. Track **p50, p90, p95, p99** latency. One user in a hundred hitting 5s is invisible in a 120ms average.

## Labels and cardinality
- Labels allow slicing (`service`, `endpoint`, `status`, `region`).
- **High cardinality** (user ID, request ID, full URL with IDs) explodes series count, memory, and cost. Keep label values bounded.
- Put high-cardinality detail in logs/traces, not metric labels.

## Frameworks for choosing metrics

### The Four Golden Signals (Google SRE)
1. **Latency**: time to serve a request (separate successful and failed requests)
2. **Traffic**: demand on the system (requests/sec)
3. **Errors**: rate of failed requests
4. **Saturation**: how "full" the system is (CPU, memory, queue, connections)

### RED method (for services/request-driven)
- **R**ate, **E**rrors, **D**uration

### USE method (for resources: CPU, disk, network)
- **U**tilization, **S**aturation, **E**rrors

### Business / product metrics
Signups, orders/min, checkout conversion, revenue, payment success rate. Monitoring should reflect user-facing outcomes, not only machine health.

## Metrics by layer
| Layer | Examples |
|-------|----------|
| Infrastructure | CPU, memory, disk I/O, disk space, network throughput |
| Containers/K8s | Pod restarts, OOMKills, CPU throttling, node pressure |
| Application/runtime | Request rate, latency, error rate, GC pauses, thread pool usage, heap |
| Dependencies | DB query latency, connection pool usage, cache hit ratio, queue lag |
| Business | Orders, signups, conversion, failed payments |
| Frontend/RUM | Page load, Core Web Vitals, JS errors |

## Push vs Pull collection
- **Pull** (Prometheus scrapes `/metrics`): central control, easy health check of targets.
- **Push** (StatsD, OTel exporters, Pushgateway): good for short-lived jobs, firewalled networks.
