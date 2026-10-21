# Popular Brokers Comparison

| Feature | RabbitMQ | Apache Kafka | Amazon SQS | Redis (Lists/Streams) | NATS | Google Pub/Sub |
|---------|----------|--------------|------------|-----------------------|------|----------------|
| Model | Queue / exchange (AMQP) | Distributed log | Managed queue | In-memory list / log | Lightweight pub/sub | Managed pub/sub |
| Retention | Until acked | Time/size-based, replayable | Up to 14 days | Memory-bound | Optional (JetStream) | Up to 7 days (configurable) |
| Ordering | Per queue | Per partition | FIFO queues only | Per list/stream | Per subject (JetStream) | Optional ordering keys |
| Throughput | Moderate-high | Very high | High (managed) | High | Very high | High |
| Routing flexibility | Excellent | Basic (topic/partition) | Basic | Basic | Subject wildcards | Subscriptions/filters |
| Replay | No (by default) | Yes | No | Streams: yes | JetStream: yes | Seek/replay supported |
| Ops burden | Medium | High | None (managed) | Low | Low | None (managed) |

## When to choose what
- **RabbitMQ**: complex routing, task queues, traditional messaging, per-message acks and priorities.
- **Kafka**: event streaming, high throughput, event sourcing, replay, analytics pipelines.
- **SQS**: simple, fully managed queue on AWS; minimal operations.
- **Redis**: lightweight/simple queues when you already run Redis and can tolerate its durability limits.
- **NATS**: low-latency, simple messaging for microservices/IoT.
- **Google Pub/Sub**: managed global pub/sub on GCP.

## Other notable options
ActiveMQ / Artemis, Apache Pulsar, Azure Service Bus, IBM MQ, ZeroMQ (library, no broker).

## Common job libraries (built on queues)
- Python: Celery, RQ, Dramatiq
- Node.js: BullMQ
- Ruby: Sidekiq
- Java: Spring AMQP / Spring Kafka
- Go: Asynq, Machinery
