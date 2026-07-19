# SQL Fundamentals

## What is it?
SQL (Structured Query Language) is the standard language for querying and manipulating relational databases — reading, inserting, updating, and deleting data, plus defining schema structure.

## Why care?
SQL is one of the most durable, universally-applicable skills in all of software engineering — virtually unchanged in its core syntax for decades, and directly transferable across companies, industries, and even most database engines.

## CRUD operations
```sql
-- CREATE (insert)
INSERT INTO customers (name, email) VALUES ('Alice', 'alice@example.com');

-- READ (select)
SELECT name, email FROM customers WHERE id = 1;

-- UPDATE
UPDATE customers SET email = 'newemail@example.com' WHERE id = 1;

-- DELETE
DELETE FROM customers WHERE id = 1;
```

## SELECT — the query you'll write most
```sql
SELECT name, total
FROM orders
WHERE total > 100
ORDER BY total DESC
LIMIT 10;
```

## JOINs — combining data across tables (essential)
```sql
-- INNER JOIN: only rows with a match in BOTH tables
SELECT customers.name, orders.total
FROM customers
INNER JOIN orders ON customers.id = orders.customer_id;

-- LEFT JOIN: all rows from customers, matched orders if any exist (NULL if not)
SELECT customers.name, orders.total
FROM customers
LEFT JOIN orders ON customers.id = orders.customer_id;
```
Understanding the difference between INNER and LEFT JOIN (and when each is correct) is one of the most commonly tested practical SQL skills in interviews.

## Aggregation
```sql
SELECT customer_id, COUNT(*) as order_count, SUM(total) as total_spent
FROM orders
GROUP BY customer_id
HAVING SUM(total) > 1000        -- HAVING filters AFTER aggregation (WHERE filters BEFORE)
ORDER BY total_spent DESC;
```

## Subqueries and CTEs (Common Table Expressions)
```sql
-- Subquery
SELECT name FROM customers
WHERE id IN (SELECT customer_id FROM orders WHERE total > 500);

-- CTE (more readable for complex, multi-step queries)
WITH high_value_orders AS (
    SELECT customer_id FROM orders WHERE total > 500
)
SELECT name FROM customers WHERE id IN (SELECT customer_id FROM high_value_orders);
```

## Window functions (a more advanced, very powerful tool)
```sql
SELECT name, total,
       RANK() OVER (ORDER BY total DESC) as sales_rank
FROM orders;
```
Lets you compute rankings, running totals, and moving averages without collapsing rows via GROUP BY — essential for analytical/reporting queries.

## In backend engineering
- **N+1 query bugs** (`01-Programming-Fundamentals/Algorithms-Basics.md`) are fixed precisely by understanding JOINs — instead of looping and querying per record, one well-written JOIN retrieves everything needed in a single round-trip to the database.
- **Query performance debugging** starts with `EXPLAIN ANALYZE` (PostgreSQL) or `EXPLAIN` (MySQL) — shows you the database's actual execution plan, revealing whether it's using an index or doing a slow full table scan (`Indexing.md`).
