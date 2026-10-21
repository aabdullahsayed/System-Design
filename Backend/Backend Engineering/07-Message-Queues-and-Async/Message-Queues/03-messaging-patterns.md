# Messaging Patterns

## 1. Point-to-point (work queue)
One queue, many consumers; **each message goes to exactly one consumer**. Used for task distribution.

```
Producer -> [ Queue ] -> Consumer A
                      -> Consumer B   (competing consumers)
```

## 2. Publish/Subscribe
A message is delivered to **all** subscribers of a topic.

```
Publisher -> [ Topic ] -> Subscriber 1
                       -> Subscriber 2
                       -> Subscriber 3
```

## 3. Request/Reply (RPC over MQ)
Requester sends a message with a `reply-to` queue and a `correlation-id`; responder replies to that queue. Adds async RPC with decoupling, but more complex than HTTP.

## 4. Routing
Exchange routes messages to queues based on the routing key (e.g. `logs.error` vs `logs.info`).

## 5. Competing consumers
Multiple consumers read from one queue to scale throughput.

## 6. Dead Letter Queue
Failed/expired messages move to a DLQ for inspection and reprocessing.

## 7. Retry with backoff
Failed message is re-published to a delay/retry queue with increasing delays (e.g. 1s, 5s, 30s), then DLQ after max attempts.

## 8. Priority queue
Higher-priority messages are delivered first.

## 9. Outbox pattern
Write the message to an **outbox table** in the same DB transaction as the business change; a relay publishes it to the broker. Avoids the dual-write problem.

## 10. Saga
Distributed transaction as a series of local transactions coordinated by events, with compensating actions on failure.

## 11. Fan-out / Fan-in
Split work into many messages (fan-out), then aggregate results (fan-in).

## 12. Claim check
Store the large payload in object storage; put only a reference in the message.
