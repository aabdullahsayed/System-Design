# Inter-Service Communication

## What is it?
The mechanisms microservices use to talk to each other — since they no longer share memory or a single database, every interaction crosses the network, with all the failure modes that implies.

## Why care?
Choosing the wrong communication pattern (or not handling network failures) is THE most common source of production incidents in a microservices architecture — this is genuinely different, harder territory than in-process function calls in a monolith.

## Synchronous communication (request-response)
- **REST/HTTP**: simple, universally understood, easy to debug (readable JSON over HTTP) — but the calling service BLOCKS waiting for a response, and a slow/down downstream service directly slows/breaks the caller.
- **gRPC**: binary protocol (Protocol Buffers), faster and more efficient than REST/JSON, strongly-typed contracts via `.proto` files, built-in support for streaming — commonly used for internal service-to-service calls where performance matters and both ends are services you control.

## Asynchronous communication (fire-and-forget / eventual response)
- **Message queues** (RabbitMQ, SQS): a service publishes a message and moves on; a consumer processes it whenever it's ready — decouples the caller from needing the downstream service to be available RIGHT NOW.
- **Event streaming** (Kafka): services publish events ("OrderPlaced") that any number of other services can subscribe to, without the publisher knowing or caring who's listening — see `07-Message-Queues-and-Async`.

## The cascading failure problem (the #1 real-world microservices danger)
```
Service A calls B calls C calls D.
If D becomes slow, C's requests pile up waiting on D,
C becomes slow, B's requests pile up waiting on C,
and eventually A (and everything calling A) grinds to a halt too.
```
This is why synchronous call CHAINS are dangerous in microservices — a single slow service can cascade failure through the entire system.

## Mitigations (must-know patterns)
- **Timeouts**: every network call MUST have an explicit timeout — never wait indefinitely for a response.
- **Circuit breakers**: after repeated failures calling a downstream service, "trip" the circuit and fail fast immediately (without even attempting the call) for a cooldown period, instead of continuing to pile up slow/failing requests. (Libraries: Netflix Hystrix (legacy but foundational), resilience4j, Polly for .NET.)
- **Retries with exponential backoff**: retry failed calls, but with increasing delay between attempts (and a max retry cap) to avoid hammering an already-struggling service.
- **Bulkheads**: isolate resources (e.g., separate connection pools) per downstream dependency, so one slow dependency can't exhaust resources needed to call OTHER, healthy dependencies.

```python
# Conceptual circuit breaker pattern
import time

class CircuitBreaker:
    def __init__(self, failure_threshold=5, cooldown=30):
        self.failures = 0
        self.threshold = failure_threshold
        self.cooldown = cooldown
        self.open_until = None

    def call(self, func, *args):
        if self.open_until and time.time() < self.open_until:
            raise Exception("Circuit open — failing fast")
        try:
            result = func(*args)
            self.failures = 0   # reset on success
            return result
        except Exception:
            self.failures += 1
            if self.failures >= self.threshold:
                self.open_until = time.time() + self.cooldown
            raise
```

## Practical example
An e-commerce checkout flow calling Inventory, Payment, and Shipping services synchronously is fragile — a slow Shipping service shouldn't block payment confirmation. A more resilient design: confirm payment synchronously (the customer needs immediate feedback), then publish an `OrderPaid` event asynchronously for Inventory and Shipping services to process independently — decoupling the customer-facing critical path from non-blocking downstream work.
