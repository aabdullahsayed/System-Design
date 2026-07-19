# Idempotency

## What is it?
An operation is **idempotent** if performing it multiple times has the exact same effect as performing it once. `PUT /users/5 {name: "Alice"}` is idempotent (setting the name to "Alice" twice leaves the same end state as doing it once); `POST /orders {item: "Widget"}` is typically NOT idempotent (calling it twice creates two separate orders).

## Why care?
In any distributed/networked system, requests can fail in ambiguous ways — did the request actually fail, or did it succeed but the response got lost on the way back? **Idempotency is the property that makes "just retry" a genuinely safe strategy** for handling these ambiguous network failures, which are a routine, expected occurrence in real distributed backend systems.

## The core problem it solves
```
Client sends: "Charge customer $50"
Network fails AFTER the server processed it but BEFORE the client got the response.
Client, not knowing if it succeeded, retries: "Charge customer $50" again.

Without idempotency: customer is charged $100 (charged twice) — a real, serious bug.
With idempotency: the server recognizes this is a retry of the same request and does NOT charge again.
```

## Implementing idempotency with idempotency keys
The client generates a unique key (e.g., a UUID) for each LOGICAL operation, and sends it with the request. The server stores which keys it has already processed, and any subsequent request with the same key returns the ORIGINAL result instead of re-executing the operation.
```python
# Server-side pseudocode
def create_payment(request):
    idempotency_key = request.headers.get("Idempotency-Key")

    existing = db.get_result_for_key(idempotency_key)
    if existing:
        return existing   # already processed — return the same result, don't redo it

    result = process_payment(request.body)
    db.store_result_for_key(idempotency_key, result)
    return result
```
This exact pattern is used by real payment providers (Stripe's API famously requires/recommends idempotency keys for payment creation requests, precisely because retried payment requests are a genuinely dangerous class of bug to get wrong).

## Naturally idempotent HTTP methods (by convention/design)
```
GET     — idempotent (reading data has no side effect)
PUT     — idempotent (sets a resource to an exact state — repeating it doesn't change the outcome)
DELETE  — idempotent (deleting an already-deleted resource has the same end state)
POST    — NOT idempotent by default (typically creates a new resource each time) — this is exactly why idempotency keys matter most for POST endpoints
```

## Making database operations idempotent
```sql
-- NOT idempotent: running twice creates duplicate rows
INSERT INTO orders (id, item) VALUES (123, 'Widget');

-- IDEMPOTENT: running twice has the same end effect as running once
INSERT INTO orders (id, item) VALUES (123, 'Widget')
ON CONFLICT (id) DO NOTHING;
-- or, using an UPSERT pattern:
INSERT INTO orders (id, item) VALUES (123, 'Widget')
ON CONFLICT (id) DO UPDATE SET item = EXCLUDED.item;
```

## Practical example
A mobile app's "place order" button, on a flaky mobile network connection, might get tapped twice by an impatient user, or the app's retry logic might automatically resend a request that appears to have timed out. Generating one idempotency key per checkout attempt (e.g., when the user first opens the checkout screen) and including it on every retry ensures the customer is charged and gets exactly one order — regardless of how many times the underlying HTTP request is actually sent due to network flakiness or retry logic.
