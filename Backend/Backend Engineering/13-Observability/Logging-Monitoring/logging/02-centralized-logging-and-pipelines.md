# Centralized Logging & Pipelines

## Why centralize?
In distributed systems (containers, autoscaling, many services) local log files disappear and are impossible to search across machines. Ship logs to a central, searchable store.

## Typical pipeline
```
App -> stdout/file -> Agent/Collector -> (Buffer/Queue) -> Processor -> Storage/Index -> UI/Alerts
```

| Stage | Purpose | Tools |
|-------|---------|-------|
| **Emit** | Apps write logs (prefer **stdout** in containers, 12-factor) | App libraries |
| **Collect/Ship** | Read and forward logs | Fluent Bit, Fluentd, Filebeat, Vector, OpenTelemetry Collector, Promtail |
| **Buffer** | Absorb spikes, decouple | Kafka, Redis, Kinesis |
| **Process** | Parse, enrich, filter, mask | Logstash, Vector, Fluentd, OTel Collector |
| **Store/Index** | Search and retain | Elasticsearch/OpenSearch, Loki, Splunk, ClickHouse, CloudWatch Logs, Datadog |
| **Visualize/Query** | Search and dashboards | Kibana, Grafana, Splunk UI |

## Popular stacks
- **ELK/EFK**: Elasticsearch + Logstash/Fluentd + Kibana. Powerful full-text search; heavier to operate.
- **Grafana Loki**: indexes only labels, stores log lines cheaply; pairs with Grafana, Prometheus-like model.
- **Splunk / Datadog / New Relic / Cloud-native** (CloudWatch, Cloud Logging, Azure Monitor): managed, convenient, can be costly at volume.

## Enrichment
Add fields at the collector: `service`, `env`, `host`, `pod`, `namespace`, `region`, `version`. Kubernetes metadata is typically added automatically by the agent.

## Correlation
- Generate a **trace/correlation ID** at the edge and propagate via headers (`traceparent` from W3C Trace Context).
- Include it in every log line so you can pull all logs for one request across services.
- Link logs <-> traces <-> metrics in your observability tool.

## Retention, cost, and volume
- Define retention by value: e.g. 7-14 days hot searchable, 30-90 days warm, archive to cheap object storage (S3/GCS) for compliance.
- Use **sampling** for noisy, low-value logs (e.g. keep 1-10% of successful health checks).
- Drop or downgrade noisy logs at the collector.
- Set per-service quotas; watch ingestion volume (a common surprise bill).
- Compress and partition by time.

## Security & compliance
- Redact secrets/PII at source and again in the pipeline.
- Encrypt in transit (TLS) and at rest.
- Restrict access (RBAC); audit who queries logs.
- Make audit logs **append-only / tamper-evident**; keep separate from app logs.
- Respect GDPR/retention rules (right to erasure may affect logs containing PII).

## Log rotation (for file-based logs)
- Rotate by size or time; compress old files; cap total disk (logrotate, Logback rolling policy).
- Prevent disks from filling, which is a classic outage cause.

## Troubleshooting with logs: a workflow
1. Identify the time window and affected service from an alert.
2. Filter by `level=ERROR` and service; look for the first error.
3. Grab a `trace_id` from one failing request.
4. Query all services' logs by that `trace_id`.
5. Find the root cause (upstream timeout, bad deploy, bad data).
6. Compare with the last deploy/config change.
