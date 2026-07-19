# Database Scaling (Replication & Sharding)

## What is it?
Techniques for scaling a database beyond what a single database server can handle — through **replication** (copying data across multiple servers) and **sharding** (splitting data across multiple servers).

## Why care?
The database is very often the first real bottleneck in a growing system (`Scalability-Concepts.md`) — application servers are relatively easy to scale horizontally, but databases have historically been much harder, making database scaling a genuinely important, frequently-tested system design topic.

## Replication — copying data for read scaling and fault tolerance
```
Primary (Writer) ──replicates to──→ Replica 1 (Reader)
                  ──replicates to──→ Replica 2 (Reader)
```
- **All writes go to the Primary.**
- **Reads can be distributed across Replicas**, dramatically increasing read capacity (common for read-heavy workloads, which describes the majority of real applications).
- **Replication lag**: replicas are updated asynchronously (usually), meaning a read from a replica might momentarily see slightly stale data compared to the primary — an "eventual consistency" trade-off that application logic sometimes needs to account for (e.g., "read your own writes" problems, where a user updates something and immediately doesn't see their own change reflected on a lagging replica).
- **Failover**: if the primary fails, a replica can be promoted to become the new primary — providing fault tolerance, not just read scaling.

## Sharding — splitting data across multiple databases
```
Shard 1: users with id 1-1,000,000
Shard 2: users with id 1,000,001-2,000,000
Shard 3: users with id 2,000,001-3,000,000
```
Unlike replication (every replica has ALL the data), sharding splits the data itself — each shard holds only a SUBSET, enabling you to scale WRITE capacity too (each shard independently handles writes for its own subset), not just reads.

### Sharding strategies
- **Range-based**: split by a range of values (e.g., user ID 1-1M, 1M-2M) — simple, but can create "hot shards" if certain ranges get disproportionate traffic.
- **Hash-based**: split by a hash of a key (e.g., `hash(user_id) % num_shards`) — distributes load more evenly, but makes range queries across shards harder.
- **Geographic**: split by user region — useful for data residency requirements and reducing latency, common for globally-distributed applications.

### The real cost of sharding
Sharding is powerful but adds SIGNIFICANT complexity: cross-shard JOINs become very difficult or impossible, transactions spanning multiple shards lose easy ACID guarantees, and rebalancing shards as data grows unevenly is genuinely hard operational work. **Sharding is usually a last resort**, reached for only after replication, caching, and query optimization have been exhausted — not a default starting architecture.

## Connection pooling (a simpler, often-overlooked scaling lever)
Before reaching for replication/sharding, ensure you're using a connection pool (reusing a limited set of database connections across requests, rather than opening a new one per request) — opening database connections is relatively expensive, and connection pooling is a simple, high-leverage optimization most ORMs/frameworks provide out of the box.

## In backend engineering
A realistic scaling progression: **(1)** optimize queries and add indexes, **(2)** add caching (`06-Caching`), **(3)** add read replicas for read-heavy load, **(4)** only reach for sharding once you've genuinely outgrown what a single, well-replicated primary database (with caching) can handle — most systems, even fairly large ones, never actually need to shard.
