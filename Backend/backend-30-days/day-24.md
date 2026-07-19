# Day 24: Integration & API Testing

**Week 4 · Production Readiness**

## 🎯 Learning Objectives
- Test the full HTTP request/response pipeline
- Spin up a test server in-process
- Assert on real database interactions

## 📚 Topics
- In-memory/test server hosting
- Integration test database strategy
- Contract testing overview

## 🟣 C# / .NET Focus
- Use `WebApplicationFactory<T>` for in-process integration tests.
- Use Testcontainers-backed Postgres per test class/collection.

## 🟠 Java Focus
- Use `@SpringBootTest` + `MockMvc` or `TestRestTemplate`.
- Use Testcontainers-backed Postgres via `@Testcontainers`.

## 🛠️ Hands-On Task
Write integration tests that boot your actual Bookstore API (in-process) and hit real HTTP endpoints, asserting on status codes and response bodies for the full CRUD flow, including auth-protected endpoints from Day 22 (test both authorized and unauthorized calls).

## 🚀 Stretch Goal
Add a simple contract test that validates your OpenAPI spec matches actual responses (e.g., using a schema validator).

## 🔗 Resources
- https://learn.microsoft.com/aspnet/core/test/integration-tests
- https://docs.spring.io/spring-boot/reference/testing/spring-boot-applications.html

## ✅ Checklist
- [ ] Read/reviewed the topics above
- [ ] Completed the hands-on task in **C#**
- [ ] Completed the hands-on task in **Java**
- [ ] Committed code with a clear commit message
- [ ] (Optional) Attempted the stretch goal
- [ ] Wrote 2-3 sentences in your own words summarizing what you learned

---
[⬅ Previous Day](day-23.md) | [🏠 Home](README.md) | [Next Day ➡](day-25.md)
