# Rate Limiting

## What is it?
Restricting how many requests a client (user, API key, IP address) can make within a given time window — protecting a backend system from being overwhelmed, whether by a genuine traffic spike, a buggy client retrying too aggressively, or a deliberate abuse/attack.

## Why care?
Without rate limiting, a single misbehaving client (or attacker) can degrade or take down a service for EVERYONE — this is a standard, expected piece of production API infrastructure, and a common system design interview topic.

## Common algorithms

### Fixed Window
Count requests in fixed time buckets (e.g., "max 100 requests per minute, resetting every minute on the clock").
**Weakness**: allows a burst of up to 2x the limit right at the window boundary (100 requests in the last second of one window + 100 in the first second of the next = 200 requests in ~2 seconds).

### Sliding Window
Tracks requests over a continuously moving time window (e.g., "max 100 requests in any trailing 60-second period") — smooths out the fixed-window boundary burst problem, at the cost of slightly more computation/memory to track.

### Token Bucket (very widely used in practice)
Each client has a "bucket" that holds up to N tokens, refilling at a steady rate (e.g., 10 tokens/second, max bucket size 100). Each request consumes one token; if the bucket is empty, the request is rejected/delayed.
```python
import time

class TokenBucket:
    def __init__(self, capacity, refill_rate):
        self.capacity = capacity
        self.tokens = capacity
        self.refill_rate = refill_rate   # tokens per second
        self.last_refill = time.time()

    def allow_request(self):
        now = time.time()
        elapsed = now - self.last_refill
        self.tokens = min(self.capacity, self.tokens + elapsed * self.refill_rate)
        self.last_refill = now

        if self.tokens >= 1:
            self.tokens -= 1
            return True
        return False
```
**Why it's popular**: naturally allows short BURSTS of traffic (up to the bucket capacity) while still enforcing a steady average rate over time — a good match for how real client traffic actually behaves (bursty, not perfectly smooth).

### Leaky Bucket
Similar concept, but processes requests at a strictly constant output rate regardless of input burstiness (like a bucket with a small hole draining at a fixed rate) — smooths bursts out entirely rather than allowing them, useful when the downstream system genuinely can't handle any burst at all.

## Where to implement rate limiting
- **At the API Gateway** (`11-Microservices/API-Gateway.md`) — centralized, protects all backend services uniformly without each one needing its own implementation.
- **Using Redis** for distributed rate limiting across multiple server instances (a single server's in-memory counter won't work correctly once you have multiple instances behind a load balancer — you need a SHARED counter, which Redis provides efficiently).

## Communicating limits to clients (good API design)
```
HTTP/1.1 429 Too Many Requests
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 0
X-RateLimit-Reset: 1689786000
Retry-After: 30
```
Returning clear, standard headers lets well-behaved API clients back off appropriately instead of hammering a rate-limited endpoint blindly.

## Practical example
A public API allows 1,000 requests/hour per API key, implemented via a token bucket backed by Redis (shared across all API server instances). A client that bursts 50 requests in one second is allowed (within the bucket's burst capacity), but a client trying to sustain 2,000 requests/hour gets consistently rate-limited with clear `429` responses and a `Retry-After` header — protecting backend resources while still accommodating normal, bursty real-world client behavior.
