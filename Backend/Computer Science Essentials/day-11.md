# Day 11: Trees & Binary Search Trees

**Week 2 · Algorithms**

## 🎯 Learning Objectives
- Traverse trees using all standard orders
- Implement and use a binary search tree
- Reason about tree height and its effect on complexity

## 📚 Topics
- Tree terminology (root, leaf, height, depth)
- Traversals: pre-order, in-order, post-order, level-order (BFS)
- Binary search trees: insert, search, delete
- Why unbalanced BSTs degrade to O(n)

## 💡 Core Ideas
- In-order traversal of a BST visits nodes in sorted order — this single fact explains most BST-based problems.
- A BST's performance is entirely dependent on its height; a degenerate (linked-list-shaped) BST loses all its advantages.
- Recursive tree code is usually the cleanest — most tree algorithms are 'do something, then recurse left and right'.

## 🛠️ Practice Exercises
- [ ] Implement a binary search tree with insert, search, and in-order traversal.
- [ ] Implement all four traversal orders (pre/in/post/level) on a tree, both recursively and iteratively for at least one.
- [ ] Implement: validate whether a binary tree is a valid BST.
- [ ] Implement: find the lowest common ancestor of two nodes in a BST.

## 🎤 Interview-Style Questions
Try answering these out loud, without notes, before checking any reference:

- Why does an unbalanced BST built from sorted input degrade to a linked list?
- How would you serialize and deserialize a binary tree?
- What's the difference between BFS and DFS traversal of a tree, and when would you use each?

## 🔗 Resources
- https://visualgo.net/en/bst
- https://leetcode.com/tag/tree/

## ✅ Daily Checklist
- [ ] Read/reviewed the topics and core ideas above
- [ ] Completed all practice exercises
- [ ] Answered the interview-style questions out loud or in writing
- [ ] Committed any code to your repo
- [ ] Wrote 2-3 sentences in your own words summarizing the day's key insight

---
[⬅ Previous Day](day-10.md) | [🏠 Home](README.md) | [Next Day ➡](day-12.md)
