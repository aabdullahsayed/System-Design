# Day 10: Searching & Binary Search

**Week 2 · Algorithms**

## 🎯 Learning Objectives
- Implement binary search correctly (including tricky variants)
- Recognize when a problem is 'binary searchable'
- Avoid classic off-by-one bugs

## 📚 Topics
- Linear vs binary search
- Binary search on sorted arrays
- Binary search on an answer space (not just an array)
- Common off-by-one pitfalls

## 💡 Core Ideas
- Binary search needs a sorted (or monotonic) search space — the core insight is 'can I eliminate half the remaining possibilities each step?'
- Binary search isn't just for arrays: you can binary-search over any monotonic function's *answer*, e.g., 'smallest x such that condition(x) is true'.
- Off-by-one errors usually come from an unclear invariant about what `low`/`high`/`mid` mean — write the invariant down before coding.

## 🛠️ Practice Exercises
- [ ] Implement binary search on a sorted array (both iterative and recursive).
- [ ] Implement: find the first and last position of a target in a sorted array with duplicates.
- [ ] Implement: find the square root of a number using binary search on the answer space, to a given precision.
- [ ] Implement: search in a rotated sorted array.

## 🎤 Interview-Style Questions
Try answering these out loud, without notes, before checking any reference:

- Why does binary search require the input to be sorted (or the search space to be monotonic)?
- How do you adapt binary search to find the *leftmost* occurrence of a duplicate value?
- What's the time complexity of binary search, and why?

## 🔗 Resources
- https://leetcode.com/tag/binary-search/
- https://www.topcoder.com/thrive/articles/Binary%20Search

## ✅ Daily Checklist
- [ ] Read/reviewed the topics and core ideas above
- [ ] Completed all practice exercises
- [ ] Answered the interview-style questions out loud or in writing
- [ ] Committed any code to your repo
- [ ] Wrote 2-3 sentences in your own words summarizing the day's key insight

---
[⬅ Previous Day](day-09.md) | [🏠 Home](README.md) | [Next Day ➡](day-11.md)
