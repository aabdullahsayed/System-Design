# Logging Best Practices & Cheat Sheet

## Do
- Use **structured JSON** logs with consistent field names.
- Include `timestamp` (UTC, ISO-8601), `level`, `service`, `env`, `version`, `trace_id`.
- Log to **stdout** in containers and let the platform ship logs.
- Propagate and log correlation/trace IDs.
- Use appropriate levels; keep INFO meaningful and low-noise.
- Log exceptions with stack traces and context.
- Make log levels adjustable at runtime.
- Test that logging doesn't break the app (async appenders, non-blocking).
- Alert on log patterns only when metrics can't do it better.

## Don't
- Log secrets, tokens, or sensitive PII.
- Log inside hot loops or at very high volume without sampling.
- Log and rethrow at every layer (duplicate noise).
- Use logs as your only monitoring; logs are not a substitute for metrics.
- Write unstructured, inconsistent messages ("error!!!" with no context).
- Let logging block request threads (use async I/O where needed).
- Keep everything forever.

## Performance tips
- Use lazy/parameterized logging (`log.debug("x={}", x)`), not string concatenation.
- Guard expensive debug computations with a level check.
- Prefer async/buffered appenders; bound queues and decide drop policy.
- Avoid synchronous network logging in the request path.

## Audit logs vs application logs
| | Application logs | Audit logs |
|---|---|---|
| Purpose | Debugging/operations | Who did what, when (compliance) |
| Retention | Short/medium | Long, often legally mandated |
| Integrity | Best effort | Tamper-evident, immutable |
| Access | Engineers | Restricted (security/compliance) |

## Logs vs Metrics vs Traces
| Signal | Best for | Cost | Cardinality tolerance |
|--------|----------|------|-----------------------|
| **Logs** | Detailed "why" for individual events | High at volume | High (any field) |
| **Metrics** | Trends, SLOs, alerting ("is something wrong?") | Low | Must be limited |
| **Traces** | Request path/latency across services | Medium (sampled) | Medium |

## Quick checklist
- [ ] JSON structured, consistent fields
- [ ] Trace ID in every line
- [ ] No secrets/PII
- [ ] Centralized with defined retention
- [ ] Log level controllable at runtime
- [ ] Sampling/limits for noisy logs
- [ ] Alerts and dashboards for error rates
- [ ] Rotation or stdout-only (no disk fill)

## Interview one-liners
- *Why structured logging?* Machine-parseable fields allow searching, aggregating, and alerting.
- *How to trace a request across services?* Propagate a trace/correlation ID and log it everywhere.
- *Logs vs metrics?* Metrics tell you something is wrong; logs help explain why.
- *How to control logging cost?* Levels, sampling, drop noise, tiered retention.
