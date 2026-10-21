# Patterns: Caching, Locks, Queues, Rate Limiting

## Caching strategies
| Strategy | How it works | Notes |
|----------|--------------|-------|
| **Cache-aside (lazy loading)** | App checks cache; on miss reads DB, writes to cache | Most common; risk of stale data |
| **Read-through** | Cache layer loads from DB on miss | Needs cache provider/library support |
| **Write-through** | Write to cache and DB together | Consistent reads; slower writes |
| **Write-behind (write-back)** | Write to cache, flush to DB async | Fast; risk of data loss |
| **Refresh-ahead** | Proactively refresh hot entries before expiry | Smooths latency |

### Cache-aside example (Python)
```python
import json, redis
r = redis.Redis(host="localhost", port=6379, decode_responses=True)

def get_user(user_id):
    key = f"user:{user_id}"
    cached = r.get(key)
    if cached:
        return json.loads(cached)
    user = db.query_user(user_id)             # slow path
    r.set(key, json.dumps(user), ex=300)      # TTL 5 min
    return user

def update_user(user_id, data):
    db.update_user(user_id, data)
    r.delete(f"user:{user_id}")               # invalidate
```

### Cache problems and fixes
| Problem | Description | Fix |
|---------|-------------|-----|
| **Stampede / dogpile** | Hot key expires, many requests hit DB | Locking on rebuild, request coalescing, stale-while-revalidate, jittered TTLs |
| **Penetration** | Requests for nonexistent keys always miss | Cache null results briefly, Bloom filter |
| **Avalanche** | Many keys expire at once | Randomize TTLs, stagger |
| **Hot key** | One key overloads a node | Local in-process cache, replicate key (`key:1..N`) |
| **Stale data** | Cache differs from DB | Short TTL, invalidate on write, event-driven invalidation |
| **Big keys** | Huge values block the server | Split values, avoid giant collections |

## Distributed lock (single instance)
```bash
SET lock:order:42 <unique-token> NX PX 30000
```
Release only if you still own it (Lua for atomicity):
```lua
if redis.call("GET", KEYS[1]) == ARGV[1] then
  return redis.call("DEL", KEYS[1])
else
  return 0
end
```
Guidelines:
- Always set a TTL; use a unique token per holder.
- Keep the critical section short; renew (watchdog) if needed.
- For strict correctness (e.g. money), use fencing tokens or a database/consensus system; Redis locks are best-effort because of failover and clock/pause issues. Redlock is debated; understand the trade-offs.

## Rate limiting

### Fixed window
```bash
INCR rate:user42:202610211540
EXPIRE rate:user42:202610211540 60   # set on first increment
```
Simple, but allows bursts at window boundaries.

### Sliding window log (sorted set)
```bash
ZADD rate:user42 <now_ms> <unique_id>
ZREMRANGEBYSCORE rate:user42 0 <now_ms - 60000>
ZCARD rate:user42        # compare with limit
EXPIRE rate:user42 60
```
Do this in a Lua script or `MULTI/EXEC` to be atomic.

### Token bucket
Store `tokens` and `last_refill` in a hash; refill on each request via Lua.

## Queues and messaging
| Option | Durable | Consumer groups | Replay | Use |
|--------|---------|-----------------|--------|-----|
| **List** (`LPUSH` + `BRPOP`) | Per persistence | No | No | Simple job queues; use `LMOVE`/`BLMOVE` for reliable queue pattern |
| **Pub/Sub** | No | No | No | Ephemeral broadcast |
| **Streams** | Yes | Yes | Yes | Event logs, work queues with acks |

Reliable list queue idea: `BLMOVE jobs processing LEFT RIGHT 5`, then `LREM processing` on success; a reaper requeues stuck jobs.

Libraries built on Redis: BullMQ (Node), Sidekiq (Ruby), Celery/RQ (Python), Asynq (Go).

## Sessions
```bash
HSET session:abc user_id 42 role admin
EXPIRE session:abc 1800
```
Refresh TTL on activity (sliding expiration).

## Leaderboards
```bash
ZADD game:scores 4200 "ada"
ZREVRANGE game:scores 0 9 WITHSCORES   # top 10
ZREVRANK game:scores "ada"             # rank (0-based)
```

## Counting unique visitors
```bash
PFADD visitors:2026-10-21 user1 user2 user3
PFCOUNT visitors:2026-10-21
```

## Atomicity: transactions and Lua
- `MULTI ... EXEC`: queue commands, run atomically (no rollback on logic errors).
- `WATCH key`: optimistic locking; EXEC fails if the key changed.
- **Lua scripts** (`EVAL`/`EVALSHA`) and Functions: run atomically server-side; keep them short.
- **Pipelining**: batch many commands in one round trip for throughput (not atomic).
