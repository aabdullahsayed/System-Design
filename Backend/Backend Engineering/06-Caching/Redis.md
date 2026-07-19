# Redis

## What is it?
Redis is an in-memory key-value data store — extremely fast (sub-millisecond operations) because data lives in RAM, commonly used for caching, session storage, rate limiting, and real-time features like leaderboards.

## Why care?
Redis is close to a universal default in modern backend stacks — understanding its core data types and common patterns is directly, practically useful across nearly any backend role.

## Basic operations
```bash
SET user:123 "Alice"
GET user:123
DEL user:123
EXPIRE user:123 300        # expire in 300 seconds
TTL user:123                 # check remaining time-to-live
```

## Data types beyond simple strings (Redis's real power)
```bash
# Hash — like a mini object/dictionary, efficient for storing structured records
HSET user:123 name "Alice" email "alice@example.com"
HGET user:123 name

# List — ordered, good for queues/recent-activity feeds
LPUSH recent_orders "order:456"
LRANGE recent_orders 0 9        # get the 10 most recent

# Set — unique, unordered collection, good for tags/membership checks
SADD user:123:roles "admin" "editor"
SISMEMBER user:123:roles "admin"

# Sorted Set — like a set, but with a score, enabling instant ranking — great for leaderboards
ZADD leaderboard 9500 "player1"
ZADD leaderboard 8700 "player2"
ZREVRANGE leaderboard 0 9 WITHSCORES     # top 10 players, descending score
```

## Common real-world Redis use cases
- **Caching** (`Caching-Basics.md`): the most common use case, storing computed results/DB query results with a TTL.
- **Session storage**: storing user session data outside the application server itself, essential for horizontally-scaled apps where a user's requests might hit different server instances.
- **Rate limiting** (`12-Scalability-and-Performance/Rate-Limiting.md`): using Redis's atomic `INCR` command to count requests per user within a time window.
```bash
INCR rate_limit:user123
EXPIRE rate_limit:user123 60     # reset counter every 60 seconds
```
- **Distributed locks**: coordinating exclusive access to a resource across multiple server instances (`SET key value NX EX 30` — set only if not already set, with an expiry, a common simple locking pattern).
- **Pub/Sub messaging**: lightweight publish/subscribe messaging between services (though for durable, guaranteed-delivery messaging, a dedicated message queue like Kafka/RabbitMQ is usually more appropriate — see `07-Message-Queues-and-Async`).
- **Real-time leaderboards/counters**: sorted sets make "top 10 right now" queries essentially instant.

## Persistence (Redis isn't purely ephemeral)
Despite being in-memory, Redis supports optional persistence to disk (RDB snapshots, AOF append-only logs) for recovering data after a restart — but it's still fundamentally optimized for speed over the same level of durability guarantees as a traditional database, and shouldn't be your ONLY copy of genuinely critical data.

## In backend engineering
Redis is memory-limited and (in its simplest single-instance form) a single point of failure — production deployments typically use **Redis Cluster** or **managed Redis** (AWS ElastiCache, Redis Cloud) for replication and high availability, directly connecting to scalability concepts in `08-System-Design-Basics`.
