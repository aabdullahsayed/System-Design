# Core Concepts

| Term | Meaning |
|------|---------|
| **Producer / Publisher** | Application that sends messages |
| **Consumer / Subscriber** | Application that receives and processes messages |
| **Broker** | Server that stores and routes messages |
| **Queue** | Ordered buffer holding messages until consumed |
| **Topic** | Named channel for publish/subscribe; many subscribers can receive each message |
| **Exchange** (AMQP) | Routes messages to queues based on rules (direct, fanout, topic, headers) |
| **Routing key** | Label used by an exchange to decide where a message goes |
| **Message** | Payload + headers/metadata (id, timestamp, correlation id, TTL) |
| **Acknowledgement (ack)** | Consumer tells broker the message was processed successfully |
| **Nack / Reject** | Consumer signals failure; message may be requeued or dead-lettered |
| **Consumer group** | Set of consumers that share the work of a topic/queue (e.g. Kafka) |
| **Partition** | Subdivision of a topic enabling parallelism and per-key ordering |
| **Offset** | Position of a consumer within a partition/log |
| **Dead Letter Queue (DLQ)** | Holding queue for messages that repeatedly fail or expire |
| **TTL** | Time-to-live; message expires if not consumed in time |
| **Prefetch / QoS** | How many unacked messages a consumer may hold at once |

## Message lifecycle
1. Producer publishes message
2. Broker persists (optionally) and routes it to a queue
3. Consumer receives message
4. Consumer processes it
5. Consumer acks, and broker removes it (or advances offset)
6. On failure: retry, requeue, or send to DLQ

## Queue vs. log
- **Queue model** (RabbitMQ, SQS): messages are removed once acked.
- **Log model** (Kafka, Pulsar, Redis Streams): messages are retained for a period; consumers track their own position and can replay.
