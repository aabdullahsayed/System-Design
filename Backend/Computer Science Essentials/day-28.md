# Day 28: Testing & Debugging Fundamentals

**Week 4 · Systems, Design & Practice**

## 🎯 Learning Objectives
- Write effective unit tests that actually catch bugs
- Apply systematic debugging instead of guess-and-check
- Understand the testing pyramid

## 📚 Topics
- The testing pyramid (unit, integration, e2e)
- What makes a good test (fast, isolated, repeatable, meaningful assertions)
- Systematic debugging: reproduce, isolate, hypothesize, verify
- Using a debugger effectively (breakpoints, watches, stepping)

## 💡 Core Ideas
- The testing pyramid argues for MANY fast unit tests, FEWER integration tests, and VERY FEW slow end-to-end tests — inverting this (an 'ice cream cone') usually means slow, flaky test suites.
- A good unit test tests ONE behavior and fails for exactly one reason — tests that check too much become brittle and hard to debug when they fail.
- Systematic debugging beats random guessing: reproduce reliably, narrow down (binary-search the codebase if needed), form a specific hypothesis, then verify it.

## 🛠️ Practice Exercises
- [ ] Take a small piece of code with a subtle bug (write one deliberately, e.g., an off-by-one in a loop), write a failing test that catches it, then fix the bug.
- [ ] Practice using your IDE/language's debugger: set a breakpoint, step through, and inspect variables for a program you've already written this month.
- [ ] Write 5 unit tests for one function from your Week 1 data structures library that you haven't tested yet.

## 🎤 Interview-Style Questions
Try answering these out loud, without notes, before checking any reference:

- What makes a test 'flaky', and how do you fix one?
- Why is 100% code coverage not the same thing as 'well-tested'?
- Walk through your systematic process for debugging a bug you can't reproduce reliably.

## 🔗 Resources
- https://martinfowler.com/articles/practical-test-pyramid.html
- https://www.debuggingbook.org/

## ✅ Daily Checklist
- [ ] Read/reviewed the topics and core ideas above
- [ ] Completed all practice exercises
- [ ] Answered the interview-style questions out loud or in writing
- [ ] Committed any code to your repo
- [ ] Wrote 2-3 sentences in your own words summarizing the day's key insight

---
[⬅ Previous Day](day-27.md) | [🏠 Home](README.md) | [Next Day ➡](day-29.md)
