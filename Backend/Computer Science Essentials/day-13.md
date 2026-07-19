# Day 13: Graphs: Representation & Traversal

**Week 2 · Algorithms**

## 🎯 Learning Objectives
- Represent graphs using adjacency lists and matrices
- Implement BFS and DFS from scratch
- Apply graph traversal to real problems (connectivity, shortest path in unweighted graphs)

## 📚 Topics
- Adjacency list vs adjacency matrix trade-offs
- Directed vs undirected, weighted vs unweighted
- BFS (shortest path in unweighted graphs)
- DFS (connectivity, cycle detection, topological concepts)

## 💡 Core Ideas
- Adjacency lists are the default choice for sparse graphs (most real graphs); adjacency matrices shine for dense graphs or O(1) edge-existence checks.
- BFS explores level-by-level using a queue, which is exactly why it finds shortest paths in unweighted graphs.
- DFS explores depth-first using a stack (or recursion) — natural for cycle detection and exploring 'all paths'.

## 🛠️ Practice Exercises
- [ ] Represent a graph both as an adjacency list and adjacency matrix; implement conversion between them.
- [ ] Implement BFS and use it to find the shortest path (in edges) between two nodes.
- [ ] Implement DFS and use it to detect a cycle in a directed graph.
- [ ] Implement: count the number of connected components in an undirected graph.

## 🎤 Interview-Style Questions
Try answering these out loud, without notes, before checking any reference:

- When would an adjacency matrix be a better choice than an adjacency list?
- Why does BFS, not DFS, guarantee the shortest path in an unweighted graph?
- How do you detect a cycle in a directed graph vs an undirected graph — what's different?

## 🔗 Resources
- https://visualgo.net/en/dfsbfs
- https://leetcode.com/tag/graph/

## ✅ Daily Checklist
- [ ] Read/reviewed the topics and core ideas above
- [ ] Completed all practice exercises
- [ ] Answered the interview-style questions out loud or in writing
- [ ] Committed any code to your repo
- [ ] Wrote 2-3 sentences in your own words summarizing the day's key insight

---
[⬅ Previous Day](day-12.md) | [🏠 Home](README.md) | [Next Day ➡](day-14.md)
