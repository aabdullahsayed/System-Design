# Scalability Concepts

## What is it?
The ability of a system to handle increasing load (more users, more data, more requests) by adding resources — the central concern of system design.

## Why care?
Code that works perfectly for 100 users can completely fail at 100,000 users, for entirely different reasons than typical bugs — scalability thinking is what separates "a working prototype" from "a production system," and is the core subject of system design interviews.

## Vertical vs Horizontal Scaling
```
Vertical Scaling (Scale UP):     Add more power to ONE server (more CPU, RAM, faster disk)
Horizontal Scaling (Scale OUT):   Add MORE servers, distributing load across them
```
| | Vertical | Horizontal |
|---|---|---|
| Complexity | Simple — no code changes needed | Requires stateless design, load balancing |
| Ceiling | Hard physical/cost limit on a single machine | Effectively unlimited (add more machines) |
| Fault tolerance | Single point of failure | Can survive individual server failures |
| Cost | Gets exponentially expensive at the high end | More linear, commodity hardware |

**Modern systems favor horizontal scaling** for its higher ceiling and fault tolerance — but it requires designing your application to be **stateless** (no server-specific in-memory state that a request depends on) so any server instance can handle any request.

## Stateless vs Stateful services
```python
# Stateful (BAD for horizontal scaling) — depends on in-memory state on THIS specific server
session_store = {}   # lives only in this one process's memory
def login(user_id):
    session_store[user_id] = {"logged_in": True}   # lost if this server restarts, invisible to OTHER servers

# Stateless (GOOD) — state lives in a shared, external store (Redis, database)
def login(user_id):
    redis.set(f"session:{user_id}", {"logged_in": True})   # any server instance can read this
```
This is precisely why JWTs (`05-Authentication-and-Security/JWT.md`) and Redis-backed sessions (`06-Caching/Redis.md`) are so common in scalable backend architectures — they externalize state out of individual application servers.

## Bottleneck identification (the core system design skill)
Every system has a bottleneck — the ONE component that limits overall throughput. Common bottlenecks:
```
Database:     often the first, most common bottleneck (see Database-Scaling.md)
CPU-bound work:  heavy computation on the application server itself
Network I/O:      slow calls to external services/APIs
Disk I/O:           especially relevant for logging, file storage
```
Scaling the WRONG component (e.g., adding more application servers when the database is actually the bottleneck) doesn't help — real system design work starts with identifying the actual bottleneck via monitoring/profiling (`13-Observability`), not guessing.

## In backend engineering
- **Premature scaling is a real, common mistake**: building for "millions of users" complexity when you have 100 real users adds unnecessary cost and engineering time — scale deliberately, based on actual or realistically anticipated load, not hypothetical future scale.
- **The "scale as you grow" mindset**: most successful systems start simple (a single server, a single database) and incrementally add complexity (caching, read replicas, horizontal scaling) as REAL bottlenecks are identified — not all at once, upfront.
