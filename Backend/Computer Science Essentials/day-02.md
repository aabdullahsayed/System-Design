# Day 2: Big-O Notation & Complexity Analysis

**Week 1 · Data Representation & Complexity**

## 🎯 Learning Objectives
- Analyze the time and space complexity of any algorithm
- Compare algorithms objectively using asymptotic notation
- Recognize common complexity classes on sight

## 📚 Topics
- Big-O, Big-Theta, Big-Omega
- Time complexity vs space complexity
- Common complexity classes: O(1), O(log n), O(n), O(n log n), O(n²), O(2ⁿ)
- Amortized analysis (why `ArrayList.add` is O(1) amortized)

## 💡 Core Ideas
- Big-O describes *growth rate*, not exact speed — an O(n) algorithm can be slower than O(n²) for small n.
- Drop constants and lower-order terms: O(2n + 100) is just O(n).
- Nested loops over the same input usually multiply complexities; sequential loops add (and the add simplifies to the max).

## 🛠️ Practice Exercises
- [ ] Take 5 code snippets (loops, nested loops, recursive calls) and determine Big-O for each without running them.
- [ ] Time an O(n²) vs O(n log n) sort on inputs of size 1,000 / 10,000 / 100,000 and graph the results.
- [ ] Explain why hash table lookup is 'O(1) average, O(n) worst case'.

## 🎤 Interview-Style Questions
Try answering these out loud, without notes, before checking any reference:

- What's the time complexity of building a hash set from a list of n items?
- Why does merge sort guarantee O(n log n) but quicksort doesn't?
- When would you choose an O(n²) algorithm over an O(n log n) one in practice?

## 🔗 Resources
- https://www.bigocheatsheet.com/
- https://en.wikipedia.org/wiki/Big_O_notation

## ✅ Daily Checklist
- [ ] Read/reviewed the topics and core ideas above
- [ ] Completed all practice exercises
- [ ] Answered the interview-style questions out loud or in writing
- [ ] Committed any code to your repo
- [ ] Wrote 2-3 sentences in your own words summarizing the day's key insight

---
[⬅ Previous Day](day-01.md) | [🏠 Home](README.md) | [Next Day ➡](day-03.md)
