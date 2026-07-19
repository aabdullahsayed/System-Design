# Day 3: Arrays & Strings

**Week 1 · Data Representation & Complexity**

## 🎯 Learning Objectives
- Manipulate arrays and strings efficiently
- Recognize and apply the two-pointer and sliding-window patterns
- Understand mutability and memory layout trade-offs

## 📚 Topics
- Static vs dynamic arrays
- String immutability and StringBuilder-style buffers
- Two-pointer technique
- Sliding window technique

## 💡 Core Ideas
- A dynamic array (ArrayList/List/Vector) doubles its backing array when full — that's why `add` is amortized O(1) but occasionally O(n).
- Two pointers turn many O(n²) 'check every pair' problems into O(n).
- Sliding window turns many O(n²) 'check every substring' problems into O(n).

## 🛠️ Practice Exercises
- [ ] Implement: reverse an array in place, without extra storage.
- [ ] Implement: find if an array has a pair summing to a target (two-pointer, on a sorted array).
- [ ] Implement: longest substring without repeating characters (sliding window).
- [ ] Implement: check if a string is a palindrome, ignoring non-alphanumeric characters.

## 🎤 Interview-Style Questions
Try answering these out loud, without notes, before checking any reference:

- Why is string concatenation in a loop often a performance trap, and what's the fix?
- How would you find the intersection of two arrays efficiently?
- What's the difference between a shallow copy and a deep copy of an array of objects?

## 🔗 Resources
- https://neetcode.io/roadmap
- https://leetcode.com/tag/array/

## ✅ Daily Checklist
- [ ] Read/reviewed the topics and core ideas above
- [ ] Completed all practice exercises
- [ ] Answered the interview-style questions out loud or in writing
- [ ] Committed any code to your repo
- [ ] Wrote 2-3 sentences in your own words summarizing the day's key insight

---
[⬅ Previous Day](day-02.md) | [🏠 Home](README.md) | [Next Day ➡](day-04.md)
