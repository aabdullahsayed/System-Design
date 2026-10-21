# Redis: Introduction

## What is Redis?
Redis (**RE**mote **DI**ctionary **S**erver) is an open-source, in-memory data structure store. It is used as a cache, database, message broker, and streaming engine. Data lives in RAM, so reads and writes typically take well under a millisecond.

## Key characteristics
- **In-memory**: extremely fast; dataset must fit in RAM (or across a cluster).
- **Rich data structures**: not just key-value strings.
- **Single-threaded command execution**: commands run one at a time, so each command is atomic. (Modern Redis uses extra threads for I/O, not for command execution.)
- **Optional persistence**: RDB snapshots and AOF log.
- **Replication, high availability, and sharding**: Replicas, Sentinel, Cluster.
- **Atomic operations, transactions, Lua scripts, pub/sub, streams.**
- Simple text protocol (**RESP**); clients in nearly every language.

## Common use cases
| Use case | How |
|----------|-----|
| Caching | Strings/hashes with TTL |
| Session store | Hash or string with expiry |
| Rate limiting | `INCR` + `EXPIRE`, sliding window with sorted sets |
| Leaderboards | Sorted sets |
| Job/message queues | Lists, Streams |
| Real-time pub/sub | Pub/Sub, Streams |
| Distributed locks | `SET key value NX PX` |
| Counters / analytics | `INCR`, HyperLogLog, bitmaps |
| Geospatial queries | GEO commands |
| Search / vectors | Redis Query Engine / vector search |

## Redis vs other stores
| | Redis | Memcached | Relational DB |
|---|---|---|---|
| Data model | Rich structures | Simple key-value | Tables, SQL |
| Persistence | Optional | None | Yes |
| Replication / clustering | Built in | Client-side sharding | Varies |
| Speed | Sub-ms | Sub-ms | ms |
| Best for | Cache + structures + messaging | Pure simple cache | Durable relational data |

## Limitations
- Memory is expensive; dataset size bounded by RAM.
- Not a replacement for a primary durable database for most workloads (durability is configurable but weaker than a typical RDBMS).
- Single-threaded commands: a slow command blocks everyone.
- No rich ad-hoc querying/joins like SQL.
- Multi-key operations in Cluster must hit the same hash slot.

## Quick start
```bash
# run with Docker
docker run -d --name redis -p 6379:6379 redis:latest

# connect
redis-cli
127.0.0.1:6379> PING
PONG
127.0.0.1:6379> SET greeting "hello"
OK
127.0.0.1:6379> GET greeting
"hello"
```

## Licensing note
Redis changed its license in 2024 (source-available) and added AGPL as an option in later versions. Forks such as **Valkey** (Linux Foundation) and compatible alternatives exist. Check current licensing for your use case.
