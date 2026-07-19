# Day 16: Dynamic Programming II: 2D & Beyond

**Week 3 · Advanced Algorithms & Systems**

## 🎯 Learning Objectives
- Extend DP to two-dimensional state spaces
- Recognize classic 2D DP problem shapes
- Reconstruct the actual solution (not just the optimal value) from a DP table

## 📚 Topics
- 2D DP tables (grids, two-sequence comparisons)
- Classic problems: knapsack, longest common subsequence, edit distance
- Reconstructing the solution path from a filled DP table

## 💡 Core Ideas
- 2D DP usually shows up when the state depends on *two* changing quantities — position in two sequences, or 'items considered so far' + 'capacity used so far'.
- Once the table is filled, walking backward through it (tracking which choice was made at each cell) reconstructs the actual solution, not just its value.
- Knapsack, LCS, and edit distance are the three most common 2D DP archetypes — recognizing the shape of a new problem against these three goes a long way.

## 🛠️ Practice Exercises
- [ ] Solve 0/1 knapsack and print which items were chosen, not just the max value.
- [ ] Solve longest common subsequence between two strings.
- [ ] Solve edit distance (Levenshtein) between two strings.
- [ ] Solve unique paths in a grid with obstacles.

## 🎤 Interview-Style Questions
Try answering these out loud, without notes, before checking any reference:

- How does 0/1 knapsack differ from the 'unbounded' knapsack variant, both in problem statement and recurrence?
- Why is edit distance a 2D DP problem — what are the two changing dimensions?
- How would you reduce the space complexity of a 2D DP solution from O(n*m) to O(min(n,m))?

## 🔗 Resources
- https://www.geeksforgeeks.org/dynamic-programming/
- https://leetcode.com/tag/dynamic-programming/

## ✅ Daily Checklist
- [ ] Read/reviewed the topics and core ideas above
- [ ] Completed all practice exercises
- [ ] Answered the interview-style questions out loud or in writing
- [ ] Committed any code to your repo
- [ ] Wrote 2-3 sentences in your own words summarizing the day's key insight

---
[⬅ Previous Day](day-15.md) | [🏠 Home](README.md) | [Next Day ➡](day-17.md)
