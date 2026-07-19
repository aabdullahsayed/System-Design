# Day 23: Unit Testing & Mocking

**Week 4 · Production Readiness**

## 🎯 Learning Objectives
- Write focused unit tests for business logic
- Use mocks/stubs to isolate dependencies
- Apply the AAA (Arrange-Act-Assert) pattern

## 📚 Topics
- Unit testing frameworks
- Mocking frameworks
- Test naming & structure conventions
- Code coverage — usefulness and limits

## 🟣 C# / .NET Focus
- Use xUnit (or NUnit) + Moq.
- Write tests for `BookService` mocking `IBookRepository`.

## 🟠 Java Focus
- Use JUnit 5 + Mockito.
- Write tests for `BookService` mocking `BookRepository`.

## 🛠️ Hands-On Task
Write a comprehensive unit test suite (15+ tests) for your `BookService`/`BookService` class covering happy paths, validation failures, and not-found scenarios — all with the repository mocked, no real database involved. Run a coverage report and aim for meaningful coverage of business logic (not just line count).

## 🚀 Stretch Goal
Add parameterized/data-driven tests for edge cases (empty title, negative price, boundary values).

## 🔗 Resources
- https://xunit.net/
- https://junit.org/junit5/docs/current/user-guide/

## ✅ Checklist
- [ ] Read/reviewed the topics above
- [ ] Completed the hands-on task in **C#**
- [ ] Completed the hands-on task in **Java**
- [ ] Committed code with a clear commit message
- [ ] (Optional) Attempted the stretch goal
- [ ] Wrote 2-3 sentences in your own words summarizing what you learned

---
[⬅ Previous Day](day-22.md) | [🏠 Home](README.md) | [Next Day ➡](day-24.md)
