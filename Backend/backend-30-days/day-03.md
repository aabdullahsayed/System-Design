# Day 3: Inheritance, Polymorphism & Interfaces

**Week 1 · Language & OOP Foundations**

## 🎯 Learning Objectives
- Design class hierarchies correctly
- Use interfaces/abstract classes to define contracts
- Apply polymorphism to write extensible code

## 📚 Topics
- Inheritance and the `virtual`/`override` (C#) vs default `override` (Java) model
- Abstract classes vs interfaces
- Multiple interface implementation
- SOLID: Liskov Substitution & Interface Segregation in practice

## 🟣 C# / .NET Focus
- Use `abstract class` and `override` keyword; understand `sealed` classes.
- Define interfaces with default implementations (C# 8+ default interface methods).
- Explore `IComparable<T>`, `IEquatable<T>`.

## 🟠 Java Focus
- Use `abstract class` and `@Override` annotation.
- Define interfaces with `default` methods.
- Explore `Comparable<T>`, `equals()`/`hashCode()` contracts.

## 🛠️ Hands-On Task
Design a small shape hierarchy: an abstract `Shape` with `Area()`/`Perimeter()`, concrete `Circle`, `Rectangle`, `Triangle`. Add an `IShapeFormatter`/`ShapeFormatter` interface with two implementations (plain text and JSON-like string). Write a Main that polymorphically prints a list of shapes.

## 🚀 Stretch Goal
Add a `Sortable` capability so shapes can be sorted by area using `IComparable`/`Comparable`.

## 🔗 Resources
- https://learn.microsoft.com/dotnet/csharp/fundamentals/object-oriented/inheritance
- https://docs.oracle.com/javase/tutorial/java/IandI/

## ✅ Checklist
- [ ] Read/reviewed the topics above
- [ ] Completed the hands-on task in **C#**
- [ ] Completed the hands-on task in **Java**
- [ ] Committed code with a clear commit message
- [ ] (Optional) Attempted the stretch goal
- [ ] Wrote 2-3 sentences in your own words summarizing what you learned

---
[⬅ Previous Day](day-02.md) | [🏠 Home](README.md) | [Next Day ➡](day-04.md)
