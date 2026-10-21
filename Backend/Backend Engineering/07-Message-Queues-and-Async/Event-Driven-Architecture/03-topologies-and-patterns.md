# Topologies & Patterns

## Two classic topologies

### Broker topology (choreography)
No central controller. Each service reacts to events and emits new ones.
```
OrderPlaced -> Payment -> PaymentCompleted -> Inventory -> StockReserved -> Shipping
```
- Pros: highly decoupled, scalable, simple pieces.
- Cons: flow is implicit and hard to see; hard to manage errors and timeouts; risk of cyclic dependencies.

### Mediator topology (orchestration)
A central orchestrator receives the initial event and commands each step.
```
Orchestrator -> ReserveStock -> ChargeCard -> ArrangeShipping
```
- Pros: explicit workflow, easier error handling and monitoring.
- Cons: orchestrator can become a bottleneck / coupling point.

**Rule of thumb**: choreography for simple, loosely related reactions; orchestration for complex, business-critical workflows.

## Core patterns

### 1. Publish/Subscribe
Producers publish to a topic; any number of subscribers react.

### 2. Event Notification
Emit a small event saying "something happened"; interested services decide what to do.

### 3. Event-Carried State Transfer
Event contains the data so consumers maintain local copies and avoid synchronous calls.

### 4. Event Sourcing
Store **every state change as an event** instead of current state; rebuild state by replaying.
- Pros: full audit, time travel, easy projections.
- Cons: complexity, snapshots needed, event schema evolution, querying needs projections.

### 5. CQRS (Command Query Responsibility Segregation)
Separate write model (commands) from read models (queries/projections), often fed by events. Pairs well with event sourcing, but each can be used alone.

### 6. Saga
A long-running business transaction as local transactions + events, with **compensating actions** on failure.
- Choreographed saga: services react to each other's events.
- Orchestrated saga: a coordinator drives steps and compensations.
Example: Order -> Reserve stock -> Charge payment -> (payment fails) -> Release stock -> Cancel order.

### 7. Transactional Outbox
Save the event in an `outbox` table in the same DB transaction as the state change; a relay (poller or CDC) publishes it. Solves the dual-write problem.

### 8. Inbox / Idempotent Consumer
Record processed event IDs to ignore duplicates.

### 9. Change Data Capture (CDC)
Stream DB changes (Debezium) as events without modifying application code.

### 10. Event Streaming / Stream Processing
Continuous processing of event streams (Kafka Streams, Flink): filtering, joining, windowing, aggregations.

### 11. Claim Check
Put large payloads in blob storage; the event carries a reference.

### 12. Dead Letter Queue + Retry
Retry with backoff; park poison events in a DLQ for inspection and replay.

### 13. Strangler via events
Gradually migrate a monolith by having it emit events that new services consume.
