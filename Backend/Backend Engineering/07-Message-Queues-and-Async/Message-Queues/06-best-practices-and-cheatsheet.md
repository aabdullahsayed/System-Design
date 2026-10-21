# Best Practices & Cheat Sheet

## Design
- Keep messages **small**; use the claim-check pattern for big payloads.
- Include `message_id`, `correlation_id`, `timestamp`, and a **schema version**.
- Use a well-defined schema (JSON Schema, Avro, Protobuf) and plan for evolution (backward/forward compatible changes).
- One message = one clear intent: **command** ("SendEmail") vs **event** ("OrderPlaced").
- Name queues/topics consistently, e.g. `service.entity.action`.

## Producers
- Use publisher confirms.
- Use the outbox pattern to avoid dual writes.
- Retry publishing with backoff.

## Consumers
- Ack **after** processing.
- Make handlers **idempotent**.
- Set sensible prefetch (start low, tune up).
- Handle poison messages: max retries, then DLQ.
- Keep handlers short; offload long work or extend timeouts.
- Shut down gracefully (finish in-flight messages, then exit).

## Operations
- Monitor queue depth, consumer lag, age of oldest message, DLQ size.
- Alert when lag or DLQ grows.
- Set TTLs and max queue lengths to prevent runaway growth.
- Secure the broker: TLS, authentication, per-service permissions.
- Plan capacity and test failover.
- Provide tooling to inspect and replay DLQ messages.

## Common pitfalls
- Assuming exactly-once delivery
- No DLQ, so poison messages block the queue forever
- Auto-ack with long-running work
- Unbounded queues causing memory/disk exhaustion
- Relying on ordering with multiple consumers
- Tight coupling through message structure (changing fields without versioning)
- Using a queue where a simple function call or DB would do

## Quick decision guide
- Need background jobs / work distribution? **Work queue** (RabbitMQ, SQS, BullMQ, Celery).
- Need many services reacting to the same event? **Pub/Sub** (Kafka, SNS, Pub/Sub).
- Need replay / event history / huge throughput? **Log-based** (Kafka, Pulsar).
- Need sync-style response? Consider plain HTTP/gRPC before request/reply over MQ.

## Interview one-liners
- *Queue vs topic?* Queue: one consumer per message. Topic: every subscriber gets it.
- *How to avoid duplicates?* You can't fully; make consumers idempotent.
- *What is a DLQ?* Where undeliverable/failed messages go for later analysis.
- *Kafka vs RabbitMQ?* Kafka = replayable log, high throughput; RabbitMQ = flexible routing, per-message queue semantics.
