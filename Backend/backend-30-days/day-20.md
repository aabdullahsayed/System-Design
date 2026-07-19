# Day 20: Caching Strategies

**Week 3 · Databases & Persistence**

## 🎯 Learning Objectives
- Apply caching patterns appropriately
- Understand cache invalidation trade-offs
- Use in-memory and distributed caching

## 📚 Topics
- Cache-aside, write-through, write-behind
- In-memory vs distributed caching
- Cache invalidation strategies
- Cache stampede prevention

## 🟣 C# / .NET Focus
- Use `IMemoryCache` for in-process caching.
- Use `IDistributedCache` (Redis-backed) for multi-instance scenarios.

## 🟠 Java Focus
- Use Spring's `@Cacheable`/`@CacheEvict` with Caffeine (in-process) or Redis (distributed).
- Configure cache TTLs per cache region.

## 🛠️ Hands-On Task
Add caching to your Bookstore's most-read endpoint (e.g., book list/search) using the cache-aside pattern, with explicit invalidation on writes (create/update/delete). Measure and record response time with vs without cache warm.

## 🚀 Stretch Goal
Add basic cache-stampede protection (e.g., a lock or 'only one recompute at a time' guard) for the cached endpoint.

## 🔗 Resources
- https://learn.microsoft.com/aspnet/core/performance/caching/overview
- https://docs.spring.io/spring-framework/reference/integration/cache.html

## ✅ Checklist
- [ ] Read/reviewed the topics above
- [ ] Completed the hands-on task in **C#**
- [ ] Completed the hands-on task in **Java**
- [ ] Committed code with a clear commit message
- [ ] (Optional) Attempted the stretch goal
- [ ] Wrote 2-3 sentences in your own words summarizing what you learned

---
[⬅ Previous Day](day-19.md) | [🏠 Home](README.md) | [Next Day ➡](day-21.md)
