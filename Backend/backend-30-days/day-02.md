# Day 2: OOP Basics: Classes & Encapsulation

**Week 1 · Language & OOP Foundations**

## 🎯 Learning Objectives
- Model real-world entities using classes
- Apply encapsulation with access modifiers
- Understand constructors and object initialization

## 📚 Topics
- Classes, fields, properties/getters-setters
- Access modifiers: public/private/protected/internal vs public/private/protected/package-private
- Constructors and object initialization
- Value types vs reference types (C# structs vs Java primitives/objects)

## 🟣 C# / .NET Focus
- Use auto-properties (`public string Name { get; set; }`).
- Explore `record` types for immutable data models.
- Understand `struct` vs `class` semantics.

## 🟠 Java Focus
- Use private fields with explicit getters/setters (or Lombok `@Getter`/`@Setter` if using Gradle/Maven with Lombok).
- Understand `final` fields for immutability.
- Compare primitive types vs wrapper classes (int vs Integer).

## 🛠️ Hands-On Task
Model a `BankAccount` class in both languages with private balance, a constructor requiring an initial deposit ≥ 0, and `Deposit`/`Withdraw` methods that enforce invariants (no negative balance). Write a small Main/Program that demonstrates valid and invalid operations.

## 🚀 Stretch Goal
Add a `Currency` value object and make `BankAccount` immutable-friendly by returning new state where appropriate.

## 🔗 Resources
- https://learn.microsoft.com/dotnet/csharp/fundamentals/object-oriented/
- https://docs.oracle.com/javase/tutorial/java/javaOO/

## ✅ Checklist
- [ ] Read/reviewed the topics above
- [ ] Completed the hands-on task in **C#**
- [ ] Completed the hands-on task in **Java**
- [ ] Committed code with a clear commit message
- [ ] (Optional) Attempted the stretch goal
- [ ] Wrote 2-3 sentences in your own words summarizing what you learned

---
[⬅ Previous Day](day-01.md) | [🏠 Home](README.md) | [Next Day ➡](day-03.md)
