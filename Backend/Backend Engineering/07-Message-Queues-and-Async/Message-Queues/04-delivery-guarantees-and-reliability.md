# Delivery Guarantees & Reliability

## Delivery semantics
| Semantics | Behavior | Risk |
|-----------|----------|------|
| **At-most-once** | Send and forget; no retry | Messages can be lost |
| **At-least-once** | Retry until acked | Duplicates possible (most common default) |
| **Exactly-once** | Processed once in effect | Hard; usually achieved via idempotency + transactions |

> In practice: **design for at-least-once delivery and make consumers idempotent.**

## Idempotency
Processing the same message twice must have the same effect as once. Techniques:
- Store processed message IDs (dedup table) and skip repeats
- Use natural idempotent operations (upsert, set value)
- Use idempotency keys on downstream API calls
- Conditional updates with version numbers

## Acknowledgements
- **Auto-ack**: fast but can lose messages if consumer crashes mid-processing.
- **Manual ack**: ack *after* successful processing (safer).
- Publisher confirms: broker confirms it has received/persisted the message.

## Durability
- Durable queues + persistent messages survive broker restart.
- Replication (mirrored/quorum queues, Kafka replication factor) survives node loss.
- Use `acks=all` (Kafka) / quorum writes for stronger guarantees.

## Ordering
- A single queue with one consumer preserves order.
- Multiple competing consumers can break ordering.
- Kafka guarantees order **within a partition**; use a partition key (e.g. `user_id`) to keep related messages together.

## Failure handling
- **Retries** with exponential backoff and jitter
- **Poison messages**: messages that always fail; cap retries and send to DLQ
- **Timeouts / visibility timeout** (SQS): message becomes visible again if not deleted in time
- **Backpressure**: limit prefetch, scale consumers, or throttle producers

## Monitoring checklist
- Queue depth / consumer lag
- Age of oldest message
- Publish and consume rates
- Error and DLQ counts
- Consumer count and processing latency
