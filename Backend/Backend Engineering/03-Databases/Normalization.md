# Database Normalization

## What is it?
A set of design principles for structuring relational tables to minimize data redundancy and prevent update anomalies, organized into progressive "normal forms" (1NF, 2NF, 3NF, etc.).

## Why care?
Poor schema design (unnormalized, redundant data scattered across a table) leads to real bugs: update one copy of a customer's address and forget the other 5 copies, and now your data is inconsistently wrong. Normalization is the discipline that prevents this class of bug.

## The normal forms (practical summary, not full academic rigor)

### 1NF — Atomic values, no repeating groups
```
BAD:  orders(id, customer_name, products="Widget,Gadget,Gizmo")   -- comma-separated list in one field
GOOD: separate an order_items table, one row per product per order
```

### 2NF — No partial dependency on a composite key
Every non-key column must depend on the ENTIRE primary key, not just part of it (relevant when a table has a composite/multi-column primary key).

### 3NF — No transitive dependency
```
BAD:  orders(id, customer_id, customer_email)   -- customer_email depends on customer_id, not directly on the order
GOOD: keep customer_email only in the customers table; orders just references customer_id
```
This is the most commonly-applied normal form in practice — the general rule of thumb is "every column should depend on the key, the whole key, and nothing but the key."

## Why normalize
- **Eliminates redundancy**: a customer's email lives in exactly ONE place — update it once, correctly, everywhere.
- **Prevents update anomalies**: no risk of the same fact (e.g., a customer's name) being inconsistently updated in some rows but not others.
- **Smaller storage footprint**: less duplicated data.

## Why (and when) to deliberately denormalize
Normalization optimizes for data integrity, but can require more JOINs at read time, which can hurt performance for read-heavy workloads. **Denormalization** (deliberately duplicating some data) trades some redundancy for faster reads — a legitimate, common optimization, but should be a deliberate choice with a clear reason, not a default starting point.

```sql
-- Normalized: order_total computed via JOIN + SUM every time it's needed
-- Denormalized: store a computed `order_total` column directly on the orders table,
--               updated whenever line items change — trades write complexity for read speed
```

## In backend engineering
- **Most backend schemas should start normalized (3NF)** — it's the safer default, preventing an entire category of data-integrity bugs.
- **Denormalize deliberately, and document why**, typically after identifying an actual, measured performance bottleneck from excessive JOINs — not preemptively "for speed" without evidence.
- **Read-heavy analytics/reporting systems** (data warehouses) often deliberately use denormalized schemas (star/snowflake schemas — see the Math-For-ML or Excel roadmap's Data Model concepts) since their workload is fundamentally different from a transactional application database.
