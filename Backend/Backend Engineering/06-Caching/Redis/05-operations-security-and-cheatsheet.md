# Operations, Security & Cheat Sheet

## Performance tips
- Use **pipelining** or `MGET`/`MSET` to cut network round trips.
- Reuse connections with a **connection pool**; avoid connect-per-request.
- Avoid O(N) commands on big collections (`KEYS`, `SMEMBERS`, `HGETALL`, `LRANGE 0 -1`); use `SCAN`, `SSCAN`, `HSCAN`, `ZSCAN`.
- Avoid big keys/values; split large structures.
- Use `UNLINK` instead of `DEL` for big keys (async free).
- Set TTLs on cache data; choose an eviction policy.
- Watch for hot keys and slow commands.
- Keep values compact (short field names, serialization formats like MessagePack/Protobuf).

## Monitoring
```bash
INFO                 # all stats  (INFO memory, INFO stats, INFO replication, INFO clients)
SLOWLOG GET 10       # slowest commands
LATENCY DOCTOR
MONITOR              # live command stream (debug only, impacts performance)
CLIENT LIST
redis-cli --bigkeys  # find large keys
redis-cli --latency
```

Key metrics to track:
| Metric | Why |
|--------|-----|
| `used_memory` / `maxmemory` | Capacity |
| `mem_fragmentation_ratio` | Wasted memory |
| `evicted_keys` | Cache pressure |
| `keyspace_hits` / `keyspace_misses` | Cache hit ratio |
| `connected_clients`, `blocked_clients` | Connection issues |
| `instantaneous_ops_per_sec` | Throughput |
| `rejected_connections` | Hitting `maxclients` |
| `master_link_status`, replication lag | Replication health |
| `rdb_last_bgsave_status`, `aof_last_write_status` | Persistence health |
| CPU, network I/O, latency (p99) | Saturation |

Hit ratio = hits / (hits + misses). Export via **redis_exporter** to Prometheus/Grafana.

## Security
- **Never expose Redis directly to the internet.** Bind to private interfaces; use firewalls/VPC/security groups.
- Enable **ACLs** (Redis 6+): per-user passwords and command/key permissions.
```
ACL SETUSER app on >strongpassword ~app:* +get +set +del +expire
```
- Disable or rename dangerous commands (`FLUSHALL`, `FLUSHDB`, `CONFIG`, `KEYS`, `DEBUG`, `SHUTDOWN`) for app users.
- Enable **TLS** for client and replication traffic.
- Use `protected-mode yes` and strong auth; rotate credentials.
- Run as a non-root user; restrict file permissions on RDB/AOF files.
- Keep Redis patched; Lua/script and module features have had CVEs.

## Configuration essentials (`redis.conf`)
```
bind 127.0.0.1 10.0.0.5
port 6379
requirepass <strong-password>          # or use ACLs
maxmemory 2gb
maxmemory-policy allkeys-lru
appendonly yes
appendfsync everysec
save 900 1
tcp-keepalive 300
timeout 0
maxclients 10000
```

## Client usage snippets
**Python (redis-py)**
```python
import redis
r = redis.Redis(host="localhost", port=6379, decode_responses=True)
r.set("k", "v", ex=60)
print(r.get("k"))
with r.pipeline() as p:
    p.incr("hits"); p.expire("hits", 3600)
    p.execute()
```
**Node.js (ioredis)**
```js
import Redis from "ioredis";
const redis = new Redis({ host: "localhost", port: 6379 });
await redis.set("k", "v", "EX", 60);
console.log(await redis.get("k"));
```

## Common pitfalls
- Running `KEYS *` or huge `SMEMBERS` in production
- No `maxmemory` set, so the box runs out of RAM
- Using Redis as the sole source of truth without persistence/backups
- Assuming replication means no data loss (it is asynchronous)
- No TTLs on cache keys, causing unbounded growth
- Exposing the port publicly with no auth
- Using Pub/Sub when you need durable delivery
- Heavy single Lua scripts or slow commands blocking the server
- Multi-key operations in Cluster without hash tags
- Forgetting fork memory overhead during snapshots

## Redis vs alternatives (quick)
- **Memcached**: simpler, multithreaded pure cache.
- **Valkey / KeyDB / Dragonfly**: Redis-compatible alternatives (Valkey is a community fork; Dragonfly is multi-threaded).
- **Kafka/RabbitMQ**: heavier-duty messaging than Redis lists/pub/sub; Streams cover lighter cases.
- **DynamoDB / Postgres**: durable primary databases.

## Interview one-liners
- *Why is Redis fast?* In-memory, efficient data structures, simple protocol, single-threaded command execution avoids locking overhead, I/O multiplexing.
- *RDB vs AOF?* RDB = periodic snapshots (compact, may lose recent data); AOF = write log (better durability, bigger).
- *Sentinel vs Cluster?* Sentinel = failover for one shard; Cluster = sharding plus failover.
- *How to avoid cache stampede?* Locking/single-flight, jittered TTLs, stale-while-revalidate.
- *Pub/Sub vs Streams?* Pub/Sub is ephemeral fan-out; Streams are durable with consumer groups and replay.
- *Is Redis single-threaded?* Command execution is; I/O threads, background persistence, and lazy free use others.
- *Is Redis strongly consistent?* Not in replicated/cluster mode; replication is asynchronous.
- *Eviction policy for a cache?* `allkeys-lru` or `allkeys-lfu`.
