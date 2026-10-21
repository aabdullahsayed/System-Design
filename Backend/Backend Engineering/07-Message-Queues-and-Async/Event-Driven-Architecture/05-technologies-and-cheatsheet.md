# Technologies & Cheat Sheet

## Common technologies
| Category | Options |
|----------|---------|
| Event streaming platforms | Apache Kafka, Redpanda, Apache Pulsar, AWS Kinesis, Azure Event Hubs |
| Message brokers | RabbitMQ, ActiveMQ/Artemis, NATS, Azure Service Bus |
| Managed cloud | AWS SNS + SQS, AWS EventBridge, Google Pub/Sub |
| Stream processing | Kafka Streams, Apache Flink, Spark Structured Streaming, ksqlDB |
| CDC | Debezium, AWS DMS |
| Workflow / orchestration | Temporal, AWS Step Functions, Camunda, Netflix Conductor |
| Event sourcing stores | EventStoreDB, Axon Server, Marten |
| Schema / contract | Confluent Schema Registry, Avro, Protobuf, CloudEvents, AsyncAPI |
| Tracing | OpenTelemetry, Jaeger, Zipkin |

## Choosing a backbone
- Need replay, huge throughput, stream processing: **Kafka / Pulsar / Redpanda**.
- Need flexible routing and task queues: **RabbitMQ**.
- Want zero ops on AWS: **SNS + SQS / EventBridge** (EventBridge for routing between SaaS/AWS events).
- Low-latency lightweight messaging: **NATS**.

## Typical flow example: e-commerce order
1. Order Service saves order + outbox row (one transaction).
2. Relay publishes `OrderPlaced`.
3. Payment Service charges card -> `PaymentCompleted` (or `PaymentFailed`).
4. Inventory reserves stock -> `StockReserved` (or `StockUnavailable`).
5. Shipping creates shipment -> `OrderShipped`.
6. Notification Service emails customer on each relevant event.
7. On failure: compensating events (`StockReleased`, `OrderCancelled`).

## Best-practice checklist
- [ ] Events are past-tense facts with clear business meaning
- [ ] Standard envelope (id, type, version, time, source, correlationId)
- [ ] Schema registry and compatibility rules
- [ ] Outbox or CDC to avoid dual writes
- [ ] Idempotent consumers (inbox/dedupe)
- [ ] Partition keys chosen for ordering needs
- [ ] Retries with backoff + DLQ with replay tooling
- [ ] Tracing with correlation IDs everywhere
- [ ] Monitor lag, DLQ size, event age
- [ ] Minimal PII; encryption and access control per topic
- [ ] Documented event catalog (AsyncAPI)
- [ ] Contract tests between teams

## Common pitfalls
- Using events as remote procedure calls (disguised commands)
- Leaking internal database models as public events
- No schema governance, so consumers break silently
- Assuming ordering or exactly-once delivery
- Building giant choreographies nobody can understand
- Skipping observability
- Event sourcing everywhere when simple state storage would do

## Interview one-liners
- *Event vs command?* Event = something happened (many listeners); command = do this (one handler).
- *Choreography vs orchestration?* Decentralized reactions vs a central coordinator.
- *Event sourcing vs EDA?* EDA is communication style; event sourcing is a persistence style. You can have either without the other.
- *CQRS needs event sourcing?* No, but they combine well.
- *How to handle distributed transactions?* Sagas with compensations, not 2PC.
- *Outbox pattern?* Store event with the state change in one DB transaction; publish afterwards.
