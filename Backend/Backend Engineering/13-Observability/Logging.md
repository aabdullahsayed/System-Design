# Logging

## What is it?
Recording structured, timestamped records of what a system is doing — the most basic, foundational form of observability, and usually the first place an engineer looks when something goes wrong.

## Why care?
When something breaks in production at 2am, logs are frequently the ONLY window into what actually happened — you can't attach a debugger to a production server serving live traffic. Good logging habits (or the lack of them) directly determine how fast an incident gets resolved.

## Log levels (use them consistently)
```
DEBUG   — detailed diagnostic info, useful only during active development/debugging
INFO    — normal operational events ("Order 12345 created successfully")
WARNING — something unexpected but recoverable ("Retry attempt 2 of 3 for payment API")
ERROR   — something failed and needs attention ("Failed to charge customer: timeout")
CRITICAL/FATAL — the application itself may be unable to continue
```
In production, you typically run at `INFO` level or above (DEBUG is too noisy/expensive at scale) but can temporarily lower the level for a specific troubleshooting session.

## Structured logging (the modern standard)
Instead of unstructured free-text log lines, log as structured JSON — dramatically easier to search, filter, and aggregate at scale.
```python
# BAD (unstructured, hard to search/parse reliably)
print(f"User {user_id} placed order {order_id} for ${amount}")

# GOOD (structured — a log aggregation tool can filter/query on any field)
logger.info("order_placed", extra={
    "user_id": user_id,
    "order_id": order_id,
    "amount": amount,
    "timestamp": datetime.utcnow().isoformat()
})
```

## Centralized log aggregation (essential once you have more than one server)
With multiple servers/containers, logs scattered across each individual machine are nearly useless for debugging a distributed request. Centralized logging tools (the **ELK stack** — Elasticsearch, Logstash, Kibana; or managed alternatives like **Datadog Logs**, **Splunk**, **CloudWatch Logs**) collect logs from every service/instance into one searchable place.

## What NOT to log (a real, serious concern)
**Never log sensitive data**: passwords, full credit card numbers, API keys/secrets, or unredacted personally identifiable information (PII) — a common, serious security/compliance mistake. Mask or omit sensitive fields explicitly:
```python
logger.info("payment_processed", extra={"card_last4": card_number[-4:]})  # never log the full number
```

## Correlation IDs (essential in distributed systems)
Attach a unique **request ID** (correlation ID) to every incoming request, and propagate it through every downstream service call and log line related to that request — lets you reconstruct the FULL story of a single request across multiple services by searching for one ID, instead of trying to manually correlate timestamps across separate services' logs.

## Practical example
A customer reports "my payment failed but I got charged." Searching centralized logs by the request's correlation ID reveals the exact sequence: payment API call succeeded (charge went through), but the subsequent order-confirmation call timed out due to a network blip — pinpointing the exact bug (a missing retry/idempotency safeguard) in under a minute, something that would be nearly impossible to diagnose from the customer's report alone without proper structured, correlated logging.
