# Consensus Algorithms (Raft, Paxos)

## What is it?
Algorithms that allow a cluster of distributed nodes to agree on a single value/decision (e.g., "who is the current leader," "what is the next entry in this log") even when some nodes might fail or messages might be delayed — the foundational building block underlying reliable distributed coordination.

## Why care?
Nearly every piece of "reliable distributed infrastructure" you use as a backend engineer (etcd, ZooKeeper, Kubernetes' control plane, distributed databases like CockroachDB) is built on top of a consensus algorithm under the hood. Understanding the core idea demystifies how these systems achieve reliability despite individual node failures.

## The problem consensus solves
In a cluster of multiple nodes, how do you ensure they all agree on the same state, even if:
- Some nodes crash or become temporarily unreachable
- Network messages are delayed or arrive out of order
- Multiple nodes simultaneously think they should be "the leader"

Without a rigorous algorithm, you can end up with **split-brain** scenarios — two nodes both believing they're in charge, making conflicting decisions, corrupting data.

## Raft (the more approachable, widely-adopted modern algorithm)
Raft was explicitly designed to be more understandable than the earlier Paxos algorithm, while providing the same guarantees. Core ideas:
1. **Leader election**: nodes hold an election (using randomized timeouts to avoid ties) to choose a single leader among themselves.
2. **Log replication**: the leader accepts writes and replicates them, in order, to follower nodes.
3. **Majority quorum**: a write/decision is only considered committed once a MAJORITY of nodes have acknowledged it — this is what allows the system to tolerate a minority of node failures while still making safe progress. (e.g., in a 5-node cluster, the system can tolerate 2 nodes failing and still function correctly, since 3 nodes still form a majority.)

```
5-node cluster: [A(leader), B, C, D, E]
Write request → Leader A replicates to B, C, D, E
Once 3 of 5 (a majority, including A) acknowledge → write is "committed", safe to respond success to the client
If A crashes → remaining nodes hold a new election → one of B, C, D, E becomes the new leader → cluster continues operating
```

## Paxos (the original, historically foundational algorithm)
Paxos solves the same fundamental problem as Raft but is notoriously difficult to understand and implement correctly — it's historically important (many production systems, including early Google infrastructure, are built on Paxos or its variants like Multi-Paxos) but Raft has become the more common choice for NEW systems specifically because of its comparative clarity.

## Where you'll actually encounter this (you rarely implement it yourself)
- **etcd** (Kubernetes' backing store) uses Raft.
- **Consul** uses Raft for its consistency guarantees.
- **CockroachDB, TiDB** (distributed SQL databases) use Raft (per data shard/range) to keep replicas consistent.
- **ZooKeeper** uses a Paxos-like protocol (ZAB) for its own coordination guarantees.

**In practice, backend engineers almost never implement a consensus algorithm from scratch** — the immense value is in understanding the concept well enough to reason correctly about the guarantees (and failure behavior) of the distributed systems tools you use, which are built on these algorithms under the hood.

## Practical example
Understanding that etcd uses Raft explains WHY a Kubernetes cluster can tolerate losing a minority of its control-plane nodes without an outage (a majority quorum can still make progress), but WILL become unavailable for writes (though existing workloads keep running) if it loses a majority of control-plane nodes simultaneously — a direct, practical consequence of the majority-quorum requirement at the heart of Raft.
