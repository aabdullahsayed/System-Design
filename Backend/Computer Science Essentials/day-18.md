# Day 18: Operating Systems: Processes & Threads

**Week 3 · Advanced Algorithms & Systems**

## 🎯 Learning Objectives
- Explain the difference between a process and a thread
- Understand concurrency primitives (locks, race conditions)
- Reason about context switching and scheduling at a high level

## 📚 Topics
- Processes vs threads vs coroutines
- The scheduler & context switching
- Race conditions, mutexes/locks, deadlocks
- Concurrency vs parallelism

## 💡 Core Ideas
- A process owns its own memory space; threads within a process share memory — that shared memory is exactly why threads are both powerful and dangerous.
- A race condition happens when correctness depends on timing — the fix is almost always 'make the critical section atomic' via a lock or similar primitive.
- Deadlock classically requires 4 conditions (mutual exclusion, hold-and-wait, no preemption, circular wait) — breaking any one prevents it.

## 🛠️ Practice Exercises
- [ ] Write a small program with two threads incrementing a shared counter without a lock; run it and observe the race condition (wrong final count).
- [ ] Fix it with a mutex/lock and verify correctness.
- [ ] Write down, in your own words, a scenario that causes deadlock and how you'd redesign it to avoid one of the four conditions.

## 🎤 Interview-Style Questions
Try answering these out loud, without notes, before checking any reference:

- What's the difference between concurrency and parallelism?
- Why is a race condition non-deterministic, and why does that make it hard to debug?
- What are the four necessary conditions for deadlock, and how do you break one of them in practice?

## 🔗 Resources
- https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/
- https://www.oreilly.com/library/view/operating-systems-three/9780985673029/ (OSTEP, free online)

## ✅ Daily Checklist
- [ ] Read/reviewed the topics and core ideas above
- [ ] Completed all practice exercises
- [ ] Answered the interview-style questions out loud or in writing
- [ ] Committed any code to your repo
- [ ] Wrote 2-3 sentences in your own words summarizing the day's key insight

---
[⬅ Previous Day](day-17.md) | [🏠 Home](README.md) | [Next Day ➡](day-19.md)
