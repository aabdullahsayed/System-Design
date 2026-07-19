# Day 22: Databases: Relational Model & Indexing

**Week 4 · Systems, Design & Practice**

## 🎯 Learning Objectives
- Explain the relational model and normalization
- Understand how indexes speed up queries and their trade-offs
- Read and reason about a query execution plan at a high level

## 📚 Topics
- Tables, keys, normalization (1NF–3NF)
- SQL joins and their complexity implications
- B-tree indexes: why they speed up lookups
- Trade-offs of indexing (write cost, storage cost)

## 💡 Core Ideas
- An index is a separate, sorted data structure (usually a B-tree) that trades extra storage and slower writes for much faster reads on the indexed column(s).
- A join without a matching index degrades toward O(n*m) — this is the single most common cause of 'why is this query slow?'.
- Normalization reduces redundancy and update anomalies, but sometimes real systems deliberately denormalize for read performance — know both directions.

## 🛠️ Practice Exercises
- [ ] Set up a local database (SQLite is fine) with 2 related tables and ~10,000 rows of generated data.
- [ ] Run a query filtering on a non-indexed column, time it, add an index, and time it again.
- [ ] Use `EXPLAIN`/`EXPLAIN ANALYZE` to view the query plan before and after indexing and note what changed.

## 🎤 Interview-Style Questions
Try answering these out loud, without notes, before checking any reference:

- Why can adding an index make writes slower, and when would you accept that trade-off?
- What's the difference between a clustered and non-clustered index, conceptually?
- When would you deliberately denormalize a schema?

## 🔗 Resources
- https://use-the-index-luke.com/
- https://www.postgresql.org/docs/current/indexes.html

## ✅ Daily Checklist
- [ ] Read/reviewed the topics and core ideas above
- [ ] Completed all practice exercises
- [ ] Answered the interview-style questions out loud or in writing
- [ ] Committed any code to your repo
- [ ] Wrote 2-3 sentences in your own words summarizing the day's key insight

---
[⬅ Previous Day](day-21.md) | [🏠 Home](README.md) | [Next Day ➡](day-23.md)
