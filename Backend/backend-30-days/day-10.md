# Day 10: Routing, Controllers & DTOs

**Week 2 · Web Fundamentals & REST APIs**

## 🎯 Learning Objectives
- Structure controllers cleanly
- Separate internal models from API contracts using DTOs
- Implement full CRUD routing

## 📚 Topics
- Route attributes/annotations
- DTO vs domain model separation
- Object mapping between DTO and entity
- Implementing POST/PUT/PATCH/DELETE

## 🟣 C# / .NET Focus
- Use `[ApiController]`, `[Route]`, `[HttpPost]` etc.
- Create separate `BookDto`/`CreateBookRequest` records.
- Manually map or use a small mapping helper method.

## 🟠 Java Focus
- Use `@RestController`, `@RequestMapping`, `@PostMapping` etc.
- Create separate `BookDto`/`CreateBookRequest` classes.
- Manually map or use MapStruct for DTO↔entity mapping.

## 🛠️ Hands-On Task
Extend your Bookstore API with full CRUD: POST (create), PUT (full update), PATCH (partial update), DELETE. Use DTOs for request/response, never exposing your internal model directly. Return correct status codes (201 Created with Location header, 204 No Content, etc.).

## 🚀 Stretch Goal
Add a `BookSummaryDto` (fewer fields) used only for the list endpoint, vs a full `BookDetailDto` for single-item GET.

## 🔗 Resources
- https://learn.microsoft.com/aspnet/core/web-api/
- https://spring.io/guides/tutorials/rest/

## ✅ Checklist
- [ ] Read/reviewed the topics above
- [ ] Completed the hands-on task in **C#**
- [ ] Completed the hands-on task in **Java**
- [ ] Committed code with a clear commit message
- [ ] (Optional) Attempted the stretch goal
- [ ] Wrote 2-3 sentences in your own words summarizing what you learned

---
[⬅ Previous Day](day-09.md) | [🏠 Home](README.md) | [Next Day ➡](day-11.md)
