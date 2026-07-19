# CDN (Content Delivery Network)

## What is it?
A CDN is a geographically distributed network of servers that cache and serve content (images, videos, JS/CSS files, and increasingly, API responses) from a location physically close to the requesting user, rather than always hitting your origin server.

## Why care?
Physical distance directly adds network latency — a user in Tokyo requesting a file from a server in Virginia experiences real, physics-limited delay. CDNs solve this by serving cached content from a nearby "edge" location instead, and are a standard, expected part of any production web architecture.

## How it works (conceptually)
```
1. User in Tokyo requests https://example.com/logo.png
2. DNS routes them to the NEAREST CDN edge server (e.g., a Tokyo edge node), not the origin server.
3. If that edge node already has "logo.png" cached: serves it immediately (a "cache hit" — very fast).
4. If not cached yet (a "cache miss"): the edge server fetches it from your origin server ONCE, caches it, and serves it — subsequent requests from nearby users hit the now-warmed cache.
```

## What CDNs are good for
- **Static assets**: images, CSS, JS, videos, fonts — content that's identical for every user and changes infrequently, the classic, ideal CDN use case.
- **Static site hosting**: entire static websites can be served entirely from a CDN's edge network.
- **DDoS protection and absorbing traffic spikes**: a CDN's massive distributed capacity can absorb sudden traffic surges that would overwhelm a single origin server.
- **Increasingly, API response caching**: some CDNs (Cloudflare, Fastly) now support caching dynamic API responses too, with careful cache-key and invalidation configuration.

## Cache-Control headers (how you tell a CDN — and browsers — how to cache your content)
```
Cache-Control: public, max-age=86400        -- cache for 24 hours, shareable across users
Cache-Control: private, max-age=0, no-store  -- never cache (e.g., for user-specific, sensitive data)
Cache-Control: public, max-age=31536000, immutable   -- cache "forever" (1 year) — common for versioned/hashed asset filenames
```
The `immutable` + versioned-filename pattern (`app.a1b2c3.js` instead of `app.js`) is a very common, powerful technique: since the filename itself changes whenever content changes, you can safely cache the OLD filename forever, because it will simply never be requested again once a new version is deployed under a new filename.

## Popular CDN providers
Cloudflare, AWS CloudFront, Fastly, Akamai, Google Cloud CDN — most modern hosting platforms (Vercel, Netlify) include CDN distribution automatically as part of their default deployment.

## In backend engineering
- **Origin shielding**: configuring a single, designated CDN region to always check with the origin server first (rather than every edge node independently hitting origin on a miss), reducing redundant load on your actual backend during a cache-cold scenario (e.g., right after a deploy invalidates cached content).
- **Cache purging/invalidation**: most CDNs provide an API to explicitly purge cached content immediately (e.g., after publishing updated content) rather than only relying on `max-age` expiration — directly connects to the same invalidation challenges discussed in `Cache-Invalidation.md`, just at the CDN layer instead of the application layer.
