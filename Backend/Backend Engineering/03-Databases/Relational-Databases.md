# Relational Databases

## What is it?
A relational database organizes data into **tables** (rows and columns) with defined relationships between them, enforced by a schema — the traditional, still-dominant model for structured backend data (PostgreSQL, MySQL, SQL Server, Oracle).

## Why care?
Most backend systems, across virtually every industry, persist their core data in a relational database. Understanding tables, relationships, and schema design is foundational, non-optional backend knowledge.

## Core concepts
- **Table**: a collection of rows, each with the same defined columns (e.g., a `users` table).
- **Row (record)**: one entry in a table (one specific user).
- **Column (field)**: an attribute of every row (e.g., `email`, `created_at`).
- **Primary Key**: a column (or combination) uniquely identifying each row — every table should have one.
- **Foreign Key**: a column referencing another table's primary key, defining a relationship.

## Relationships
```
One-to-Many:   One Customer  →  Many Orders     (most common relationship type)
Many-to-Many:  Many Students ↔ Many Courses      (needs a "junction table" in between)
One-to-One:    One User      →  One UserProfile    (less common, sometimes used to split rarely-accessed columns)
```

### Example schema
```sql
CREATE TABLE customers (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100) UNIQUE
);

CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    customer_id INT REFERENCES customers(id),   -- foreign key
    total DECIMAL(10,2),
    created_at TIMESTAMP DEFAULT NOW()
);
```

## Why the relational model has lasted
- **Data integrity**: foreign keys and constraints prevent invalid data (e.g., an order for a customer that doesn't exist).
- **ACID transactions** (`Transactions-ACID.md`): guaranteed consistency even under concurrent access and failures.
- **Mature tooling and query language (SQL)**: decades of optimization, tooling, and a genuinely powerful, well-understood query language.

## In backend engineering
- **ORMs** (SQLAlchemy, Prisma, Entity Framework, Hibernate) let you interact with relational tables using your programming language's objects instead of raw SQL — useful for productivity, but understanding the underlying SQL/relational model is still essential for debugging performance issues an ORM might hide.
- **Schema migrations** (versioned, incremental changes to your database structure, applied via tools like Alembic, Flyway, or Prisma Migrate) are how real teams evolve a production database schema safely over time without downtime or data loss.
- **Choosing relational vs NoSQL** (`NoSQL-Databases.md`) is one of the most consequential early architecture decisions in a backend system — relational is usually the right DEFAULT choice unless you have a specific reason (extreme write scale, flexible/unstructured schema needs) to reach for NoSQL.
