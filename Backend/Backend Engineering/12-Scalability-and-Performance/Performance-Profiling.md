# Performance Profiling for Backend Systems

## What is it?
Systematically measuring WHERE time is actually being spent in a backend system — a specific slow endpoint, a specific slow database query, a specific slow function — rather than guessing at optimizations.

## Why care?
"This feels slow" is not actionable. Profiling turns vague performance complaints into concrete, prioritized, evidence-based optimization work — and prevents wasting engineering time optimizing something that was never actually the bottleneck.

## The layered approach to finding a bottleneck
```
1. Is it the network? (client-to-server latency, DNS, TLS handshake)
2. Is it the application code? (slow algorithm, blocking I/O, N+1 queries)
3. Is it the database? (missing index, slow query, lock contention)
4. Is it a downstream dependency? (slow third-party API call)
5. Is it infrastructure? (CPU/memory/disk saturated, under-provisioned)
```
Work through this layer by layer with actual measurements — don't guess which layer is the problem.

## Application Performance Monitoring (APM) tools
Tools like **Datadog APM**, **New Relic**, **Elastic APM** automatically instrument your application to show exactly where time is spent within a request — a "flame graph" or "trace waterfall" showing, e.g., "this request took 800ms total: 50ms in your code, 700ms waiting on a database query, 50ms serializing the response."

## The N+1 query problem (the single most common backend performance bug)
```python
# BAD: 1 query to get orders, then N additional queries (one per order) to get each customer
orders = db.query("SELECT * FROM orders")
for order in orders:
    customer = db.query(f"SELECT * FROM customers WHERE id = {order.customer_id}")   # N+1!

# GOOD: 1 query total, using a JOIN
orders_with_customers = db.query("""
    SELECT orders.*, customers.name FROM orders
    JOIN customers ON orders.customer_id = customers.id
""")
```
This single pattern — accidentally issuing one query per row in a loop instead of one JOINed query — is responsible for an enormous share of real-world backend performance problems, especially common when using ORMs that make it easy to accidentally trigger lazy-loaded queries inside a loop.

## Load testing (measuring under realistic, simulated traffic)
```bash
# Using a tool like wrk or k6 to simulate concurrent load
wrk -t4 -c100 -d30s http://localhost:8080/api/orders
```
Load testing BEFORE a real traffic spike (e.g., before a big product launch or sale event) reveals bottlenecks under realistic concurrent load that wouldn't show up testing with a single request at a time.

## Database query profiling
```sql
EXPLAIN ANALYZE SELECT * FROM orders WHERE customer_id = 123;
```
Shows the database's actual execution plan — whether it's using an index efficiently or doing a slow full table scan — the single most important diagnostic tool for a slow query (see `03-Databases/Indexing.md`).

## Practical example
An API endpoint that takes 3 seconds to respond. APM tracing reveals: 50ms application code, 2.8 seconds in database queries — specifically, `EXPLAIN ANALYZE` reveals it's a full table scan on a 2-million-row table because of a missing index on the `customer_id` column used in the WHERE clause. Adding the index drops the query (and the whole endpoint) to under 50ms — a concrete example of profiling correctly identifying the ACTUAL bottleneck (database, not application code) before spending time on the wrong optimization.
