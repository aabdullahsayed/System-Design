# Day 26: Design Patterns

**Week 4 · Systems, Design & Practice**

## 🎯 Learning Objectives
- Recognize the most common design patterns and the problems they solve
- Avoid over-applying patterns where simpler code would do
- Read patterns in existing codebases/frameworks

## 📚 Topics
- Creational patterns: Factory, Singleton, Builder
- Structural patterns: Adapter, Decorator, Facade
- Behavioral patterns: Strategy, Observer, Command
- When NOT to use a pattern (over-engineering)

## 💡 Core Ideas
- Every design pattern exists to solve a *specific* recurring problem — learn the problem first, then the pattern falls out naturally; memorizing pattern names without the 'why' doesn't stick.
- Singleton and Factory are the most overused patterns in junior code — often a simple function or dependency injection solves the same problem more simply.
- Frameworks you already use are full of these patterns (e.g., Strategy in sorting comparators, Observer in event listeners, Decorator in middleware) — spotting them in real code cements understanding better than toy examples.

## 🛠️ Practice Exercises
- [ ] Implement the Strategy pattern: a `Sorter` that can plug in different comparison strategies at runtime.
- [ ] Implement the Observer pattern: a simple event system where multiple listeners react to one event.
- [ ] Find one design pattern already used in a framework/library you use daily (e.g., your web framework, your language's collections) and write 2–3 sentences identifying it.

## 🎤 Interview-Style Questions
Try answering these out loud, without notes, before checking any reference:

- What problem does the Factory pattern solve that a plain constructor doesn't?
- Give an example of over-applying a design pattern and why it made code worse, not better.
- How is Dependency Injection related to (and different from) the Strategy pattern?

## 🔗 Resources
- https://refactoring.guru/design-patterns
- https://sourcemaking.com/design_patterns

## ✅ Daily Checklist
- [ ] Read/reviewed the topics and core ideas above
- [ ] Completed all practice exercises
- [ ] Answered the interview-style questions out loud or in writing
- [ ] Committed any code to your repo
- [ ] Wrote 2-3 sentences in your own words summarizing the day's key insight

---
[⬅ Previous Day](day-25.md) | [🏠 Home](README.md) | [Next Day ➡](day-27.md)
