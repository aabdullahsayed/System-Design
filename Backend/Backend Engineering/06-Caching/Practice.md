# Practice — Caching

1. Install Redis locally (or use a free cloud tier), and practice basic commands: SET/GET with an expiry, HSET/HGET for a structured record, and ZADD/ZREVRANGE for a small leaderboard.
2. Implement a cache-aside pattern in code: a function that checks Redis first, falls back to a (simulated slow) database call on a miss, and populates the cache before returning.
3. Time the difference between a cached and uncached call to the same function (add an artificial `sleep`/delay to simulate a slow DB query) to see the real performance impact.
4. Implement explicit cache invalidation: a write endpoint that updates "the database" (even a simple in-memory dict) and also deletes/updates the corresponding cache entry.
5. Simulate a cache stampede: have 10 concurrent requests hit an expired cache key simultaneously (without a stampede mitigation), and observe how many times the "slow" backing function gets called — then add a simple lock and observe the difference.
6. Research: look up how your favorite website/app serves images (view a network request in your browser's dev tools) — identify whether a CDN is involved by checking the response headers (e.g., `cf-cache-status`, `x-cache`, or the serving domain).

✅ Done? Move to `07-Message-Queues-and-Async`.
