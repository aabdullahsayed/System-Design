# Day 4: Linked Lists

**Week 1 · Data Representation & Complexity**

## 🎯 Learning Objectives
- Implement singly and doubly linked lists from scratch
- Apply fast/slow pointer techniques
- Compare linked lists to arrays for different access patterns

## 📚 Topics
- Singly vs doubly linked lists
- Insertion/deletion complexity vs arrays
- Fast & slow pointers (Floyd's cycle detection)
- Sentinel/dummy nodes to simplify edge cases

## 💡 Core Ideas
- Arrays give O(1) random access but O(n) insertion in the middle; linked lists flip that trade-off.
- A dummy head node eliminates most 'is this the first node?' special-casing in list code.
- Fast/slow pointers solve cycle detection and 'find the middle' in one pass without extra memory.

## 🛠️ Practice Exercises
- [ ] Implement a singly linked list with insert/delete/search from scratch (no built-in list types).
- [ ] Implement: reverse a linked list, iteratively.
- [ ] Implement: detect a cycle in a linked list (Floyd's algorithm).
- [ ] Implement: find the middle node in one pass.

## 🎤 Interview-Style Questions
Try answering these out loud, without notes, before checking any reference:

- When would you prefer a linked list over an array/ArrayList in real production code?
- How do you detect and find the *start* of a cycle in a linked list?
- What's the complexity of accessing the k-th element in a linked list vs an array?

## 🔗 Resources
- https://visualgo.net/en/list
- https://leetcode.com/tag/linked-list/

## ✅ Daily Checklist
- [ ] Read/reviewed the topics and core ideas above
- [ ] Completed all practice exercises
- [ ] Answered the interview-style questions out loud or in writing
- [ ] Committed any code to your repo
- [ ] Wrote 2-3 sentences in your own words summarizing the day's key insight

---
[⬅ Previous Day](day-03.md) | [🏠 Home](README.md) | [Next Day ➡](day-05.md)
