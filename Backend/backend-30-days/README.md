# 30-Day Backend Developer Roadmap (C# & Java)

A structured, hands-on plan to go from solid fundamentals to a production-shaped backend system — built **twice**, side by side, in **C# (.NET)** and **Java (Spring Boot)**. By Day 30 you'll have two portfolio-ready "Bookstore" backend systems with real databases, auth, messaging, tests, Docker, and CI/CD.

## Why build the same project in both languages?

Comparing idioms side by side is one of the fastest ways to *actually* understand a concept instead of memorizing syntax. Each day's file gives you the same task with language-specific guidance for both stacks — do both, or pick your primary language and skim the other for context.

## How this is organized

```
backend-30-days/
├── README.md          ← you are here
├── PROGRESS.md         ← printable/checkable tracker for all 30 days
├── day-01.md ... day-30.md
```

Each `day-XX.md` file follows the same template:

- **🎯 Learning Objectives** — what you should be able to do by the end of the day
- **📚 Topics** — concepts to read up on
- **🟣 C# / .NET Focus** — stack-specific guidance
- **🟠 Java Focus** — stack-specific guidance
- **🛠️ Hands-On Task** — the actual thing you build today (this is where the learning happens)
- **🚀 Stretch Goal** — optional extra for when you finish early
- **🔗 Resources** — official docs to go deeper
- **✅ Checklist** — tick off as you go

## The 30-Day Arc

| Week | Days | Theme |
|---|---|---|
| 1 | 1–7 | Language & OOP Foundations |
| 2 | 8–14 | Web Fundamentals & REST APIs |
| 3 | 15–21 | Databases & Persistence |
| 4 | 22–30 | Production Readiness (auth, testing, observability, messaging, Docker, CI/CD) |

By the end of each week you build a small capstone (Day 7, 14, 21) and Day 30 is the final capstone tying the whole system together.

## Suggested pace

- **~1–2 hours/day** if you already know one of the two languages.
- **~2–3 hours/day** if both are new to you — it's fine to slow down in Week 1.
- Treat each day as a checkpoint, not a hard deadline. Skipping a stretch goal is fine; skipping the hands-on task is not — that's where the actual skill is built.

## Prerequisites

- Basic programming experience in *any* language (variables, loops, functions)
- A computer that can run Docker (for Weeks 3–4)
- Git installed and a GitHub account
- Curiosity and willingness to read error messages carefully 🙂

## What you'll have by Day 30

- Two working backend systems (C# and Java) implementing the same "Bookstore" domain
- Real REST APIs with validation, DTOs, and consistent error handling
- Relational (PostgreSQL) + document (MongoDB) + cache (Redis) persistence
- JWT authentication and role-based authorization
- Unit + integration test suites
- Structured logging, health checks, and metrics
- Async messaging between two decomposed microservices
- Dockerized, Compose-orchestrated local environment
- A working GitHub Actions CI/CD pipeline
- A `SYSTEM_DESIGN.md` you can talk through in an interview

## Tips for success

1. **Code along, don't just read.** Type every example yourself.
2. **Commit daily.** A visible commit history is part of the portfolio.
3. **Write your own summary** at the end of each day (there's a checklist item for it) — explaining a concept in your own words is how you find out if you actually understood it.
4. **Don't skip the capstones.** They're where scattered daily lessons become one coherent system.
5. **It's okay to repeat a day.** If Day 16 (ORM) doesn't click, spend an extra day there before moving on — the plan is a guide, not a law.

Good luck — start with [Day 1](day-01.md) 🚀
