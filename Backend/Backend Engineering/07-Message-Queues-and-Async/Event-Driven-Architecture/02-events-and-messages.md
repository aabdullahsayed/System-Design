# Events: Types, Design, and Schema

## Message kinds
| Kind | Meaning | Example | Direction |
|------|---------|---------|-----------|
| **Event** | A fact that already happened (past tense) | `OrderPlaced` | Broadcast, many consumers |
| **Command** | A request to do something (imperative) | `ChargeCard` | One specific handler |
| **Query** | A request for data | `GetOrder` | One responder |

## Event types
- **Domain event**: business-meaningful fact inside a bounded context (`InvoicePaid`).
- **Integration event**: published across services/contexts; stable public contract.
- **Notification event (thin)**: only an ID and type; consumers call back for details.
- **Event-carried state transfer (fat)**: includes the data consumers need; no callback.
- **Change event (CDC)**: row-level change captured from a database.

## Thin vs fat events
| | Thin | Fat (state transfer) |
|---|---|---|
| Size | Small | Larger |
| Coupling | Consumer must call producer | Consumer self-sufficient |
| Staleness | Always fresh on callback | Snapshot may go stale |
| Risk | Callback storms, availability dependency | Schema/privacy exposure |

## Anatomy of a good event
```json
{
  "id": "evt_01HZX...",
  "type": "order.placed",
  "version": 2,
  "source": "order-service",
  "time": "2026-10-21T07:00:00Z",
  "correlationId": "req_8f2...",
  "causationId": "evt_01HZW...",
  "data": { "orderId": "o_123", "total": 4200, "currency": "USD" }
}
```
Tip: follow the **CloudEvents** spec for a standard envelope.

## Naming
- Past tense, business language: `OrderShipped`, not `UpdateOrder`.
- Include the domain: `billing.invoice.paid`.

## Schema and versioning
- Use a schema format: Avro, Protobuf, JSON Schema; manage via a **schema registry**.
- **Backward compatible**: new consumers read old events.
- **Forward compatible**: old consumers read new events.
- Prefer additive changes (new optional fields); never repurpose a field.
- Version in the event (`version`) or type name; support old versions during migration.
- Treat events as a **public API**.

## Ordering and keys
Choose a partition/ordering key (e.g. `orderId`) so events for the same entity stay in order.
