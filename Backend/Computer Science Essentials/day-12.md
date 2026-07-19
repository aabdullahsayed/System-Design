# Day 12: Balanced Trees & Heaps

**Week 2 · Algorithms**

## 🎯 Learning Objectives
- Explain how self-balancing trees maintain O(log n) guarantees
- Implement and use a binary heap
- Solve top-k / priority problems with heaps

## 📚 Topics
- Self-balancing trees (AVL/Red-Black) at a conceptual level
- Binary heaps: array representation, heapify, insert/extract
- Priority queues built on heaps
- Heap-based algorithms (top-k, k-way merge)

## 💡 Core Ideas
- You rarely hand-roll an AVL/Red-Black tree in practice, but you must understand *why* they exist: to guarantee O(log n) even on adversarial input.
- A binary heap stored as an array uses index math (`2i+1`, `2i+2`, `(i-1)/2`) instead of pointers — that's why it's so cache-friendly.
- 'Top-k' problems almost always reduce to 'maintain a heap of size k'.

## 🛠️ Practice Exercises
- [ ] Implement a min-heap from scratch (array-backed), with insert and extract-min.
- [ ] Implement: find the k largest elements in an array using a heap.
- [ ] Implement: merge k sorted lists using a min-heap.
- [ ] Use your language's built-in priority queue to solve one of the above and compare code length/clarity to your hand-rolled version.

## 🎤 Interview-Style Questions
Try answering these out loud, without notes, before checking any reference:

- Why is heap insertion O(log n) but 'building a heap from n elements' O(n), not O(n log n)?
- When would you choose a heap over a sorted array for a priority queue?
- What real-world systems use priority queues (name two)?

## 🔗 Resources
- https://www.geeksforgeeks.org/heap-data-structure/
- https://leetcode.com/tag/heap-priority-queue/

## ✅ Daily Checklist
- [ ] Read/reviewed the topics and core ideas above
- [ ] Completed all practice exercises
- [ ] Answered the interview-style questions out loud or in writing
- [ ] Committed any code to your repo
- [ ] Wrote 2-3 sentences in your own words summarizing the day's key insight

---
[⬅ Previous Day](day-11.md) | [🏠 Home](README.md) | [Next Day ➡](day-13.md)
