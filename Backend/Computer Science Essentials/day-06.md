# Day 6: Hash Tables

**Week 1 · Data Representation & Complexity**

## 🎯 Learning Objectives
- Explain how hash tables achieve near O(1) operations
- Handle collisions correctly
- Choose good hash functions and recognize poor ones

## 📚 Topics
- Hashing functions and hash distribution
- Collision resolution: chaining vs open addressing
- Load factor and resizing
- Hash sets vs hash maps

## 💡 Core Ideas
- A hash table is 'array + hash function' — the hash function's job is to spread keys evenly across buckets.
- Collisions are inevitable (pigeonhole principle); chaining and open addressing are the two main coping strategies.
- Load factor (items/buckets) drives *when* to resize — too high and chains get long, tanking performance.

## 🛠️ Practice Exercises
- [ ] Implement a basic hash map from scratch using an array of linked lists (separate chaining), including resizing.
- [ ] Implement: two-sum using a hash map in one pass, O(n).
- [ ] Implement: group anagrams together using a hash map.
- [ ] Implement: find the first non-repeating character in a string.

## 🎤 Interview-Style Questions
Try answering these out loud, without notes, before checking any reference:

- Why must objects used as hash-map keys implement both `equals`/`==` and a consistent hash function?
- What happens to hash-map performance if every key hashes to the same bucket?
- How does a hash set differ from a hash map internally?

## 🔗 Resources
- https://www.interviewcake.com/concept/java/hash-map
- https://leetcode.com/tag/hash-table/

## ✅ Daily Checklist
- [ ] Read/reviewed the topics and core ideas above
- [ ] Completed all practice exercises
- [ ] Answered the interview-style questions out loud or in writing
- [ ] Committed any code to your repo
- [ ] Wrote 2-3 sentences in your own words summarizing the day's key insight

---
[⬅ Previous Day](day-05.md) | [🏠 Home](README.md) | [Next Day ➡](day-07.md)
