# Data Structures for Backend Engineers

## What is it?
The fundamental ways to organize data in memory — arrays, linked lists, hash maps, trees, graphs, stacks, queues — each with different trade-offs for access, insertion, and deletion speed.

## Why care?
Backend engineering is constantly about choosing the right structure: how you store a cache, model a queue, or index a database all trace back to these basics. Also a core part of virtually every backend interview.

## The essentials, with Big-O

| Structure | Access | Search | Insert | Delete | Use case |
|---|---|---|---|---|---|
| **Array** | O(1) | O(n) | O(n) | O(n) | Fixed-size, fast indexed access |
| **Linked List** | O(n) | O(n) | O(1) | O(1) | Frequent insert/delete, unknown size |
| **Hash Map** | O(1) avg | O(1) avg | O(1) avg | O(1) avg | Fast key-based lookup (caches, indexes) |
| **Stack (LIFO)** | O(1) top | O(n) | O(1) | O(1) | Undo operations, call stacks, DFS |
| **Queue (FIFO)** | O(1) front | O(n) | O(1) | O(1) | Task queues, BFS, request buffering |
| **Binary Search Tree** | O(log n) avg | O(log n) avg | O(log n) avg | O(log n) avg | Ordered data, range queries |
| **Heap** | O(1) top | O(n) | O(log n) | O(log n) | Priority queues, scheduling |
| **Graph** | — | O(V+E) | O(1) | O(1) | Relationships/networks (social graphs, routes) |

## In backend engineering

- **Hash maps ARE how databases build indexes** and how in-memory caches (Redis) achieve O(1) lookups — understanding this explains why a well-indexed query is fast and an unindexed one is slow (`03-Databases/Indexing.md`).
- **Queues** are the literal data structure behind message queues (Kafka, RabbitMQ, SQS) — FIFO processing of background jobs (`07-Message-Queues-and-Async`).
- **B-Trees** (a generalization of binary search trees) are what most relational databases use internally to store indexes, because they stay balanced and minimize disk reads.
- **Graphs** model real backend problems directly: service dependency graphs, social networks, recommendation systems, routing/pathfinding (delivery/logistics APIs).
- **LRU (Least Recently Used) caches**, built from a hash map + doubly linked list, are a classic real interview question AND a genuinely common caching strategy used in production systems.

```python
# A classic backend interview problem: LRU Cache
from collections import OrderedDict

class LRUCache:
    def __init__(self, capacity):
        self.cache = OrderedDict()
        self.capacity = capacity

    def get(self, key):
        if key not in self.cache:
            return -1
        self.cache.move_to_end(key)
        return self.cache[key]

    def put(self, key, value):
        if key in self.cache:
            self.cache.move_to_end(key)
        self.cache[key] = value
        if len(self.cache) > self.capacity:
            self.cache.popitem(last=False)   # evict least recently used
```
