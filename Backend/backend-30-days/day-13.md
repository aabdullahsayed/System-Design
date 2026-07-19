# Day 13: Configuration & Environment Management

**Week 2 · Web Fundamentals & REST APIs**

## 🎯 Learning Objectives
- Manage per-environment configuration safely
- Avoid hardcoding secrets
- Bind configuration to strongly-typed objects

## 📚 Topics
- appsettings.json / application.yml
- Environment variables & secrets
- Strongly-typed configuration binding
- Profiles (Dev/Staging/Prod)

## 🟣 C# / .NET Focus
- Use `appsettings.json` + `appsettings.Development.json`.
- Bind with `IOptions<T>`.
- Use `dotnet user-secrets` for local secrets.

## 🟠 Java Focus
- Use `application.yml`/`application.properties` with Spring Profiles.
- Bind with `@ConfigurationProperties`.
- Use environment variables or `.env` + Spring Cloud Config for secrets.

## 🛠️ Hands-On Task
Externalize a `BookstoreSettings` (e.g., max page size, feature flag `EnableSearch`) into configuration files, bind it to a strongly-typed class, inject it via DI, and prove that switching environment/profile changes behavior without code changes.

## 🚀 Stretch Goal
Add a `/health` endpoint that reports current environment/profile and app version.

## 🔗 Resources
- https://learn.microsoft.com/dotnet/core/extensions/configuration
- https://docs.spring.io/spring-boot/reference/features/external-config.html

## ✅ Checklist
- [ ] Read/reviewed the topics above
- [ ] Completed the hands-on task in **C#**
- [ ] Completed the hands-on task in **Java**
- [ ] Committed code with a clear commit message
- [ ] (Optional) Attempted the stretch goal
- [ ] Wrote 2-3 sentences in your own words summarizing what you learned

---
[⬅ Previous Day](day-12.md) | [🏠 Home](README.md) | [Next Day ➡](day-14.md)
