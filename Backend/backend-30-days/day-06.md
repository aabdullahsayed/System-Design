# Day 6: File I/O & JSON Serialization

**Week 1 · Language & OOP Foundations**

## 🎯 Learning Objectives
- Read and write files safely
- Serialize/deserialize objects to/from JSON
- Handle streams and resource cleanup

## 📚 Topics
- File/stream reading and writing
- JSON serialization libraries
- Resource management (using/try-with-resources)
- Working with configuration files

## 🟣 C# / .NET Focus
- Use `System.Text.Json` for serialization.
- Read/write files with `File.ReadAllTextAsync`/`StreamReader`.
- Use `using` statements for automatic disposal.

## 🟠 Java Focus
- Use `Jackson` or `Gson` for serialization.
- Read/write files with `Files.readString`/`BufferedReader`.
- Use try-with-resources for automatic closing.

## 🛠️ Hands-On Task
Write a program that serializes a list of `Product{Id, Name, Price, Tags[]}` objects to a JSON file, then reads it back, filters products under a price threshold, and writes the filtered list to a new JSON file. Handle missing-file and malformed-JSON errors gracefully.

## 🚀 Stretch Goal
Add support for reading the same data from a CSV as an alternate input format.

## 🔗 Resources
- https://learn.microsoft.com/dotnet/standard/serialization/system-text-json/
- https://github.com/FasterXML/jackson

## ✅ Checklist
- [ ] Read/reviewed the topics above
- [ ] Completed the hands-on task in **C#**
- [ ] Completed the hands-on task in **Java**
- [ ] Committed code with a clear commit message
- [ ] (Optional) Attempted the stretch goal
- [ ] Wrote 2-3 sentences in your own words summarizing what you learned

---
[⬅ Previous Day](day-05.md) | [🏠 Home](README.md) | [Next Day ➡](day-07.md)
