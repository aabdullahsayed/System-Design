# Practice — Databases

1. Design a normalized (3NF) schema for a simple e-commerce system: `customers`, `products`, `orders`, `order_items` — identify primary and foreign keys.
2. Write SQL to create these tables, then insert sample data (at least 5 customers, 10 products, 20 orders).
3. Write a query joining `customers`, `orders`, and `order_items` to find each customer's total spend, sorted descending.
4. Run `EXPLAIN ANALYZE` on that query before and after adding an index on `orders.customer_id` — compare the execution plans.
5. Deliberately reproduce an N+1 query problem in code (loop over customers, querying orders one customer at a time), then fix it with a single JOIN query — compare the number of database round-trips.
6. Wrap a two-step "transfer balance between two accounts" operation in an explicit transaction, and verify (by forcing an error partway through) that a failure rolls back both steps, not just one.
7. Research: pick one NoSQL database (MongoDB, Redis, or Cassandra) and write 3-4 sentences on what specific problem it's best suited for, and why a relational database would be a worse fit for that specific use case.

✅ Done? Move to `04-APIs-and-Web-Fundamentals`.
