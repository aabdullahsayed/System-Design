# Day 24: System Design Fundamentals

**Week 4 · Systems, Design & Practice**

## 🎯 Learning Objectives
- Estimate scale (back-of-envelope calculations)
- Apply core scaling techniques: caching, load balancing, sharding
- Structure a system design answer clearly

## 📚 Topics
- Back-of-envelope estimation (QPS, storage, bandwidth)
- Vertical vs horizontal scaling
- Load balancing strategies
- Caching layers & database sharding, conceptually

## 💡 Core Ideas
- A system design answer should always start with requirements and scale estimates BEFORE architecture — numbers drive design decisions, not the other way around.
- Horizontal scaling (more machines) is usually preferred at large scale over vertical scaling (bigger machines) because it has no hard ceiling and improves fault tolerance.
- Caching, load balancing, and sharding are the three most reusable scaling tools — most system design problems combine some subset of these three.

## 🛠️ Practice Exercises
- [ ] Do a back-of-envelope estimate for a URL shortener: given 100M new URLs/month and a 100:1 read:write ratio, estimate storage needs and read/write QPS.
- [ ] Sketch (markdown/diagram) a high-level architecture for that URL shortener: API layer, database, cache, and how they connect.
- [ ] Explain 3 different sharding strategies (range-based, hash-based, geographic) and a downside of each.

## 🎤 Interview-Style Questions
Try answering these out loud, without notes, before checking any reference:

- Why is horizontal scaling generally preferred over vertical scaling at large scale?
- How would you decide what to cache and for how long in a read-heavy system?
- What problem does consistent hashing solve in the context of sharding/load balancing?

## 🔗 Resources
- https://github.com/donnemartin/system-design-primer
- https://www.educative.io/courses/grokking-the-system-design-interview

## ✅ Daily Checklist
- [ ] Read/reviewed the topics and core ideas above
- [ ] Completed all practice exercises
- [ ] Answered the interview-style questions out loud or in writing
- [ ] Committed any code to your repo
- [ ] Wrote 2-3 sentences in your own words summarizing the day's key insight

---
[⬅ Previous Day](day-23.md) | [🏠 Home](README.md) | [Next Day ➡](day-25.md)
