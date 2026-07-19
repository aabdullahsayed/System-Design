# Day 19: NoSQL Basics: MongoDB & Redis

**Week 3 · Databases & Persistence**

## 🎯 Learning Objectives
- Recognize when NoSQL fits better than relational storage
- Perform basic document store operations
- Use a key-value store for simple caching/session use cases

## 📚 Topics
- Document databases vs relational (MongoDB)
- Key-value stores (Redis)
- When to use polyglot persistence

## 🟣 C# / .NET Focus
- Use `MongoDB.Driver` for C#.
- Use `StackExchange.Redis` for Redis access.

## 🟠 Java Focus
- Use Spring Data MongoDB.
- Use Spring Data Redis / Jedis/Lettuce.

## 🛠️ Hands-On Task
Add a `BookReview` feature stored in MongoDB (flexible schema: rating, text, optional fields) alongside your relational Book data. Separately, use Redis to cache the 'top 10 books by rating' result for 60 seconds, with a cache-miss fallback to computing it fresh.

## 🚀 Stretch Goal
Add TTL-based Redis caching for individual book lookups by id, with cache invalidation on update/delete.

## 🔗 Resources
- https://www.mongodb.com/docs/drivers/csharp/current/
- https://spring.io/projects/spring-data-redis

## ✅ Checklist
- [ ] Read/reviewed the topics above
- [ ] Completed the hands-on task in **C#**
- [ ] Completed the hands-on task in **Java**
- [ ] Committed code with a clear commit message
- [ ] (Optional) Attempted the stretch goal
- [ ] Wrote 2-3 sentences in your own words summarizing what you learned

---
[⬅ Previous Day](day-18.md) | [🏠 Home](README.md) | [Next Day ➡](day-20.md)
