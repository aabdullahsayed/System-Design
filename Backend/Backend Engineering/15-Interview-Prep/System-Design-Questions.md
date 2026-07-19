# System Design Interview Questions

Open-ended, scenario-style questions common in mid-to-senior backend interviews. Practice explaining your reasoning out loud, not just naming technologies.

## How to approach ANY system design question
1. **Clarify requirements** — ask about scale (users, requests/sec), read/write ratio, consistency requirements, and what "success" looks like for this system. Never assume — asking good clarifying questions is itself part of what's being evaluated.
2. **Estimate scale** — rough back-of-envelope math on data volume, request rate, storage needs. Doesn't need to be precise, but shows you're thinking quantitatively.
3. **Start high-level** — sketch the major components (client, API layer, database, cache, queue) before diving into details of any one piece.
4. **Go deep on 2-3 interesting areas** — the interviewer usually wants depth somewhere, not uniform shallow coverage everywhere. Follow their interest/questions.
5. **Discuss trade-offs explicitly** — for every major decision (SQL vs NoSQL, sync vs async, CP vs AP), briefly state why you chose it and what you're giving up.
6. **Address failure modes** — what happens when a component goes down? How does the system degrade rather than fully fail?

## Practice questions

### 1. Design a URL shortener (classic starter question)
Key considerations: how to generate short, unique codes (base62 encoding of an auto-incrementing ID, or a hash-based approach); read-heavy workload (redirects vastly outnumber new URL creation) → aggressive caching; database schema (short_code → long_url mapping); handling custom aliases; analytics/click tracking (likely asynchronous, via a message queue, to avoid slowing down the redirect itself).

### 2. Design a rate limiter (as a standalone service)
Key considerations: which algorithm (token bucket vs sliding window — discuss trade-offs); where state is stored for correctness across multiple server instances (Redis); how to handle the rate limiter itself becoming a bottleneck/single point of failure; per-user vs per-IP vs per-API-key limiting.

### 3. Design a notification system (push/email/SMS at scale)
Key considerations: asynchronous processing via a message queue (sending notifications shouldn't block the triggering action); handling multiple channels (push, email, SMS) with different providers/retry semantics; deduplication (avoid sending the same notification twice); user preferences/opt-outs; rate limiting to avoid spamming a user.

### 4. Design a news feed (like Twitter/Instagram's feed)
Key considerations: **fan-out on write** (precompute each follower's feed when a post is made — fast reads, expensive/slow for users with millions of followers) vs **fan-out on read** (compute the feed at request time by querying all followed users' recent posts — slower reads, no wasted precomputation) — discuss the hybrid approach real systems use (fan-out on write for most users, fan-out on read for celebrity accounts with huge follower counts, to avoid the "thundering herd" write problem).

### 5. Design a distributed cache (like a simplified Redis)
Key considerations: eviction policy (LRU, LFU) when the cache is full; consistent hashing for distributing keys across multiple cache server instances with minimal redistribution when instances are added/removed; handling cache server failures gracefully (does the system degrade to hitting the database directly, and if so, how do you prevent a "cache stampede" where all traffic suddenly hits the database at once?).

### 6. Design an e-commerce checkout/inventory system
Key considerations: preventing overselling the last item in stock (requires careful consistency handling — likely CP for inventory counts, using database-level locking or atomic decrements); the Saga pattern for coordinating Order/Payment/Inventory/Shipping across services (see `11-Microservices/Saga-Pattern.md`); idempotency for payment processing (see `14-Advanced-Topics/Idempotency.md`).

## What interviewers are actually evaluating
Not whether you know the "one correct answer" (there usually isn't one) — but whether you can: ask good clarifying questions, reason about trade-offs explicitly, communicate your thinking clearly, and adapt your design when the interviewer pushes back with a new constraint ("what if this needs to handle 100x the traffic?").
