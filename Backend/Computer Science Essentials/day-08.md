# Day 8: Recursion & Backtracking

**Week 2 · Algorithms**

## 🎯 Learning Objectives
- Write correct recursive functions with proper base cases
- Apply backtracking to combinatorial search problems
- Reason about recursion in terms of the call stack

## 📚 Topics
- Base case & recursive case design
- Recursion vs iteration trade-offs
- Backtracking: choose, explore, un-choose
- Memoization as an intro to avoiding repeated work

## 💡 Core Ideas
- Every recursive function needs a base case that's reached in finite steps, or it never terminates.
- Backtracking explores a decision tree and prunes branches that can't lead to a solution — it's DFS over choices, not data.
- Recursion trades code clarity for stack space; deep recursion can overflow where iteration wouldn't.

## 🛠️ Practice Exercises
- [ ] Implement: factorial and Fibonacci, recursively, then add memoization to Fibonacci and measure the speedup.
- [ ] Implement: generate all subsets of a set.
- [ ] Implement: generate all permutations of a list.
- [ ] Implement: solve N-Queens for a small N (e.g., 4 or 8) using backtracking.

## 🎤 Interview-Style Questions
Try answering these out loud, without notes, before checking any reference:

- Why does naive recursive Fibonacci run in exponential time, and how does memoization fix it?
- How would you convert a recursive solution to an iterative one using an explicit stack?
- What's the difference between backtracking and brute force?

## 🔗 Resources
- https://leetcode.com/tag/backtracking/
- https://www.geeksforgeeks.org/recursion/

## ✅ Daily Checklist
- [ ] Read/reviewed the topics and core ideas above
- [ ] Completed all practice exercises
- [ ] Answered the interview-style questions out loud or in writing
- [ ] Committed any code to your repo
- [ ] Wrote 2-3 sentences in your own words summarizing the day's key insight

---
[⬅ Previous Day](day-07.md) | [🏠 Home](README.md) | [Next Day ➡](day-09.md)
