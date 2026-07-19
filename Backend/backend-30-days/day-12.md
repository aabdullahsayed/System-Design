# Day 12: Dependency Injection

**Week 2 · Web Fundamentals & REST APIs**

## 🎯 Learning Objectives
- Understand the DI container and its lifecycle scopes
- Refactor controllers to depend on abstractions
- Register and resolve services correctly

## 📚 Topics
- Constructor injection
- Service lifetimes: Transient/Scoped/Singleton (C#) vs default singleton beans (Java)
- Interfaces for testability

## 🟣 C# / .NET Focus
- Register services in `Program.cs`: `builder.Services.AddScoped<IBookService, BookService>();`.
- Understand Transient vs Scoped vs Singleton.

## 🟠 Java Focus
- Use `@Service`, `@Component`, `@Autowired`/constructor injection.
- Understand Spring's default singleton scope vs `@Scope("prototype")`.

## 🛠️ Hands-On Task
Refactor your Bookstore controller to delegate all logic to an injected `IBookService`/`BookService` interface, with an in-memory implementation. Write a second alternate implementation (e.g., a 'slow logging' decorator) and swap it via configuration to prove the abstraction works.

## 🚀 Stretch Goal
Add a logging decorator/wrapper service registered via DI that wraps every service call with timing logs.

## 🔗 Resources
- https://learn.microsoft.com/dotnet/core/extensions/dependency-injection
- https://docs.spring.io/spring-framework/reference/core/beans/dependencies.html

## ✅ Checklist
- [ ] Read/reviewed the topics above
- [ ] Completed the hands-on task in **C#**
- [ ] Completed the hands-on task in **Java**
- [ ] Committed code with a clear commit message
- [ ] (Optional) Attempted the stretch goal
- [ ] Wrote 2-3 sentences in your own words summarizing what you learned

---
[⬅ Previous Day](day-11.md) | [🏠 Home](README.md) | [Next Day ➡](day-13.md)
