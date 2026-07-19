# NoSQL Databases

## What is it?
A broad category of non-relational databases, each trading some of relational databases' strict structure/consistency guarantees for specific benefits: flexible schemas, horizontal scalability, or specialized data models.

## Why care?
Not every problem fits neatly into tables and JOINs — knowing which NoSQL category solves which real problem (and knowing when to just use a relational database instead) is a genuine, practical architecture decision backend engineers make regularly.

## The main categories

### Document stores (MongoDB, Couchbase)
Store flexible, JSON-like documents instead of rigid rows — great when your data's shape varies between records or nests naturally.
```json
{
  "_id": "123",
  "name": "Alice",
  "orders": [
    {"product": "Widget", "qty": 2},
    {"product": "Gadget", "qty": 1}
  ]
}
```
Use case: content management systems, product catalogs with varying attributes, rapidly evolving schemas early in a product's life.

### Key-Value stores (Redis, DynamoDB)
The simplest model: a key maps directly to a value, optimized for extremely fast lookups.
```
GET user:123:session  →  "{ loggedIn: true, expiresAt: ... }"
```
Use case: caching (`06-Caching`), session storage, real-time leaderboards, rate limiting counters.

### Wide-column stores (Cassandra, HBase, Bigtable)
Optimized for massive write throughput and horizontal scale across many servers, with a flexible column structure per row.
Use case: time-series data, IoT sensor data, systems needing to ingest huge, continuous write volumes across distributed regions.

### Graph databases (Neo4j, Amazon Neptune)
Optimized for traversing relationships (edges) between entities (nodes) — directly modeling networks.
Use case: social networks, recommendation engines, fraud detection (finding suspicious connection patterns).

## NoSQL vs Relational — the real decision framework
```
Choose Relational (default) when:  Data is structured, relationships matter, you need
                                     strong consistency/transactions (payments, inventory).

Choose NoSQL when:                 Schema is genuinely variable/evolving, you need
                                     massive horizontal write scale, or your access
                                     pattern is fundamentally key-value/graph-shaped.
```
**A common real mistake**: choosing NoSQL prematurely "for scale" before actually needing it — most applications, even fairly large ones, run comfortably on a well-indexed relational database for years. Don't add NoSQL complexity without a specific, identified need.

## In backend engineering
- **Polyglot persistence** is common in mature systems: a relational database for core transactional data, Redis for caching/sessions, and maybe Elasticsearch for full-text search — using the right specialized tool for each specific access pattern, rather than forcing one database to do everything.
- **CAP theorem** (`08-System-Design-Basics/CAP-Theorem.md`) explains the fundamental trade-offs many NoSQL databases make (often favoring Availability/Partition-tolerance over strict Consistency) — understanding this explains WHY certain NoSQL databases behave the way they do under network partitions.
