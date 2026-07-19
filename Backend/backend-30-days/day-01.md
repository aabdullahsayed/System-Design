# Day 1: Environment Setup & Hello World

**Week 1 · Language & OOP Foundations**

## 🎯 Learning Objectives
- Install and verify your backend toolchain for both stacks
- Understand project/solution structure
- Run your first console program in each language

## 📚 Topics
- SDK/JDK installation and version managers
- Project vs solution (C#) and Maven/Gradle project (Java)
- Package managers: NuGet vs Maven Central
- IDE setup: VS Code/Rider/Visual Studio vs IntelliJ IDEA/Eclipse

## 🟣 C# / .NET Focus
- Install .NET SDK (latest LTS). Run `dotnet --version`.
- Create a console app: `dotnet new console -n HelloBackend`
- Understand `Program.cs`, top-level statements, and the `.csproj` file.
- Run with `dotnet run`.

## 🟠 Java Focus
- Install a JDK (Temurin 21 recommended) and a build tool (Maven or Gradle).
- Create a project: `mvn archetype:generate` or use IntelliJ's new project wizard.
- Understand `pom.xml`/`build.gradle`, `src/main/java` layout.
- Run with `mvn compile exec:java` or your IDE run button.

## 🛠️ Hands-On Task
Create a 'Hello, Backend' console app in BOTH C# and Java that prints your name, the current date, and the runtime version. Commit both to a Git repo with a clear folder structure (`/csharp`, `/java`).

## 🚀 Stretch Goal
Add a `.gitignore` for both stacks (bin/obj for C#, target/ for Java) and write a short README explaining how to run each.

## 🔗 Resources
- https://learn.microsoft.com/dotnet/core/get-started
- https://dev.java/learn/getting-started/

## ✅ Checklist
- [ ] Read/reviewed the topics above
- [ ] Completed the hands-on task in **C#**
- [ ] Completed the hands-on task in **Java**
- [ ] Committed code with a clear commit message
- [ ] (Optional) Attempted the stretch goal
- [ ] Wrote 2-3 sentences in your own words summarizing what you learned

---
[🏠 Home](README.md) | [Next Day ➡](day-02.md)
