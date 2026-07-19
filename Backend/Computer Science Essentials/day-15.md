# Day 15: Dynamic Programming I: Foundations

**Week 3 · Advanced Algorithms & Systems**

## 🎯 Learning Objectives
- Recognize when a problem has optimal substructure and overlapping subproblems
- Convert a recursive solution into a memoized one
- Convert a memoized solution into a bottom-up tabulated one

## 📚 Topics
- Optimal substructure & overlapping subproblems
- Top-down (memoization) vs bottom-up (tabulation)
- 1D DP problems
- Building a DP table by hand

## 💡 Core Ideas
- DP is recursion + a cache — if you can write the naive recursive solution, memoization is often a one-line change (cache the result by input).
- Tabulation builds the answer bottom-up in an explicit table, usually trading recursion's elegance for better constant factors and no stack risk.
- The hardest part of DP is finding the *recurrence relation* — spend your time there before touching code.

## 🛠️ Practice Exercises
- [ ] Solve climbing stairs (how many ways to climb n stairs, 1 or 2 steps at a time) three ways: naive recursion, memoized, tabulated — and time all three on n=35.
- [ ] Solve: house robber (max sum of non-adjacent elements in an array).
- [ ] Solve: coin change (minimum coins to make an amount), and identify the recurrence relation in writing before coding.

## 🎤 Interview-Style Questions
Try answering these out loud, without notes, before checking any reference:

- What's the difference between memoization and tabulation, and when would you prefer one over the other?
- How do you identify that a problem is a DP problem in the first place?
- What's the space complexity of a 1D DP solution, and can it often be optimized further? How?

## 🔗 Resources
- https://www.freecodecamp.org/news/follow-these-steps-to-solve-any-dynamic-programming-interview-problem/
- https://leetcode.com/tag/dynamic-programming/

## ✅ Daily Checklist
- [ ] Read/reviewed the topics and core ideas above
- [ ] Completed all practice exercises
- [ ] Answered the interview-style questions out loud or in writing
- [ ] Committed any code to your repo
- [ ] Wrote 2-3 sentences in your own words summarizing the day's key insight

---
[⬅ Previous Day](day-14.md) | [🏠 Home](README.md) | [Next Day ➡](day-16.md)
