# Database Scaling

## What is it?
Techniques to handle a database that's become a bottleneck as data volume and query load grow beyond what a single database server can efficiently handle.

## Why care?
The database is very often THE hardest part of a system to scale (unlike stateless application servers, a database inherently holds state) — and it's one of the most commonly probed topics in backend/system-design interviews.

## Vertical scaling (the first, simplest lever)
Bigger database server (more RAM for caching, faster disks, more CPU cores) — often sufficient for a surprisingly long time, and should usually be tried before more complex horizontal strategies.

## Read Replicas
Create read-only COPIES of the primary database that stay synchronized (via replication) — route all WRITEs to the primary, and distribute READs across the replicas.
```
              ┌──── Read Replica 1 (reads)
Primary (writes) ──── Read Replica 2 (reads)
              └──── Read Replica 3 (reads)
```
**Great for read-heavy workloads** (most web apps are read-heavy — far more reads than writes). **Limitation**: replication has a small lag (replicas are "eventually consistent" with the primary), so a user reading immediately after their own write might briefly see stale data if routed to a lagging replica — a real, common consideration in system design ("read-after-write consistency").

## Sharding (Partitioning)
Split your data across MULTIPLE independent database servers, each holding a subset of the data (e.g., by user ID range, or a hash of a key).
```
Shard 1: Users A-H       Shard 2: Users I-P       Shard 3: Users Q-Z
```
**Solves both read AND write scaling** (unlike read replicas, which only help reads) — each shard independently handles its own subset of write load too. **Trade-off**: significantly more complex — queries spanning multiple shards (e.g., "find all users older than 30" across every shard) become much harder, and resharding (when your partition scheme needs to change) is a genuinely difficult, risky operation on a live system.

## Choosing a shard key (a critical, hard-to-reverse decision)
A good shard key distributes data (and load) evenly across shards, and ideally keeps data that's frequently queried TOGETHER on the same shard (avoiding expensive cross-shard queries/joins). A poorly chosen shard key (e.g., sharding by signup date when most traffic is for recently-signed-up users) creates "hot shards" that receive disproportionate load, defeating the purpose of sharding.

## Caching (reducing database load without scaling the database itself)
See `06-Caching` — often the highest-leverage, simplest first step before reaching for read replicas or sharding: a well-placed cache can eliminate the majority of database read load entirely.

## Denormalization
Deliberately duplicating some data (violating strict normalization, see `03-Databases/Normalization.md`) to avoid expensive joins at read time — a common trade-off at scale, accepting some write complexity/redundancy in exchange for much faster, simpler reads.

## Practical example
A social media app's database initially handles both reads and writes on one server (fine at small scale). As read traffic grows (users scrolling feeds constantly, but posting relatively rarely), the team adds 3 read replicas, routing all feed-loading queries to replicas and only writes (new posts, likes) to the primary — dramatically increasing read capacity without touching write capacity, which was never actually the bottleneck.
