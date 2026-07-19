# Concurrency & Parallelism in Backend Systems

## What is it?
**Concurrency**: handling multiple tasks by interleaving them (not necessarily simultaneously) — e.g., a single-threaded event loop juggling many I/O-bound requests. **Parallelism**: multiple tasks genuinely executing AT THE SAME TIME, typically across multiple CPU cores.

## Why care?
Backend servers must handle many simultaneous client requests efficiently — understanding these models explains why Node.js, Python (with asyncio), Java, and Go each handle concurrency so differently, and helps you choose/reason about the right approach for a given workload.

## I/O-bound vs CPU-bound (the key distinction)
- **I/O-bound work** (database queries, external API calls, file reads): the CPU is mostly IDLE, waiting on a slow external resource. Concurrency (not necessarily parallelism) is the right tool — while waiting on I/O, the server can work on other requests.
- **CPU-bound work** (image processing, complex calculations, data transformation): the CPU is genuinely busy computing. True PARALLELISM (multiple cores) is needed to speed this up — concurrency alone doesn't help if the CPU is the actual bottleneck.

## Common backend concurrency models
| Model | Example | How it works |
|---|---|---|
| **Thread-per-request** | Traditional Java servlets, older Apache | Each request gets a dedicated OS thread — simple, but doesn't scale to huge connection counts (thread overhead) |
| **Event loop (single-threaded, async)** | Node.js, Python asyncio | One thread handles many connections via non-blocking I/O — very efficient for I/O-bound workloads, but a CPU-bound task blocks EVERYTHING |
| **Multi-threaded event loop / thread pool + async** | ASP.NET Core, Go (goroutines + scheduler) | Combines async I/O efficiency with genuine multi-core parallelism for CPU work |
| **Actor model** | Erlang/Elixir, Akka (JVM) | Isolated "actors" communicate only via messages, no shared memory — sidesteps many classic concurrency bugs (race conditions) by design |

## The golden rule: never block the event loop
In event-loop-based servers (Node.js, Python asyncio), a single CPU-bound operation (e.g., a slow synchronous computation) blocks the ENTIRE server — no other requests can be processed until it finishes, since there's only one thread handling everything.
```python
# BAD in an async web server — blocks the entire event loop for everyone
def cpu_heavy_sync_function():
    result = 0
    for i in range(100_000_000):
        result += i
    return result

# GOOD — offload CPU-bound work to a separate process/worker pool
from concurrent.futures import ProcessPoolExecutor
executor = ProcessPoolExecutor()
result = await loop.run_in_executor(executor, cpu_heavy_sync_function)
```

## Race conditions in a backend context
Multiple requests modifying shared state concurrently (e.g., two simultaneous "decrement inventory" requests) can cause the exact race conditions covered in `06-Synchronization` of the OS roadmap — solved via database-level locking (`SELECT ... FOR UPDATE`), atomic database operations (`UPDATE inventory SET qty = qty - 1`), or distributed locks (e.g., using Redis) for cross-service coordination.

## Practical example
A Node.js API endpoint that resizes an uploaded image synchronously (CPU-bound) will freeze the ENTIRE server for every other concurrent user while processing that one image. The fix: offload image processing to a separate worker process (or a background job queue, see `07-Message-Queues-and-Async`), keeping the main event loop free to keep handling other I/O-bound requests (which is what it's actually good at) while the CPU-heavy work happens elsewhere.
