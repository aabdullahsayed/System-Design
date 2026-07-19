# Day 4: Collections & Generics

**Week 1 · Language & OOP Foundations**

## 🎯 Learning Objectives
- Choose the right collection type for a problem
- Write type-safe generic code
- Understand time complexity of common collection operations

## 📚 Topics
- List/ArrayList, Dictionary/HashMap, Set/HashSet, Queue, Stack
- Generics and type constraints
- LINQ vs Java Streams for data transformation
- Big-O intuition for common operations

## 🟣 C# / .NET Focus
- Use `List<T>`, `Dictionary<TKey,TValue>`, `HashSet<T>`.
- Write a generic method/class with constraints (`where T : IComparable<T>`).
- Use LINQ: `Where`, `Select`, `GroupBy`, `OrderBy`.

## 🟠 Java Focus
- Use `ArrayList`, `HashMap`, `HashSet`, `TreeMap`.
- Write a generic method/class with bounded types (`<T extends Comparable<T>>`).
- Use Streams: `filter`, `map`, `collect`, `groupingBy`.

## 🛠️ Hands-On Task
Given a list of `Order{Id, Customer, Amount, Date}` objects (hardcode ~20 sample records), write code in both languages to: total revenue per customer, top 3 orders by amount, and orders grouped by month — using LINQ in C# and Streams in Java.

## 🚀 Stretch Goal
Benchmark `List` vs `Dictionary`/`HashMap` lookup for 100k items and note the difference.

## 🔗 Resources
- https://learn.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/
- https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html

## ✅ Checklist
- [ ] Read/reviewed the topics above
- [ ] Completed the hands-on task in **C#**
- [ ] Completed the hands-on task in **Java**
- [ ] Committed code with a clear commit message
- [ ] (Optional) Attempted the stretch goal
- [ ] Wrote 2-3 sentences in your own words summarizing what you learned

---
[⬅ Previous Day](day-03.md) | [🏠 Home](README.md) | [Next Day ➡](day-05.md)
