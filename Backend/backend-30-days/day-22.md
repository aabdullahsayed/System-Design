# Day 22: Authentication & Authorization (JWT/OAuth2)

**Week 4 · Production Readiness**

## 🎯 Learning Objectives
- Implement token-based authentication
- Enforce role/claim-based authorization
- Understand OAuth2/OpenID Connect at a conceptual level

## 📚 Topics
- Password hashing (bcrypt/argon2)
- JWT structure & validation
- Role-based and policy-based authorization
- OAuth2/OIDC overview (not full implementation)

## 🟣 C# / .NET Focus
- Use ASP.NET Core Identity or a custom user store with BCrypt.Net.
- Issue JWTs with `Microsoft.AspNetCore.Authentication.JwtBearer`.
- Use `[Authorize(Roles = "Admin")]`.

## 🟠 Java Focus
- Use Spring Security with BCryptPasswordEncoder.
- Issue JWTs with `jjwt` or Spring Security OAuth2 Resource Server.
- Use `@PreAuthorize("hasRole('ADMIN')")`.

## 🛠️ Hands-On Task
Add user registration/login to your Bookstore API with hashed passwords and JWT-based auth. Protect write endpoints (create/update/delete book) to require an `Admin` role, while read endpoints stay public. Write test requests proving unauthorized/forbidden responses work correctly.

## 🚀 Stretch Goal
Add refresh tokens with rotation and a `/logout` that revokes them.

## 🔗 Resources
- https://learn.microsoft.com/aspnet/core/security/authentication/
- https://docs.spring.io/spring-security/reference/index.html

## ✅ Checklist
- [ ] Read/reviewed the topics above
- [ ] Completed the hands-on task in **C#**
- [ ] Completed the hands-on task in **Java**
- [ ] Committed code with a clear commit message
- [ ] (Optional) Attempted the stretch goal
- [ ] Wrote 2-3 sentences in your own words summarizing what you learned

---
[⬅ Previous Day](day-21.md) | [🏠 Home](README.md) | [Next Day ➡](day-23.md)
