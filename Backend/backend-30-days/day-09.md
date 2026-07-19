# Day 9: Your First REST API

**Week 2 · Web Fundamentals & REST APIs**

## 🎯 Learning Objectives
- Stand up a minimal web API server
- Understand the framework's request pipeline
- Return JSON responses from an endpoint

## 📚 Topics
- ASP.NET Core Minimal APIs / Controllers
- Spring Boot auto-configuration
- Running and testing locally

## 🟣 C# / .NET Focus
- `dotnet new webapi -n Bookstore.Api`.
- Explore Minimal API syntax: `app.MapGet("/books", ...)`.
- Run with `dotnet run` and test via Swagger UI.

## 🟠 Java Focus
- Use Spring Initializr (start.spring.io) to generate a project with `spring-boot-starter-web`.
- Create a `@RestController` with a `@GetMapping("/books")`.
- Run with `mvn spring-boot:run`.

## 🛠️ Hands-On Task
Implement GET `/books` (returns a hardcoded in-memory list) and GET `/books/{id}` in both frameworks, matching the design from Day 8. Verify both return correct status codes for found/not-found.

## 🚀 Stretch Goal
Add GET `/books/search?title=...` with basic case-insensitive matching.

## 🔗 Resources
- https://learn.microsoft.com/aspnet/core/tutorials/min-web-api
- https://spring.io/guides/gs/rest-service/

## ✅ Checklist
- [ ] Read/reviewed the topics above
- [ ] Completed the hands-on task in **C#**
- [ ] Completed the hands-on task in **Java**
- [ ] Committed code with a clear commit message
- [ ] (Optional) Attempted the stretch goal
- [ ] Wrote 2-3 sentences in your own words summarizing what you learned

---
[⬅ Previous Day](day-08.md) | [🏠 Home](README.md) | [Next Day ➡](day-10.md)
