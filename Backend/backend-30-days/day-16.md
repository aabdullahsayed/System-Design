# Day 16: ORM Basics: EF Core & JPA/Hibernate

**Week 3 · Databases & Persistence**

## 🎯 Learning Objectives
- Map domain classes to database tables
- Perform CRUD via an ORM instead of raw SQL
- Understand the unit-of-work / change-tracking model

## 📚 Topics
- Entity mapping & fluent configuration / annotations
- DbContext / EntityManager lifecycle
- Change tracking and SaveChanges/persist-flush
- N+1 query problem

## 🟣 C# / .NET Focus
- Add `Microsoft.EntityFrameworkCore.PostgreSQL` (or SqlServer).
- Define `DbContext`, `DbSet<Book>`, fluent API in `OnModelCreating`.
- Use `dotnet ef migrations add InitialCreate`.

## 🟠 Java Focus
- Add `spring-boot-starter-data-jpa` + PostgreSQL driver.
- Define `@Entity` classes and a `JpaRepository<Book, Long>`.
- Let Hibernate auto-DDL in dev, or use Flyway (preview of Day 17).

## 🛠️ Hands-On Task
Replace your in-memory Bookstore storage with a real database using EF Core (C#) and Spring Data JPA (Java). Wire up the same CRUD endpoints from Week 2, now backed by Postgres, and verify data persists across app restarts.

## 🚀 Stretch Goal
Add an `Author` entity with a one-to-many relationship to `Book` and expose `GET /authors/{id}/books`.

## 🔗 Resources
- https://learn.microsoft.com/ef/core/
- https://spring.io/projects/spring-data-jpa

## ✅ Checklist
- [ ] Read/reviewed the topics above
- [ ] Completed the hands-on task in **C#**
- [ ] Completed the hands-on task in **Java**
- [ ] Committed code with a clear commit message
- [ ] (Optional) Attempted the stretch goal
- [ ] Wrote 2-3 sentences in your own words summarizing what you learned

---
[⬅ Previous Day](day-15.md) | [🏠 Home](README.md) | [Next Day ➡](day-17.md)
