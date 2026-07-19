# Interview Questions — Backend Engineering

Quick-fire theory questions with concise model answers, organized by topic.

## Web Fundamentals
**Q: What's the difference between PUT and PATCH?**
A: PUT replaces the entire resource with the provided representation (idempotent — sending the same PUT twice yields the same end state). PATCH applies a partial update to specific fields only.

**Q: What's the difference between 401 and 403 status codes?**
A: 401 Unauthorized means the client isn't authenticated at all (no/invalid credentials). 403 Forbidden means the client IS authenticated, but doesn't have permission to access this specific resource.

**Q: Explain REST statelessness.**
A: Each request must contain all information needed to process it — the server doesn't store client session state between requests. This is what enables horizontal scaling (any server can handle any request) and simplifies caching.

## Databases
**Q: When would you choose NoSQL over a relational database?**
A: When your data doesn't fit a rigid relational schema well, when you need massive horizontal write scalability beyond what sharding a relational DB conveniently offers, or when your access patterns are simple key-value/document lookups without complex joins. Relational databases remain the better default for data with genuine relationships and where strong consistency/transactions matter.

**Q: What is an index, and what's the trade-off of adding one?**
A: A data structure (typically a B-tree) that lets the database find rows without scanning the whole table, dramatically speeding up reads matching the indexed column(s). Trade-off: every index slows down writes (INSERT/UPDATE/DELETE must also update the index) and uses additional storage — indexes should be added deliberately based on actual query patterns, not indiscriminately.

**Q: Explain ACID.**
A: Atomicity (a transaction fully succeeds or fully rolls back, no partial effects), Consistency (a transaction takes the database from one valid state to another, respecting all constraints), Isolation (concurrent transactions don't interfere with each other's intermediate states), Durability (once committed, data survives even a crash immediately after).

## System Design / Scalability
**Q: How would you scale a read-heavy API?**
A: Layer defenses: add caching (application-level or a CDN for static/cacheable responses), add database read replicas, and only reach for sharding if write load also becomes a bottleneck (which read scaling alone doesn't address).

**Q: Explain the CAP theorem in your own words.**
A: A distributed system can't simultaneously guarantee Consistency and Availability during a network partition (Partition tolerance is effectively mandatory for any real distributed system) — you must choose to either reject requests (favor Consistency) or serve possibly-stale data (favor Availability) when nodes can't communicate.

**Q: What is idempotency and why does it matter for APIs?**
A: An idempotent operation produces the same end result no matter how many times it's performed. It matters because network failures make "did my request actually succeed?" genuinely ambiguous — idempotency is what makes safely retrying a request possible without risking duplicate side effects (like double-charging a customer).

## Security
**Q: What's the difference between authentication and authorization?**
A: Authentication verifies WHO you are (login, proving identity). Authorization determines WHAT you're allowed to do (permissions, access control) — you must be authenticated before authorization checks are meaningful.

**Q: How do you securely store user passwords?**
A: Never store plaintext. Hash with a slow, salted algorithm designed for passwords (bcrypt, Argon2, scrypt) — never a fast general-purpose hash like plain SHA-256/MD5, which are vulnerable to brute-force/rainbow table attacks precisely because they're fast.

**Q: What is SQL injection and how do you prevent it?**
A: An attack where untrusted user input is concatenated directly into a SQL query, letting an attacker inject arbitrary SQL logic. Prevented by ALWAYS using parameterized queries/prepared statements (never string-concatenating user input into SQL), which the database treats input as pure data, never executable code.

## Architecture
**Q: Monolith or microservices for a new startup's first product?**
A: Generally, start with a well-structured monolith. Microservices add substantial operational complexity (network calls, distributed data consistency, service discovery) that's rarely worth it before you have genuine, demonstrated scaling or team-boundary problems a monolith can't solve.

**Q: What problem does a message queue solve that a direct API call doesn't?**
A: Decoupling — the producer doesn't need the consumer to be available right now, and can hand off work and move on immediately. This also naturally smooths out traffic spikes (the queue buffers the burst) and allows independent scaling/retrying of the consumer side without affecting the producer.

See `System-Design-Questions.md` for full scenario-style system design interview practice.
