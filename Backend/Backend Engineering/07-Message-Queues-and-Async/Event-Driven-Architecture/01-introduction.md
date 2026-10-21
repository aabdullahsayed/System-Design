# Event-Driven Architecture: Introduction

## What is EDA?
Event-Driven Architecture (EDA) is a style where components communicate by **producing and reacting to events** rather than calling each other directly. An **event** is an immutable record that something happened ("OrderPlaced", "PaymentFailed").

## Building blocks
- **Event producer**: detects a state change and emits an event.
- **Event channel / broker**: transports events (Kafka, RabbitMQ, SNS/SQS, EventBridge, Pub/Sub).
- **Event consumer**: subscribes and reacts.
- **Event store / log** (optional): durable history of events.

```
[Order Service] --OrderPlaced--> [ Broker ] --> [Payment Service]
                                          --> [Inventory Service]
                                          --> [Email Service]
```

## Request-driven vs event-driven
| | Request/Response | Event-Driven |
|---|---|---|
| Coupling | Caller knows callee | Producer doesn't know consumers |
| Timing | Synchronous | Mostly asynchronous |
| Failure | Caller blocked/fails | Events buffered, retried |
| Adding a consumer | Change the caller | Just subscribe |
| Consistency | Immediate | Eventual |

## Benefits
- Loose coupling and independent deployment
- Scalability and elasticity per consumer
- Resilience through buffering and retries
- Easy extensibility (new subscribers without touching producers)
- Real-time reactions and natural audit trail

## Drawbacks
- Eventual consistency
- Harder debugging and end-to-end tracing
- Event schema evolution is a long-term commitment
- Duplicates, ordering, and failure handling need design
- No single place showing the whole flow

## When to use
Microservices integration, real-time analytics, IoT, notifications, order/payment workflows, audit and data pipelines.

## When not to
Simple CRUD apps, flows needing strict immediate consistency, small teams where a monolith is enough.
