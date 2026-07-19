# Case Study: Designing a URL Shortener

## What is it?
A classic system design interview question (build something like bit.ly) — small enough to reason about in an interview, but touches nearly every concept in this folder: scaling, database design, caching, and trade-off reasoning.

## Why care?
Walking through a complete, realistic system design end-to-end is the best way to see how the individual concepts (load balancing, caching, database scaling) combine into a coherent real system — and this exact question (or a close variant) is genuinely common in interviews.

## Step 1: Clarify requirements
- **Functional**: given a long URL, generate a short one; given a short URL, redirect to the original.
- **Non-functional**: high availability, low latency (redirects should be near-instant), and a REALISTIC estimate of scale (e.g., "100M new URLs/month, 10:1 read:write ratio" — always state your assumptions explicitly in an interview).

## Step 2: Core API design
```
POST /api/shorten   { "long_url": "https://example.com/very/long/path" }
  → { "short_url": "https://short.ly/aZ3xQ" }

GET /aZ3xQ
  → 301/302 redirect to the original long URL
```

## Step 3: The core algorithm — generating short codes
- **Base62 encoding of an auto-incrementing ID**: convert a sequential database ID (1, 2, 3...) into a base62 string (`a-z`, `A-Z`, `0-9`) — a 7-character base62 string can represent ~3.5 trillion unique values, more than enough.
- **Alternative: hash-based** (e.g., MD5 of the long URL, take the first 7 characters) — simpler conceptually, but needs collision handling (two different URLs hashing to the same prefix).
- Trade-off discussion: base62-of-sequential-ID avoids collisions entirely and is simpler to reason about, but requires a way to generate unique, distributed IDs at scale (see below).

## Step 4: Database design
```sql
CREATE TABLE urls (
    id BIGSERIAL PRIMARY KEY,
    short_code VARCHAR(10) UNIQUE,
    long_url TEXT NOT NULL,
    created_at TIMESTAMP DEFAULT NOW(),
    click_count BIGINT DEFAULT 0
);
CREATE INDEX idx_short_code ON urls(short_code);   -- essential — every redirect looks up by this
```

## Step 5: Scaling the read path (redirects are read-heavy, ~10:1 or more)
- **Cache short_code → long_url mappings in Redis** (`06-Caching/Redis.md`) — redirects are by far the most frequent operation, and this data changes rarely, making it an ideal caching candidate.
- **CDN/edge caching** for the redirect response itself, where feasible.
- **Read replicas** (`Database-Scaling.md`) for cache-miss lookups, since reads vastly outnumber writes.

## Step 6: Scaling ID generation (a subtle, genuinely interesting challenge)
A single auto-incrementing database ID becomes a bottleneck/single point of failure at scale. Real solutions: **distributed ID generation** (e.g., Twitter's Snowflake algorithm — encoding timestamp + machine ID + sequence number into a unique ID without needing a single central counter), or pre-allocating ID RANGES to different server instances so they can generate IDs independently without coordinating on every single request.

## Step 7: Additional considerations to discuss
- **Analytics** (click counts): don't update `click_count` synchronously on every redirect (adds latency to the hot path) — instead, asynchronously increment via a message queue (`07-Message-Queues-and-Async`) or batch-aggregate periodically.
- **Custom short codes**: user-requested vanity URLs need a uniqueness check against the same table.
- **Expiration**: some URL shorteners support optional expiry — a straightforward additional column and a background cleanup job.

## Interview tip
The specific "correct" design matters less than your ability to: state assumptions explicitly, identify the actual bottleneck (here, the read-heavy redirect path), and justify trade-offs (e.g., "I'd cache aggressively here because redirects vastly outnumber new URL creation, and slight staleness in click counts is acceptable, but staleness in the redirect target itself is NOT acceptable").
