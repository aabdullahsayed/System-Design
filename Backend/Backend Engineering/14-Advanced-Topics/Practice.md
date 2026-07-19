# Practice — Advanced Topics

1. For 3 different real systems you use daily (e.g., your bank's app, a social media app, a ride-sharing app), classify each one's likely CAP trade-off (CP or AP) for its core data, and justify your reasoning based on the business consequences of being wrong.
2. Research and write a short explanation (3-5 sentences) of how Raft's "majority quorum" requirement specifically prevents split-brain scenarios during a network partition.
3. Implement an idempotency-key pattern for a simple "create payment" endpoint in a small API — demonstrate that sending the same request twice (same key) doesn't create a duplicate charge/record.
4. Implement a token bucket rate limiter from scratch (as shown in `Rate-Limiting.md`), and write a test that confirms it allows a burst up to capacity but rejects requests beyond the sustained rate.
5. Explain in your own words: why is `POST` not idempotent by default, but `PUT` is — and how does adding an idempotency key change POST's behavior without changing the HTTP method itself?
6. Design (on paper) a rate limiter that needs to work correctly across 5 different API server instances behind a load balancer — explain specifically why a simple in-memory counter on each server wouldn't work, and how Redis solves this.

✅ Done? Move to `15-Interview-Prep`.
