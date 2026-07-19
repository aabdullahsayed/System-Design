# Day 23: Distributed Systems Fundamentals

**Week 4 · Systems, Design & Practice**

## 🎯 Learning Objectives
- Explain the CAP theorem and what it actually constrains
- Reason about consistency models
- Understand why distributed systems are hard (partial failure)

## 📚 Topics
- CAP theorem (Consistency, Availability, Partition tolerance)
- Strong vs eventual consistency
- Partial failure & the fallacies of distributed computing
- Replication basics (leader-follower)

## 💡 Core Ideas
- CAP theorem says: during a network partition, you must choose consistency OR availability — you can't have both. Outside of a partition, you can often have both.
- 'Eventual consistency' means replicas *will* converge, but reads can temporarily see stale data — many real systems accept this for availability/performance.
- The core insight of distributed systems: any single component can fail at any time, independently, and your system must define behavior for that — not just the happy path.

## 🛠️ Practice Exercises
- [ ] Write out the CAP theorem in your own words with a concrete example of a system choosing CP vs AP.
- [ ] Read about the 'Fallacies of Distributed Computing' and pick 3 that you've personally seen bite a real system (or would if you haven't yet).
- [ ] Sketch (in markdown or a diagram) a simple leader-follower replication setup and what happens if the leader crashes.

## 🎤 Interview-Style Questions
Try answering these out loud, without notes, before checking any reference:

- Give a real example of a system that prioritizes availability over consistency, and one that does the opposite.
- What does 'eventual consistency' actually guarantee, and what does it NOT guarantee?
- Why can't a distributed system be simultaneously consistent, available, and partition-tolerant during an actual partition?

## 🔗 Resources
- https://www.the-paper-trail.org/post/2012-11-19-mongodb-and-the-cap-theorem/
- https://en.wikipedia.org/wiki/Fallacies_of_distributed_computing

## ✅ Daily Checklist
- [ ] Read/reviewed the topics and core ideas above
- [ ] Completed all practice exercises
- [ ] Answered the interview-style questions out loud or in writing
- [ ] Committed any code to your repo
- [ ] Wrote 2-3 sentences in your own words summarizing the day's key insight

---
[⬅ Previous Day](day-22.md) | [🏠 Home](README.md) | [Next Day ➡](day-24.md)
