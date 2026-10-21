# Logging Fundamentals

## What is logging?
A log is a timestamped record of something that happened in a system. Logs answer **"what happened, when, and why?"** and are the main tool for debugging, auditing, and incident investigation.

## Log levels
| Level | Use for | Example |
|-------|---------|---------|
| **TRACE** | Very fine-grained flow details | Entering function X with args |
| **DEBUG** | Diagnostic info for developers | Cache miss for key `user:42` |
| **INFO** | Normal, notable business/system events | Order 123 placed |
| **WARN** | Unexpected but recoverable | Retry 2/3 for payment API |
| **ERROR** | Operation failed, needs attention | Payment failed: timeout |
| **FATAL / CRITICAL** | App can't continue | DB connection lost, shutting down |

Guidelines:
- Production default: **INFO** (DEBUG enabled temporarily or per-request when needed).
- Make levels changeable at runtime without redeploying.
- Don't log an ERROR for expected conditions (e.g. user typed wrong password = INFO/WARN).

## Structured logging
Prefer machine-readable (JSON) over free text.

```json
{
  "timestamp": "2026-10-21T07:15:02.123Z",
  "level": "ERROR",
  "service": "payment-service",
  "env": "prod",
  "version": "1.8.2",
  "trace_id": "4bf92f3577b34da6",
  "span_id": "00f067aa0ba902b7",
  "user_id": "u_42",
  "message": "Payment failed",
  "error": { "type": "TimeoutError", "message": "gateway timeout after 3000ms" },
  "duration_ms": 3001
}
```

Benefits: easy to search, filter, aggregate, and alert on fields.

## What to log
- Request start/end (method, path, status, duration)
- Business events (order placed, login, permission change)
- Errors with stack trace and context
- External calls (target, latency, outcome)
- Config/startup/shutdown events
- Security events (auth failures, privilege changes)

## What NOT to log
- Passwords, tokens, API keys, secrets
- Full credit card numbers, government IDs
- Unnecessary PII (mask or hash if needed)
- Huge payloads (log size or a reference instead)
- Noise inside tight loops

## Good log messages
- Say what happened and include identifiers: `Failed to charge order o_123 (user u_42): gateway timeout`
- Include context as fields, not string concatenation.
- One event per line; avoid multi-line unless it's a stack trace in a structured field.
- Use consistent field names across services (e.g. `trace_id`, `user_id`).
- Log once at the boundary where you handle an error; don't log and rethrow at every layer.

## Common logging libraries
- Java: SLF4J + Logback / Log4j2
- Python: `logging`, structlog, loguru
- Node.js: pino, winston
- Go: zap, zerolog, slog
- .NET: Serilog, Microsoft.Extensions.Logging
