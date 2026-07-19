# Practice — Scalability & Performance

1. Explain in your own words: why must an application be "stateless" before it can be horizontally scaled? What specifically breaks if it isn't?
2. Design (on paper) a database scaling strategy for a read-heavy blog platform (many more reads than writes) vs. a write-heavy IoT sensor-logging platform — explain why you'd choose different strategies (read replicas vs sharding) for each.
3. Reproduce an N+1 query problem in a small script/ORM setup, measure the number of queries executed, then fix it with a JOIN and confirm the query count drops to 1.
4. Run `EXPLAIN ANALYZE` on a slow query against a reasonably large table (or generate synthetic data to make one large enough to be slow), identify whether it's doing a full table scan, then add an appropriate index and re-run to confirm improvement.
5. Load test a simple local API endpoint using `wrk` or a similar tool at increasing concurrency levels (10, 50, 200 concurrent connections) — plot how response time changes as concurrency increases, and identify the point where it starts degrading.
6. Explain in your own words: why does blocking the event loop with CPU-heavy work in a Node.js/async Python server affect EVERY user, not just the one who triggered the heavy computation?

✅ Done? Move to `13-Observability`.
