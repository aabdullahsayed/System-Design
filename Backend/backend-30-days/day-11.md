# Day 11: Validation & Error Handling Middleware

**Week 2 · Web Fundamentals & REST APIs**

## 🎯 Learning Objectives
- Validate incoming requests declaratively
- Centralize error handling
- Return consistent, informative error responses

## 📚 Topics
- Data annotations / Bean Validation
- Global exception handling middleware
- Problem Details (RFC 7807) / consistent error shape
- 400 vs 422 vs 500 semantics

## 🟣 C# / .NET Focus
- Use Data Annotations (`[Required]`, `[Range]`) or FluentValidation.
- Implement exception-handling middleware or `IExceptionFilter`.
- Return `ProblemDetails` responses.

## 🟠 Java Focus
- Use Bean Validation (`@NotNull`, `@Size`) with `@Valid`.
- Implement `@ControllerAdvice` + `@ExceptionHandler`.
- Return a consistent error response shape.

## 🛠️ Hands-On Task
Add validation rules to your Book create/update DTOs (title required, price > 0, ISBN format). Implement a global error handler that converts validation failures and custom exceptions into a consistent JSON error format with a trace/correlation id.

## 🚀 Stretch Goal
Add a `NotFoundException` and `ConflictException` mapped to 404 and 409 respectively via the global handler.

## 🔗 Resources
- https://learn.microsoft.com/aspnet/core/web-api/handle-errors
- https://www.baeldung.com/exception-handling-for-rest-with-spring

## ✅ Checklist
- [ ] Read/reviewed the topics above
- [ ] Completed the hands-on task in **C#**
- [ ] Completed the hands-on task in **Java**
- [ ] Committed code with a clear commit message
- [ ] (Optional) Attempted the stretch goal
- [ ] Wrote 2-3 sentences in your own words summarizing what you learned

---
[⬅ Previous Day](day-10.md) | [🏠 Home](README.md) | [Next Day ➡](day-12.md)
