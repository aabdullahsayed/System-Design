# CAP Theorem

## What is it?
A fundamental theoretical result stating that a DISTRIBUTED data system can only fully guarantee TWO of the following three properties simultaneously, during a network partition:
- **Consistency (C)**: every read receives the most recent write (or an error) — all nodes see the same data at the same time.
- **Availability (A)**: every request receives a (non-error) response, even if some nodes are down/unreachable.
- **Partition Tolerance (P)**: the system continues to operate despite network failures splitting it into disconnected groups of nodes.

## Why care?
This theorem explains WHY different databases make fundamentally different design trade-offs (why MongoDB behaves differently from PostgreSQL under network issues, why some NoSQL systems favor availability over strict consistency) — genuinely foundational distributed systems knowledge, and a very common system design interview topic.

## The key insight: Partition tolerance isn't really optional
In any real distributed system spanning multiple machines/data centers, network partitions (temporary connectivity failures between nodes) WILL eventually happen — so in practice, the real choice is between **C and A** during a partition, not a free choice among all three; P is essentially a given for any genuinely distributed system.

## CP systems (Consistency + Partition tolerance, sacrifice Availability)
During a partition, the system may refuse some requests (become unavailable for affected nodes) rather than risk returning inconsistent/stale data.
Examples: traditional relational databases configured for strong consistency, HBase, MongoDB (in its default configuration), ZooKeeper.
Use case: financial systems, inventory systems — where returning WRONG data (e.g., showing stock available when it's actually sold out) is worse than briefly being unavailable.

## AP systems (Availability + Partition tolerance, sacrifice strict Consistency)
During a partition, the system keeps responding to requests on both sides of the partition, accepting that different nodes might temporarily disagree — resolved via "eventual consistency" once the partition heals.
Examples: Cassandra, DynamoDB, CouchDB (in typical configurations).
Use case: social media feeds, shopping carts, product catalogs — where briefly showing slightly stale data is an acceptable trade-off for the system always being responsive.

## This is a SPECTRUM in practice, not a rigid binary choice
Real systems often let you TUNE this trade-off per-operation (not just pick one global setting) — e.g., Cassandra lets you specify a "consistency level" per query (how many replica nodes must acknowledge a write/read before it's considered successful), letting different parts of the same application make different trade-offs based on their specific needs.

## PACELC — a useful extension worth knowing
CAP only describes behavior DURING a partition. **PACELC** extends this: even when there's no partition, there's still a trade-off between Latency and Consistency (strongly consistent reads/writes typically require more coordination, hence more latency) — a more complete, practically useful framework for reasoning about real distributed database trade-offs.

## In backend engineering
- **Choosing a database isn't just about features — it's about which consistency/availability trade-off matches your actual business requirement.** A banking ledger needs strong consistency (CP-leaning); a "likes" counter on a social post can tolerate eventual consistency (AP-leaning) in exchange for the system always feeling responsive.
- **This theorem is frequently invoked (sometimes loosely) in system design interviews** — being able to correctly explain the trade-off, and correctly identify which side a given real-world requirement should favor, is a strong, evaluated signal of distributed systems understanding.
