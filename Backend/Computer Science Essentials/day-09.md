# Day 9: Sorting Algorithms

**Week 2 · Algorithms**

## 🎯 Learning Objectives
- Implement the major sorting algorithms from scratch
- Compare their time/space complexity and stability
- Choose the right sort for a given constraint

## 📚 Topics
- Bubble/insertion/selection sort (O(n²) family)
- Merge sort & quicksort (O(n log n) family)
- Stability in sorting
- When built-in sorts are (and aren't) enough

## 💡 Core Ideas
- Merge sort guarantees O(n log n) and is stable, at the cost of O(n) extra space; quicksort is usually faster in practice but O(n²) worst case and unstable.
- 'Stable' means equal elements keep their relative order — this matters when sorting by multiple keys.
- Most real systems use a hybrid (e.g., Timsort, Introsort) — know the trade-offs even if you rarely hand-roll a sort.

## 🛠️ Practice Exercises
- [ ] Implement merge sort from scratch.
- [ ] Implement quicksort from scratch (pick a pivot strategy and justify it).
- [ ] Implement insertion sort and identify when it beats O(n log n) sorts in practice (small/nearly-sorted input).
- [ ] Sort a list of objects by two keys (e.g., last name, then first name) and verify stability matters.

## 🎤 Interview-Style Questions
Try answering these out loud, without notes, before checking any reference:

- Why is quicksort's worst case O(n²), and how do real implementations avoid triggering it?
- When would you choose insertion sort over quicksort in production code?
- How would you sort a huge file that doesn't fit in memory (external sorting, conceptually)?

## 🔗 Resources
- https://visualgo.net/en/sorting
- https://www.toptal.com/developers/sorting-algorithms

## ✅ Daily Checklist
- [ ] Read/reviewed the topics and core ideas above
- [ ] Completed all practice exercises
- [ ] Answered the interview-style questions out loud or in writing
- [ ] Committed any code to your repo
- [ ] Wrote 2-3 sentences in your own words summarizing the day's key insight

---
[⬅ Previous Day](day-08.md) | [🏠 Home](README.md) | [Next Day ➡](day-10.md)
