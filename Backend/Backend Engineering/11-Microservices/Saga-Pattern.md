# The Saga Pattern (Distributed Transactions)

## What is it?
A pattern for managing data consistency across MULTIPLE microservices (and their separate databases) when a single business operation needs to update several services — since traditional ACID database transactions (`03-Databases/Transactions-ACID.md`) don't work across separate databases/services.

## Why care?
This is one of the genuinely hard, distinctly "microservices-specific" problems — a monolith with one shared database gets this for free via normal transactions; microservices must solve it explicitly, and getting it wrong causes real data inconsistency bugs in production.

## The problem
```
Order Service: create order
Payment Service: charge customer
Inventory Service: reserve stock

What if Payment succeeds but Inventory fails (out of stock)?
The customer was charged for an order that can't be fulfilled — data is now inconsistent across services.
```
No single database transaction can wrap all three services' separate databases together.

## Saga: a sequence of local transactions with compensating actions
A **saga** breaks the overall operation into a sequence of local transactions (one per service), where EACH step has a defined **compensating transaction** to undo it if a LATER step fails.
```
1. Order Service: create order (pending)         [compensate: cancel order]
2. Payment Service: charge customer                 [compensate: refund customer]
3. Inventory Service: reserve stock                    [compensate: release stock]

If step 3 fails: run compensations for steps 2 and 1, in reverse order
  → refund the customer, cancel the order
```
The end state is consistent (nothing happened, from the customer's perspective) even though it took several separate, non-atomic steps to get there.

## Two coordination styles

### Choreography (decentralized)
Each service publishes an event when it completes its step; other services react to those events independently — no central coordinator.
```
Order Service publishes "OrderCreated"
  → Payment Service listens, charges, publishes "PaymentCompleted" (or "PaymentFailed")
    → Inventory Service listens, reserves stock, publishes "StockReserved" (or "StockUnavailable")
      → if "StockUnavailable": Payment Service listens for this and issues a refund
```
**Pro**: no single point of coordination/failure. **Con**: the overall business logic/flow is implicit, spread across many services' event handlers — can become genuinely hard to reason about and debug as the number of steps grows.

### Orchestration (centralized)
A dedicated orchestrator service explicitly calls each step in sequence and explicitly triggers compensations on failure.
```
Orchestrator: call Order Service → call Payment Service → call Inventory Service
              if any step fails: call compensating actions for prior completed steps, in order
```
**Pro**: the business logic/flow is explicit and centralized, in one place, much easier to understand and debug. **Con**: the orchestrator becomes a critical, potentially complex component, and a new kind of centralized dependency.

## Practical example
An e-commerce checkout saga (orchestration style): a dedicated `CheckoutOrchestrator` service calls Order → Payment → Inventory → Shipping in sequence; if Inventory reports out-of-stock after Payment already succeeded, the orchestrator explicitly triggers a refund via the Payment service's compensating "refund" operation, and marks the order as cancelled — ensuring the customer is never left in a charged-but-unfulfilled state, despite the operation spanning 4 separate services with 4 separate databases.
