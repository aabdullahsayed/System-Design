# Distributed Tracing & OpenTelemetry

## Distributed tracing concepts
- **Trace**: the full journey of one request through the system (identified by `trace_id`).
- **Span**: a single operation within a trace (DB query, HTTP call, function). Has: name, start/end time, `span_id`, `parent_span_id`, attributes, events, status, links.
- **Context propagation**: passing trace context between services via headers/metadata so spans join the same trace.
- **Baggage**: key-value pairs propagated with the trace (use sparingly; can leak data/size).

```
Trace 4bf92f35...
└─ GET /checkout                       (api-gateway)      480 ms
   ├─ auth.verify                      (auth-service)      20 ms
   ├─ POST /orders                     (order-service)    410 ms
   │  ├─ SELECT inventory              (postgres)          35 ms
   │  ├─ POST /charge                  (payment-service)  310 ms   <-- bottleneck
   │  │   └─ HTTP stripe.com/charges   (external)         295 ms
   │  └─ publish OrderPlaced           (kafka)             12 ms
   └─ render                           (api-gateway)        8 ms
```

## W3C Trace Context
```
traceparent: 00-4bf92f3577b34da6a3ce929d0e0e4736-00f067aa0ba902b7-01
             ver  trace-id (16 bytes)            parent span-id   flags (sampled)
tracestate: vendor-specific data
```
Propagate across HTTP, gRPC, and messaging (put context in message headers). Async/queue hops often use **span links** instead of parent-child.

## Span attributes: useful semantic conventions
- HTTP: `http.request.method`, `http.response.status_code`, `url.path`/`http.route`
- DB: `db.system`, `db.operation.name`, `db.query.text` (sanitize!)
- Messaging: `messaging.system`, `messaging.destination.name`
- Service: `service.name`, `service.version`, `deployment.environment`
- Business: `user.tier`, `order.id` (avoid PII; watch cardinality in metrics derived from spans)

## Sampling strategies
| Strategy | Decision point | Pros | Cons |
|----------|----------------|------|------|
| **Head-based** | At trace start (e.g. 10%) | Cheap, simple | May miss rare errors/slow traces |
| **Tail-based** | After trace completes (in collector) | Keep all errors/slow traces | Needs buffering; more infra |
| **Adaptive / rate-limited** | Dynamic per service/route | Controls cost | Complexity |
| **Always-on for errors** | Keep every error trace | Debuggability | Spikes in volume |

Typical policy: keep 100% of errors and slow traces, ~1-10% of normal traffic.

## OpenTelemetry (OTel)
CNCF project providing vendor-neutral **APIs, SDKs, semantic conventions, protocol (OTLP), and a Collector** for metrics, logs, and traces.

### Components
| Component | Role |
|-----------|------|
| **API** | What app/library code calls (stable, no-op by default) |
| **SDK** | Implements API: sampling, processors, exporters |
| **Auto-instrumentation** | Agents/plugins for common frameworks and libraries |
| **OTLP** | Standard wire protocol (gRPC/HTTP) |
| **Collector** | Receives, processes, exports telemetry (receivers -> processors -> exporters) |
| **Semantic conventions** | Standard attribute names |

### Collector deployment patterns
- **Agent** (per host/DaemonSet): low-latency local collection, enrichment.
- **Gateway** (central cluster): batching, tail sampling, routing, auth, multi-backend export.
- Often both: app -> agent -> gateway -> backend(s).

### Collector config sketch
```yaml
receivers:
  otlp:
    protocols: { grpc: {}, http: {} }
processors:
  batch: {}
  memory_limiter: { check_interval: 1s, limit_mib: 512 }
  attributes:
    actions:
      - key: http.request.header.authorization
        action: delete
exporters:
  otlp/tempo: { endpoint: tempo:4317, tls: { insecure: true } }
  prometheus: { endpoint: 0.0.0.0:8889 }
service:
  pipelines:
    traces:  { receivers: [otlp], processors: [memory_limiter, attributes, batch], exporters: [otlp/tempo] }
    metrics: { receivers: [otlp], processors: [memory_limiter, batch], exporters: [prometheus] }
```

### Manual instrumentation (Python)
```python
from opentelemetry import trace
tracer = trace.get_tracer("checkout-service")

def charge(order):
    with tracer.start_as_current_span("charge_card") as span:
        span.set_attribute("order.id", order.id)
        span.set_attribute("payment.provider", "stripe")
        try:
            return gateway.charge(order)
        except Exception as e:
            span.record_exception(e)
            span.set_status(trace.Status(trace.StatusCode.ERROR))
            raise
```

### Auto-instrumentation examples
```bash
# Python
pip install opentelemetry-distro opentelemetry-exporter-otlp
opentelemetry-bootstrap -a install
OTEL_SERVICE_NAME=orders OTEL_EXPORTER_OTLP_ENDPOINT=http://collector:4317 \
  opentelemetry-instrument python app.py

# Java
java -javaagent:opentelemetry-javaagent.jar \
  -Dotel.service.name=orders -Dotel.exporter.otlp.endpoint=http://collector:4317 -jar app.jar
```

## Correlating signals
- Inject `trace_id`/`span_id` into **logs** (OTel log appenders do this).
- Use **exemplars** to link a metric data point (e.g. p99 latency bucket) to a sample trace.
- Link **profiles** to spans to see the hot code for a slow span.
- Add deploy/version/feature-flag attributes so you can compare cohorts.
