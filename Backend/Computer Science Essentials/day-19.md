# Day 19: Memory Management & Concurrency Patterns

**Week 3 · Advanced Algorithms & Systems**

## 🎯 Learning Objectives
- Explain stack vs heap memory and how garbage collection works conceptually
- Apply common concurrency patterns beyond raw locks
- Reason about memory leaks and how to spot them

## 📚 Topics
- Stack vs heap allocation
- Garbage collection strategies (reference counting, mark-and-sweep — conceptually)
- Higher-level concurrency: thread pools, futures/promises, producer-consumer
- Common memory leak patterns

## 💡 Core Ideas
- Stack memory is automatically reclaimed when a function returns; heap memory lives until something (you or the GC) frees it — most memory bugs live on the heap.
- Garbage collectors trade a bit of runtime overhead for eliminating manual free() bugs — but they don't prevent *logical* memory leaks (e.g., a growing cache no one ever clears).
- Producer-consumer with a bounded queue is one of the most reusable concurrency patterns — it decouples work generation from work processing.

## 🛠️ Practice Exercises
- [ ] Implement a producer-consumer setup: one thread/task generates items into a bounded queue, another consumes them, with proper synchronization.
- [ ] Write a small program that intentionally leaks memory (e.g., an ever-growing cache with no eviction) and identify why it leaks.
- [ ] Research (5–10 min) how your primary language's garbage collector works at a high level and write 3 sentences summarizing it.

## 🎤 Interview-Style Questions
Try answering these out loud, without notes, before checking any reference:

- What's a memory leak in a garbage-collected language, if the GC handles freeing memory?
- Why is a thread pool usually preferable to spawning a new thread per task?
- What's the difference between a Future/Promise and a raw thread?

## 🔗 Resources
- https://www.oreilly.com/library/view/operating-systems-three/9780985673029/
- https://docs.oracle.com/javase/tutorial/essential/concurrency/ (concepts apply broadly)

## ✅ Daily Checklist
- [ ] Read/reviewed the topics and core ideas above
- [ ] Completed all practice exercises
- [ ] Answered the interview-style questions out loud or in writing
- [ ] Committed any code to your repo
- [ ] Wrote 2-3 sentences in your own words summarizing the day's key insight

---
[⬅ Previous Day](day-18.md) | [🏠 Home](README.md) | [Next Day ➡](day-20.md)
