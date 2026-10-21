# Data Types & Commands

## Keys
- Binary-safe strings; keep them readable and namespaced: `user:42:profile`, `cart:abc123`.
- Avoid very long keys; avoid the `KEYS *` command in production (use `SCAN`).

```bash
EXISTS key        DEL key          UNLINK key (async delete)
EXPIRE key 60     TTL key          PERSIST key
TYPE key          RENAME a b       SCAN 0 MATCH user:* COUNT 100
```

## Strings
Text, numbers, or binary up to 512 MB.
```bash
SET name "Ada"             GET name
SET token abc EX 3600      # expire in 1 hour
SET lock 1 NX PX 30000     # set only if not exists, 30s
INCR page:views            INCRBY stock:5 10        DECR / DECRBY
MSET a 1 b 2               MGET a b
APPEND log "text"          STRLEN name
```

## Hashes (objects / field-value maps)
```bash
HSET user:1 name "Ada" age 36 city "London"
HGET user:1 name
HGETALL user:1
HINCRBY user:1 age 1
HDEL user:1 city
```

## Lists (linked lists, queues, stacks)
```bash
LPUSH jobs "a"        RPUSH jobs "b"
LPOP jobs             RPOP jobs
BRPOP jobs 5          # blocking pop, 5s timeout (worker queues)
LRANGE jobs 0 -1      LLEN jobs        LTRIM jobs 0 99
```

## Sets (unique, unordered)
```bash
SADD tags "redis" "cache"      SMEMBERS tags
SISMEMBER tags "redis"         SCARD tags
SINTER a b    SUNION a b    SDIFF a b      SPOP / SRANDMEMBER
```

## Sorted Sets (unique members with scores)
```bash
ZADD leaderboard 100 "ada" 250 "bob" 175 "cy"
ZRANGE leaderboard 0 -1 WITHSCORES           # ascending
ZREVRANGE leaderboard 0 2 WITHSCORES         # top 3
ZRANK leaderboard "ada"      ZSCORE leaderboard "bob"
ZINCRBY leaderboard 50 "ada"
ZRANGEBYSCORE leaderboard 100 200
ZREMRANGEBYSCORE rate:user1 0 <timestamp>    # sliding window cleanup
```
(Newer versions also support `ZRANGE ... REV BYSCORE` style.)

## Streams (append-only log with consumer groups)
```bash
XADD orders * id 123 total 42
XLEN orders
XRANGE orders - +
XGROUP CREATE orders workers $ MKSTREAM
XREADGROUP GROUP workers c1 COUNT 10 BLOCK 5000 STREAMS orders >
XACK orders workers 1729494900000-0
XPENDING orders workers
```
Like a lightweight Kafka: persistent, replayable, consumer groups with acknowledgements.

## Pub/Sub
```bash
SUBSCRIBE news            PUBLISH news "hello"      PSUBSCRIBE news.*
```
Fire-and-forget: subscribers that are offline miss messages. Use Streams for durability.

## Specialized types
| Type | Use | Commands |
|------|-----|----------|
| **Bitmap** | Flags per ID, daily active users | `SETBIT`, `GETBIT`, `BITCOUNT` |
| **HyperLogLog** | Approximate unique counts (~0.81% error, 12 KB) | `PFADD`, `PFCOUNT` |
| **Geospatial** | Locations, nearby search | `GEOADD`, `GEOSEARCH` |
| **JSON** (module/Stack) | Nested JSON docs | `JSON.SET`, `JSON.GET` |
| **Bloom / Cuckoo filters** | Probabilistic membership | `BF.ADD`, `BF.EXISTS` |
| **Time series** | Metrics over time | `TS.ADD`, `TS.RANGE` |
| **Vector sets / search** | Similarity search | Query Engine |

## Complexity cheat sheet
| Operation | Complexity |
|-----------|------------|
| GET / SET / HGET / HSET | O(1) |
| LPUSH / RPUSH / LPOP / RPOP | O(1) |
| LRANGE | O(S+N) |
| SADD / SISMEMBER | O(1) |
| ZADD / ZRANK / ZINCRBY | O(log N) |
| ZRANGE | O(log N + M) |
| KEYS | O(N), avoid in prod |
| SMEMBERS / HGETALL on huge collections | O(N), can block |

## Expiration and eviction
- `EXPIRE`, `SET ... EX`, `TTL`: keys deleted lazily and by background sampling.
- When memory hits `maxmemory`, the **eviction policy** applies:
  - `noeviction`: reject writes (default)
  - `allkeys-lru` / `allkeys-lfu`: evict least recently/frequently used across all keys
  - `volatile-lru` / `volatile-lfu` / `volatile-ttl`: only keys with TTL
  - `allkeys-random`, `volatile-random`
- Typical cache setup: `maxmemory 2gb` + `maxmemory-policy allkeys-lru` (or `allkeys-lfu`).
