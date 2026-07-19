# Day 15: SQL Fundamentals & Relational Design

**Week 3 · Databases & Persistence**

## 🎯 Learning Objectives
- Design a normalized relational schema
- Write correct SQL for common query patterns
- Understand keys, indexes, and constraints

## 📚 Topics
- Normalization (1NF–3NF)
- Primary/foreign keys, constraints
- JOINs, aggregates, subqueries
- Indexing basics

## 🟣 C# / .NET Focus
- Use SQL Server/PostgreSQL locally via Docker.
- Explore `sqlcmd`/`psql` CLI or a GUI (Azure Data Studio/DBeaver).

## 🟠 Java Focus
- Use PostgreSQL/MySQL locally via Docker.
- Explore `psql`/`mysql` CLI or DBeaver.

## 🛠️ Hands-On Task
Design a schema for the Bookstore domain (Books, Authors, Categories — many-to-many Books↔Authors) with proper keys and constraints. Write raw SQL to: create tables, seed sample data, and answer 5 queries (e.g., 'top 3 authors by book count', 'books with no category'). Run it against a real Postgres instance in Docker.

## 🚀 Stretch Goal
Add an `Orders`/`OrderItems` table set and write a query computing revenue per author.

## 🔗 Resources
- https://www.postgresql.org/docs/current/tutorial.html
- https://use-the-index-luke.com/

## ✅ Checklist
- [ ] Read/reviewed the topics above
- [ ] Completed the hands-on task in **C#**
- [ ] Completed the hands-on task in **Java**
- [ ] Committed code with a clear commit message
- [ ] (Optional) Attempted the stretch goal
- [ ] Wrote 2-3 sentences in your own words summarizing what you learned

---
[⬅ Previous Day](day-14.md) | [🏠 Home](README.md) | [Next Day ➡](day-16.md)
