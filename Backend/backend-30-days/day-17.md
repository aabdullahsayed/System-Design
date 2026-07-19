# Day 17: Migrations & Schema Management

**Week 3 · Databases & Persistence**

## 🎯 Learning Objectives
- Version-control database schema changes
- Apply and roll back migrations safely
- Avoid destructive migration pitfalls

## 📚 Topics
- Migration tools and workflow
- Forward-only vs reversible migrations
- Seeding reference data
- Team workflow for schema changes

## 🟣 C# / .NET Focus
- Use EF Core Migrations (`dotnet ef migrations add`, `dotnet ef database update`).
- Understand generated migration files and how to hand-edit them.

## 🟠 Java Focus
- Use Flyway or Liquibase (`V1__init.sql`, `V2__add_authors.sql`).
- Understand versioned vs repeatable migrations.

## 🛠️ Hands-On Task
Introduce a breaking schema change (e.g., split `Book.Author` string into a proper `Author` table) via a migration in both stacks. Write the migration, apply it to a fresh database, and verify existing seed data survives (write a data-backfill step in the migration).

## 🚀 Stretch Goal
Simulate a rollback: write the down-migration (EF) or a compensating migration (Flyway/Liquibase) and test it.

## 🔗 Resources
- https://learn.microsoft.com/ef/core/managing-schemas/migrations/
- https://flywaydb.org/documentation/

## ✅ Checklist
- [ ] Read/reviewed the topics above
- [ ] Completed the hands-on task in **C#**
- [ ] Completed the hands-on task in **Java**
- [ ] Committed code with a clear commit message
- [ ] (Optional) Attempted the stretch goal
- [ ] Wrote 2-3 sentences in your own words summarizing what you learned

---
[⬅ Previous Day](day-16.md) | [🏠 Home](README.md) | [Next Day ➡](day-18.md)
