# Day 17: Greedy Algorithms & Graph Shortest Paths

**Week 3 · Advanced Algorithms & Systems**

## 🎯 Learning Objectives
- Recognize when a greedy choice leads to a globally optimal solution
- Implement Dijkstra's algorithm for weighted shortest paths
- Compare greedy vs DP approaches to the same problem class

## 📚 Topics
- The greedy-choice property & when greedy fails
- Interval scheduling as a classic greedy problem
- Dijkstra's algorithm (weighted shortest path, non-negative weights)
- Minimum spanning tree intuition (Kruskal's/Prim's, conceptually)

## 💡 Core Ideas
- Greedy works only when local optimal choices provably lead to a global optimum — always sanity-check with a counter-example before trusting a greedy idea.
- Dijkstra's is BFS with a priority queue instead of a plain queue — the 'always expand the currently-closest node' rule is what handles weights correctly.
- Many problems that look greedy are actually DP in disguise (e.g., certain scheduling variants) — this is a common interview trap.

## 🛠️ Practice Exercises
- [ ] Solve: activity/interval scheduling — select the maximum number of non-overlapping intervals (classic greedy).
- [ ] Implement Dijkstra's algorithm on a weighted graph using a priority queue, and find shortest paths from a single source.
- [ ] Find a problem where the 'obvious' greedy approach fails, construct a counter-example, and explain why.

## 🎤 Interview-Style Questions
Try answering these out loud, without notes, before checking any reference:

- Why doesn't Dijkstra's algorithm work correctly with negative edge weights?
- What's the difference between Dijkstra's and BFS, given BFS also finds 'shortest paths'?
- Give an example of a problem where greedy fails but DP succeeds.

## 🔗 Resources
- https://www.coursera.org/lecture/algorithms-greedy (search)
- https://leetcode.com/tag/greedy/

## ✅ Daily Checklist
- [ ] Read/reviewed the topics and core ideas above
- [ ] Completed all practice exercises
- [ ] Answered the interview-style questions out loud or in writing
- [ ] Committed any code to your repo
- [ ] Wrote 2-3 sentences in your own words summarizing the day's key insight

---
[⬅ Previous Day](day-16.md) | [🏠 Home](README.md) | [Next Day ➡](day-18.md)
