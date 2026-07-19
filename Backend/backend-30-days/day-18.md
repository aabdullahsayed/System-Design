# Day 18: Transactions & Concurrency

**Week 3 · Databases & Persistence**

## 🎯 Learning Objectives
- Use transactions to preserve data integrity
- Understand isolation levels and their trade-offs
- Handle concurrent updates safely

## 📚 Topics
- ACID properties
- Isolation levels
- Optimistic vs pessimistic concurrency control
- Deadlocks and how to avoid them

## 🟣 C# / .NET Focus
- Use `DbContext.Database.BeginTransaction()` or implicit transactions in `SaveChanges`.
- Implement optimistic concurrency with a `[ConcurrencyCheck]`/`RowVersion` column.

## 🟠 Java Focus
- Use `@Transactional` on service methods.
- Implement optimistic locking with `@Version`.

## 🛠️ Hands-On Task
Simulate a 'purchase book' operation that decrements stock: implement it transactionally so partial failures roll back, and add optimistic concurrency control so two simultaneous purchases of the last copy don't both succeed. Write a quick test/script that fires concurrent requests to prove it.

## 🚀 Stretch Goal
Add a pessimistic-locking variant and compare behavior/performance to the optimistic version under load.

## 🔗 Resources
- https://learn.microsoft.com/ef/core/saving/transactions
- https://www.baeldung.com/jpa-optimistic-locking

## ✅ Checklist
- [ ] Read/reviewed the topics above
- [ ] Completed the hands-on task in **C#**
- [ ] Completed the hands-on task in **Java**
- [ ] Committed code with a clear commit message
- [ ] (Optional) Attempted the stretch goal
- [ ] Wrote 2-3 sentences in your own words summarizing what you learned

---
[⬅ Previous Day](day-17.md) | [🏠 Home](README.md) | [Next Day ➡](day-19.md)
