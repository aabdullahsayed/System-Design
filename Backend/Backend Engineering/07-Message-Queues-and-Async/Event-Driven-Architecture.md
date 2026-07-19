# Event-Driven Architecture

## What is it?
An architectural style where services communicate by publishing and reacting to EVENTS (facts about something that happened, e.g., "OrderCreated," "PaymentProcessed") rather than directly calling each other — built on top of message queues/event streams (`Message-Queues.md`).

## Why care?
Event-driven architecture is a common, powerful pattern for decoupling services in larger systems — especially relevant as you move into microservices (`11-Microservices`), where tightly-coupled direct service-to-service calls become a genuine scaling and reliability liability.

## Direct calls vs event-driven — the core difference
```
Direct (tightly coupled):
  OrderService directly calls InventoryService.decrementStock()
  and EmailService.sendConfirmation()
  and AnalyticsService.trackPurchase()
  → OrderService needs to know about ALL of these other services,
    and if any one is down/slow, it can block/fail the whole order flow.

Event-driven (decoupled):
  OrderService publishes an "OrderCreated" event and moves on.
  InventoryService, EmailService, and AnalyticsService each independently
  SUBSCRIBE to "OrderCreated" and react on their own.
  → OrderService doesn't know or care who's listening; new subscribers
    can be added later WITHOUT modifying OrderService at all.
```

## Publish/Subscribe (Pub/Sub) pattern
```python
# Publisher — doesn't know or care who's listening
event_bus.publish("OrderCreated", {"order_id": 123, "user_id": 456, "total": 99.99})

# Subscribers — each independently reacts to the event
@event_bus.subscribe("OrderCreated")
def update_inventory(event):
    decrement_stock(event["order_id"])

@event_bus.subscribe("OrderCreated")
def send_confirmation(event):
    send_email(event["user_id"], "Order Confirmed")
```

## Benefits
- **Loose coupling**: services don't need direct knowledge of each other, only of the shared event format/contract.
- **Independent scaling and failure isolation**: if the email service is slow/down, it doesn't block order creation — it just processes the backlog of events once it recovers.
- **Extensibility**: adding a new reaction to an existing event (e.g., a new fraud-detection service subscribing to "OrderCreated") requires zero changes to the existing publisher.

## Real challenges this introduces
- **Eventual consistency**: since reactions happen asynchronously, there's a window where, e.g., an order exists but inventory hasn't been decremented YET — systems must be designed with this delay in mind, rather than assuming instant, synchronous consistency everywhere.
- **Debugging/tracing complexity**: following a single business operation across many independently-reacting services is harder than following a single synchronous call stack — this is precisely why distributed tracing (`13-Observability/Distributed-Tracing.md`) becomes essential in event-driven systems.
- **Event schema evolution**: like API versioning (`04-APIs-and-Web-Fundamentals/API-Versioning.md`), event formats need careful, backward-compatible evolution as multiple independent consumers may depend on the existing shape.

## CQRS and Event Sourcing (related, more advanced patterns worth knowing by name)
- **CQRS** (Command Query Responsibility Segregation): separating the model used for WRITES from the model used for READS, often enabled by an event-driven architecture where read models are built by consuming the same events multiple ways.
- **Event Sourcing**: storing the full history of EVENTS as the source of truth (rather than just the current state), letting you reconstruct any past state by replaying events — a more advanced pattern with real benefits (full audit trail, ability to rebuild derived views) and real complexity trade-offs.

## In backend engineering
Event-driven architecture is a genuine, deliberate trade-off — it solves real coupling and scaling problems, but adds real complexity (eventual consistency, harder debugging) that isn't worth taking on for a small, simple system. It becomes increasingly valuable as a system grows into multiple independently-deployed services.
