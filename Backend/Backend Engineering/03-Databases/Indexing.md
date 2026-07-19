# Database Indexing

## What is it?
An **index** is a separate data structure (typically a B-Tree) that lets the database find rows matching a condition without scanning the entire table — the single biggest lever for query performance.

## Why care?
The difference between a query that takes 5ms and one that takes 5 seconds, on the same data, is almost always about whether the right index exists. This is one of the most practically important, frequently-debugged backend performance topics.

## How it works (conceptually)
Without an index, finding `WHERE email = 'alice@example.com'` requires scanning EVERY row (a "full table scan," O(n)). An index on `email` builds a sorted structure (like a phone book) that lets the database jump almost directly to the matching row (O(log n)) — see `01-Programming-Fundamentals/Data-Structures.md` for the underlying B-Tree structure.

## Creating an index
```sql
CREATE INDEX idx_customers_email ON customers(email);

-- Composite index (covers queries filtering on BOTH columns, in this order)
CREATE INDEX idx_orders_customer_date ON orders(customer_id, created_at);
```

## Primary keys and unique constraints are automatically indexed
Every table's primary key gets an index automatically — this is why looking up a row by its ID is always fast, without any manual indexing work.

## When an index WON'T help
- **Low-cardinality columns** (e.g., a boolean `is_active` column with only 2 possible values) — an index provides little benefit since it doesn't narrow the search much.
- **Small tables** — a full scan of a few hundred rows is already fast; the index overhead isn't worth it.
- **Queries using functions on the indexed column** (e.g., `WHERE LOWER(email) = '...'`) — a plain index on `email` won't be used; you'd need a function-based/expression index instead.
- **Leading wildcard searches** (`WHERE name LIKE '%smith'`) — standard B-Tree indexes can't help here (they CAN help with `LIKE 'smith%'`, trailing wildcard).

## The trade-off: indexes aren't free
Every index speeds up READS but slows down WRITES (INSERT/UPDATE/DELETE), because the index itself must also be updated on every write, and indexes consume additional disk space. **Don't index every column "just in case"** — index based on your actual, observed query patterns.

## Diagnosing missing indexes
```sql
EXPLAIN ANALYZE SELECT * FROM orders WHERE customer_id = 42;
```
Look for **"Seq Scan"** (sequential/full table scan — bad, for a large table) vs **"Index Scan"** (good) in the output — this is a standard, essential debugging step whenever a query feels slower than expected.

## Practical example
An `orders` table with 10 million rows and no index on `customer_id`: `SELECT * FROM orders WHERE customer_id = 42` scans all 10 million rows every single time (potentially seconds). Adding `CREATE INDEX idx_orders_customer_id ON orders(customer_id);` turns this into a near-instant, indexed lookup — a single line of SQL that can transform a production application's performance.
