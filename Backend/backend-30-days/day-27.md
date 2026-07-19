# Day 27: Microservices Architecture Basics

**Week 4 · Production Readiness**

## 🎯 Learning Objectives
- Decompose a monolith into logically separate services
- Understand inter-service communication trade-offs
- Recognize distributed-systems pitfalls

## 📚 Topics
- Service boundaries & bounded contexts
- Synchronous (HTTP/gRPC) vs asynchronous (messaging) communication
- API Gateway pattern
- Common pitfalls: distributed transactions, chatty services

## 🟣 C# / .NET Focus
- Split into `Bookstore.Catalog.Api` and `Bookstore.Orders.Api` as separate ASP.NET Core projects.
- Use `HttpClientFactory` for service-to-service calls.

## 🟠 Java Focus
- Split into `catalog-service` and `orders-service` as separate Spring Boot apps.
- Use `RestTemplate`/`WebClient` for service-to-service calls.

## 🛠️ Hands-On Task
Split your Bookstore monolith into two services: a Catalog service (books/authors) and an Orders service (purchases), communicating over HTTP. The Orders service calls Catalog to check stock/price before completing an order. Document the new architecture with a simple diagram (markdown/mermaid).

## 🚀 Stretch Goal
Replace the synchronous stock-check call with the async event-driven approach from Day 26 and compare trade-offs in your README.

## 🔗 Resources
- https://microservices.io/patterns/index.html
- https://learn.microsoft.com/dotnet/architecture/microservices/

## ✅ Checklist
- [ ] Read/reviewed the topics above
- [ ] Completed the hands-on task in **C#**
- [ ] Completed the hands-on task in **Java**
- [ ] Committed code with a clear commit message
- [ ] (Optional) Attempted the stretch goal
- [ ] Wrote 2-3 sentences in your own words summarizing what you learned

---
[⬅ Previous Day](day-26.md) | [🏠 Home](README.md) | [Next Day ➡](day-28.md)
