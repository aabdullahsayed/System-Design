# Cache Invalidation

## What is it?
The problem of knowing WHEN cached data is stale and needs to be refreshed or removed — famously one of the hardest problems in computer science, precisely because it's easy to get subtly, silently wrong.

## Why care?
A cache serving STALE data isn't just "slightly outdated" — it can cause genuinely confusing bugs (a user updates their profile, refreshes, and still sees old data) and, in worse cases, real business problems (stale pricing or inventory data shown to customers).

## Strategy 1: TTL (Time-based expiration) — simplest, most common
```python
redis.set(f"user:{user_id}", data, ex=300)   # automatically expires after 5 minutes
```
Simple and self-healing (stale data auto-expires eventually), but there's always a window where data COULD be stale (up to the TTL duration) — acceptable for data where slight staleness is fine (a product description), risky for data where it isn't (available inventory count, account balance).

## Strategy 2: Explicit invalidation on write
```python
def update_user(user_id, new_data):
    db.update_user(user_id, new_data)
    redis.delete(f"user:{user_id}")     # explicitly invalidate the cache entry on write
```
More precise — the cache is cleared exactly when the underlying data changes, not just after some fixed time — but requires DISCIPLINE: every single code path that modifies this data must remember to also invalidate the cache, which is a real, common source of bugs when a new write path is added later and this step is forgotten.

## Strategy 3: Write-through (update cache and DB together)
```python
def update_user(user_id, new_data):
    db.update_user(user_id, new_data)
    redis.set(f"user:{user_id}", new_data, ex=300)   # update cache with the NEW data directly, don't just delete it
```
Avoids a "cache miss immediately after write" scenario (where the very next read has to hit the slow path again) at the cost of slightly more write-path complexity.

## Cache stampede (a real, subtle production issue)
When a popular cache key expires, MANY simultaneous requests can all experience a cache miss at once, and all hammer the database simultaneously trying to repopulate the same cache entry — potentially overwhelming the database exactly when load is highest.
**Mitigations**:
- **Locking**: only let ONE request repopulate the cache; others wait briefly for that result.
- **Probabilistic early expiration**: refresh slightly before actual expiry, staggered randomly, to avoid many keys expiring in the exact same instant.
- **Stale-while-revalidate**: serve the (slightly) stale cached value immediately while asynchronously refreshing it in the background — the user gets a fast response, and the cache is updated shortly after, transparently.

## Choosing a strategy based on data volatility
```
Rarely changes (product category list):     Long TTL, or even manual invalidation on the rare admin update
Changes on user action (user profile):        Explicit invalidation on write
Changes very frequently (stock ticker price):   Very short TTL, or don't cache at all — real-time source instead
```

## In backend engineering
There's no single universally correct invalidation strategy — the right choice genuinely depends on how tolerant a specific piece of data is to being briefly stale, and getting this wrong is a very common, realistic source of confusing production bugs worth planning around deliberately rather than defaulting blindly to "just cache everything with a TTL."
