# Persistence, Replication & Clustering

## Persistence options

### RDB (snapshotting)
Point-in-time binary snapshots written periodically (fork + copy-on-write).
```
save 900 1      # snapshot if >=1 change in 900s
save 300 10
save 60 10000
```
- Pros: compact, fast restarts, good for backups.
- Cons: you lose data since the last snapshot on crash.

### AOF (Append Only File)
Logs every write command; replays on restart.
```
appendonly yes
appendfsync everysec   # always | everysec | no
```
- `always`: safest, slowest. `everysec`: lose at most ~1s (common default). `no`: OS decides.
- Background **rewrite** compacts the file.
- Pros: better durability. Cons: larger files, slower restarts than RDB.

### Combined (recommended for durability)
Enable both: AOF for durability, RDB for fast backups. In modern versions AOF uses a multi-part format with an RDB preamble.

### No persistence
Pure cache; fastest; data gone on restart.

## Backups
- Copy `dump.rdb` (or use `BGSAVE`) to remote storage regularly.
- Test restores. Persistence is not a backup strategy by itself.

## Replication
Asynchronous leader (primary) -> replicas.
```
# on replica
replicaof 10.0.0.1 6379
```
- Replicas serve reads (watch for stale reads) and provide failover candidates.
- Initial sync via RDB transfer, then command stream; partial resync using a replication backlog.
- `WAIT numreplicas timeout` can force acknowledgment from replicas (still not fully strong consistency).
- Async replication means a failover can lose the most recent writes.

## High availability with Sentinel
- Separate Sentinel processes (run **3 or more**, odd number) monitor primary/replicas.
- They detect failures by quorum, **promote a replica**, and tell clients the new primary address.
- Good for a single-shard setup that needs automatic failover but not horizontal scaling.

## Redis Cluster (sharding)
- Data split into **16384 hash slots**; each primary owns a subset.
- Slot = `CRC16(key) mod 16384`.
- Each primary can have replicas; cluster fails over automatically.
- Minimum practical setup: **3 primaries** (+ replicas, typically 6 nodes).
- Clients must be cluster-aware and follow `MOVED`/`ASK` redirections.

### Hash tags
Force related keys into the same slot using `{}`:
```
user:{42}:profile
user:{42}:cart     # both hash on "42"
```
Needed for multi-key operations (`MGET`, transactions, Lua) in Cluster.

### Cluster limitations
- Multi-key commands only work for keys in the same slot.
- Only database 0.
- Resharding moves slots between nodes (online but adds load).
- Hot keys/hot slots can overload one node.

## Choosing a topology
| Need | Choose |
|------|--------|
| Dev / simple cache, small data | Standalone |
| Read scaling + backup copy | Primary + replicas |
| Automatic failover, data fits one node | Primary + replicas + Sentinel |
| Dataset/throughput exceeds one node | Redis Cluster |
| Managed, no ops | AWS ElastiCache / MemoryDB, Azure Cache, Google Memorystore, Redis Cloud |

## Memory management
- Check: `INFO memory`, `MEMORY USAGE key`, `MEMORY STATS`.
- Reduce memory: compact encodings (small hashes/lists/sets use listpack/intset), shorter keys, proper TTLs, avoid huge values, use hashes for many small objects, compression client-side.
- Watch fragmentation ratio (`mem_fragmentation_ratio`); enable active defrag if needed.
- Leave headroom for fork copy-on-write during RDB/AOF rewrite (RAM can spike).
