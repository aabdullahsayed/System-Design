# Backend Engineering — Cheat Sheet (Last-Minute Revision)

## HTTP & REST
- Idempotent methods: GET, PUT, DELETE. NOT idempotent by default: POST.
- 2xx success, 3xx redirect, 4xx client error, 5xx server error.
- 401 = not authenticated. 403 = authenticated but not authorized.
- Statelessness = any server can handle any request → enables horizontal scaling.

## Databases
- ACID: Atomicity, Consistency, Isolation, Durability.
- Index = faster reads, slower writes, more storage — add deliberately, based on real query patterns.
- N+1 query problem: one query per row in a loop instead of one JOIN — the #1 backend performance bug.
- Read replicas scale READS; sharding scales READS and WRITES (but adds real complexity).

## Caching
- Cache-aside: app checks cache, falls back to DB on miss, populates cache.
- Cache invalidation is genuinely hard — TTLs are the simplest, most common safety net.
- Cache stampede: many requests simultaneously miss on the same expired key, all hit the DB at once — mitigate with locking or staggered TTLs.

## Security
- Authentication = who you are. Authorization = what you can do.
- Hash passwords with bcrypt/Argon2 (slow, salted) — never plain SHA-256/MD5.
- Prevent SQL injection with parameterized queries, always.
- JWTs: stateless, self-contained tokens — can't be revoked before expiry without extra infrastructure (a blocklist).

## Messaging & Async
- Message queue = decouples producer from consumer; smooths traffic bursts.
- Pub/Sub = one event, many independent subscribers.
- Idempotency keys make retries safe — essential for any at-least-once delivery system.

## Microservices
- Circuit breaker: fail fast after repeated failures, instead of piling up slow/hung requests.
- API Gateway: single entry point — routing, auth, rate limiting, centralized.
- Saga pattern: sequence of local transactions + compensating actions, for cross-service consistency.

## System Design
- CAP theorem: during a partition, choose Consistency OR Availability (Partition tolerance is mandatory).
- Vertical scaling = bigger server, simple, has a ceiling. Horizontal scaling = more servers, needs statelessness.
- Rate limiting: token bucket is the most common algorithm — allows bursts, enforces a steady average rate.

## Observability
- Logs = detailed per-event records. Metrics = aggregated numeric trends. Traces = a single request's journey across services.
- The 4 golden signals: Latency, Traffic, Errors, Saturation.
- Correlation IDs let you reconstruct one request's full story across multiple services' logs.

## Golden one-liners for interviews
- "Start with a monolith; move to microservices when you have a genuine scaling or team-boundary problem it can't solve."
- "Idempotency is what makes retrying a request after a network failure safe."
- "CAP: partition tolerance isn't optional in a real distributed system — the real choice is consistency vs availability."
- "Cache the read-heavy, rarely-changing stuff first — it's usually the highest-leverage, simplest performance win."
- "Every network call needs a timeout, and every repeatedly-failing call should trip a circuit breaker."
