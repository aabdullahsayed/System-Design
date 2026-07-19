# Vertical vs Horizontal Scaling

## What is it?
Two fundamentally different strategies for handling more load: **vertical scaling** (scale up) means making a single server more powerful (more CPU/RAM); **horizontal scaling** (scale out) means adding MORE servers and distributing load across them.

## Why care?
This choice shapes your entire architecture — horizontal scaling requires designing for statelessness and coordination between servers from day one; vertical scaling is simpler but has a hard ceiling. Nearly every "how would you scale this" system design interview question centers on this decision.

## Vertical Scaling
```
Before:  [Server: 4 CPU, 16GB RAM]
After:   [Server: 32 CPU, 256GB RAM]   ← same single server, bigger
```
**Pros**: simple — no application changes needed, no distributed systems complexity, no data consistency issues (still one server, one database instance).
**Cons**: there's a hard physical ceiling (can't scale infinitely), it's a **single point of failure** (one server going down takes everything down), and cost scales non-linearly (very large servers are disproportionately expensive vs several smaller ones).

## Horizontal Scaling
```
Before:  [Server]
After:   [Server] [Server] [Server]  ← behind a load balancer
```
**Pros**: near-limitless scaling potential, better fault tolerance (one server failing doesn't take down the whole system, assuming proper load balancing/redundancy), often more cost-effective at scale (many commodity servers vs one giant specialized one).
**Cons**: requires your application to be designed for it — critically, **the application must be stateless** (see below), requires a load balancer, and introduces distributed systems complexity (session handling, cache consistency, distributed transactions).

## The critical prerequisite: statelessness
Horizontal scaling only works cleanly if any server can handle any request — meaning the application shouldn't store session state (e.g., "is this user logged in") in a single server's local memory. If it does, a user's second request might hit a DIFFERENT server that doesn't know they're logged in.
```
BAD (stateful):   session stored in Server A's memory → user's next request hits Server B → "who is this user?"
GOOD (stateless): session stored in a shared store (Redis, database, or a signed JWT the client holds) → any server can validate it
```
This single architectural decision — designing for statelessness — is one of the most important, foundational choices for building a horizontally scalable backend.

## The realistic, combined approach
Most real production systems use BOTH: vertically-scaled reasonably-sized servers/database instances, THEN horizontally scaled by running multiple instances of those behind a load balancer — getting the simplicity benefits of reasonably-powerful individual machines plus the fault-tolerance/scaling benefits of running several of them.

## Practical example
A startup's API server initially runs on a single beefy EC2 instance (vertical scaling — simple, works fine for moderate traffic). As traffic grows and that single instance becomes a bottleneck AND a single point of failure risk, the team refactors the app to be stateless (moving sessions to Redis) and deploys 4 identical instances behind an Application Load Balancer (horizontal scaling) — now individual server failures don't cause an outage, and they can add/remove instances dynamically based on load.
