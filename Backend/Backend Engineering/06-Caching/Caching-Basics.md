# Caching Basics

## What is it?
Storing a copy of expensive-to-compute or slow-to-fetch data somewhere faster to access, so repeated requests don't redo the same expensive work.

## Why care?
Caching is one of the single highest-leverage performance techniques in backend engineering — the fastest database query is the one you never had to run. Nearly every high-traffic system relies heavily on caching at multiple layers.

## Where caching happens (multiple layers, often combined)
```
Browser cache        → client-side, avoids re-downloading unchanged assets
CDN                  → geographically distributed cache for static content (06-Caching/CDN.md)
Application cache     → in-memory or Redis cache for expensive computations/DB queries
Database query cache   → some databases cache repeated identical query results
```

## Cache-Aside (Lazy Loading) — the most common pattern
```python
def get_user(user_id):
    cached = redis.get(f"user:{user_id}")
    if cached:
        return json.loads(cached)          # cache hit — fast path

    user = db.query("SELECT * FROM users WHERE id = %s", user_id)   # cache miss — slow path
    redis.set(f"user:{user_id}", json.dumps(user), ex=300)            # populate cache, 5 min expiry
    return user
```
The application checks the cache first; on a miss, it fetches from the source of truth and populates the cache for next time.

## Write-Through vs Write-Behind
- **Write-Through**: every write updates BOTH the cache and the database synchronously — cache is always consistent, but writes are slightly slower.
- **Write-Behind (Write-Back)**: writes go to the cache immediately, and are asynchronously flushed to the database later — faster writes, but risk of data loss if the cache fails before flushing.

## Cache eviction policies (what happens when the cache is full)
- **LRU (Least Recently Used)**: evict the item that hasn't been accessed in the longest time — the most common default (`01-Programming-Fundamentals/Data-Structures.md` has an LRU implementation).
- **LFU (Least Frequently Used)**: evict the item accessed least often overall.
- **TTL (Time To Live)**: items automatically expire after a set duration, regardless of access pattern — often used alongside LRU/LFU.

## Cache hit ratio — the key metric
```
Hit Ratio = Cache Hits / (Cache Hits + Cache Misses)
```
A low hit ratio means your cache isn't providing much value (either poorly chosen keys, too-short TTL, or genuinely unpredictable/unique access patterns) — this is a real metric to monitor in production (`13-Observability`).

## In backend engineering
- **Database load reduction**: a well-cached read-heavy endpoint can reduce database load by 90%+ , directly improving both latency and the database's ability to handle other traffic.
- **The hardest problem in caching is invalidation** ("There are only two hard things in Computer Science: cache invalidation and naming things") — see `Cache-Invalidation.md` for exactly this challenge.
