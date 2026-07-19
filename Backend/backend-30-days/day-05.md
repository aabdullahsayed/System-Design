# Day 5: Exception Handling & Logging

**Week 1 · Language & OOP Foundations**

## 🎯 Learning Objectives
- Handle errors predictably and safely
- Design custom exception types
- Set up structured logging

## 📚 Topics
- try/catch/finally, checked vs unchecked exceptions
- Custom exception hierarchies
- Fail-fast vs defensive programming
- Structured logging fundamentals

## 🟣 C# / .NET Focus
- All exceptions are unchecked; use `try/catch/finally` and `using` for disposal.
- Create custom exceptions inheriting from `Exception`.
- Set up `Serilog` or `Microsoft.Extensions.Logging`.

## 🟠 Java Focus
- Understand checked vs unchecked exceptions and when to use each.
- Create custom exceptions extending `RuntimeException` or `Exception`.
- Set up SLF4J with Logback.

## 🛠️ Hands-On Task
Build a small `FileParser` that reads a CSV of numbers and throws a custom `InvalidRecordException` on bad rows, logging each failure with row number and reason, while still processing valid rows. Log a summary at the end (parsed vs failed count).

## 🚀 Stretch Goal
Add retry logic (3 attempts with backoff) for a simulated flaky operation.

## 🔗 Resources
- https://learn.microsoft.com/dotnet/standard/exceptions/
- https://docs.oracle.com/javase/tutorial/essential/exceptions/

## ✅ Checklist
- [ ] Read/reviewed the topics above
- [ ] Completed the hands-on task in **C#**
- [ ] Completed the hands-on task in **Java**
- [ ] Committed code with a clear commit message
- [ ] (Optional) Attempted the stretch goal
- [ ] Wrote 2-3 sentences in your own words summarizing what you learned

---
[⬅ Previous Day](day-04.md) | [🏠 Home](README.md) | [Next Day ➡](day-06.md)
