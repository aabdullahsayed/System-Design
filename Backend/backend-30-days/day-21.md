# Day 21: Week 3 Capstone: Persistent Bookstore API

**Week 3 · Databases & Persistence**

## 🎯 Learning Objectives
- Integrate relational + NoSQL + caching into one cohesive backend
- Practice writing repository-layer tests against a real database

## 📚 Topics
- Repository pattern
- Test containers for integration tests
- Data layer code review checklist

## 🟣 C# / .NET Focus
- Use Testcontainers for .NET to spin up Postgres/Mongo/Redis in tests.
- Structure a clean `Repositories` layer separate from `Services`.

## 🟠 Java Focus
- Use Testcontainers for Java (`org.testcontainers`) to spin up Postgres/Mongo/Redis in tests.
- Structure a clean repository layer using Spring Data interfaces.

## 🛠️ Hands-On Task
Finalize your Bookstore API with: Postgres for core data, MongoDB for reviews, Redis for caching, all wired via migrations, DI, and DTOs from Weeks 1–2. Write at least 3 integration tests (using Testcontainers) that spin up real databases and verify a full create→read→update→delete flow.

## 🚀 Stretch Goal
Add a `/stats` endpoint aggregating data across all three stores (e.g., book count, avg review rating, cache hit rate).

## 🔗 Resources
- https://testcontainers.com/
- https://learn.microsoft.com/ef/core/testing/

## ✅ Checklist
- [ ] Read/reviewed the topics above
- [ ] Completed the hands-on task in **C#**
- [ ] Completed the hands-on task in **Java**
- [ ] Committed code with a clear commit message
- [ ] (Optional) Attempted the stretch goal
- [ ] Wrote 2-3 sentences in your own words summarizing what you learned

---
[⬅ Previous Day](day-20.md) | [🏠 Home](README.md) | [Next Day ➡](day-22.md)
