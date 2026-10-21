# Consistency, Reliability & Challenges

## Eventual consistency
Different services see updates at slightly different times. Design UX and logic for it:
- Show "processing" states; don't assume instant updates.
- Use read-your-writes tricks (return the new state in the response, or poll/subscribe).
- Reconcile periodically to repair drift.

## Delivery guarantees
- **At-most-once**: may lose events.
- **At-least-once**: may duplicate (typical default).
- **Exactly-once**: effectively achieved with idempotency + transactions (e.g. Kafka transactions).

> Design for **at-least-once + idempotent consumers**.

## Idempotency
- Dedupe by event ID (inbox table).
- Use upserts and conditional updates.
- Pass idempotency keys to external calls (payments, emails).

## Ordering
- Order is only guaranteed within a partition/queue/key.
- Use entity IDs as keys; avoid requiring global ordering.
- Handle out-of-order events with version numbers or timestamps (ignore stale updates).

## Dual-write problem
Writing to a DB and publishing to a broker are two operations that can partially fail. Fix with the **outbox pattern** or **CDC** (or event sourcing).

## Failure handling
- Retries with exponential backoff and jitter.
- Dead letter queues for poison events.
- Timeouts and circuit breakers on downstream calls.
- **Compensating actions** (sagas) instead of distributed transactions.
- Replay capability to recover from bugs: reprocess from an offset.

## Observability
- **Correlation ID** across the whole flow; **causation ID** for the parent event.
- Distributed tracing (OpenTelemetry) propagated in event headers.
- Metrics: consumer lag, event age, throughput, error/DLQ rate.
- Structured logs with event ID and type.
- Event catalog / flow diagrams to make implicit flows visible.

## Testing
- Unit test handlers with sample events.
- **Contract tests** between producer and consumer schemas.
- Integration tests with a real broker (Testcontainers).
- Test duplicates, out-of-order, and replayed events.
- Chaos testing for broker/consumer failures.

## Common challenges and mitigations
| Challenge | Mitigation |
|-----------|-----------|
| Hard to follow the flow | Event catalog, tracing, docs (e.g. AsyncAPI) |
| Schema breakage | Registry, compatibility checks, versioning |
| Duplicate processing | Idempotent consumers |
| Event storms / cascading events | Rate limits, backpressure, avoid cycles |
| Data duplication across services | Accept it; define a single source of truth |
| Security / PII in events | Minimize data, encrypt, ACLs per topic |
| Poison messages | Retry caps + DLQ |
| Big replay cost | Snapshots, compaction, retention policy |
