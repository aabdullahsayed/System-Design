# Day 28: Docker & Containerization

**Week 4 · Production Readiness**

## 🎯 Learning Objectives
- Containerize a backend application
- Write efficient, secure Dockerfiles
- Orchestrate multi-service apps with Docker Compose

## 📚 Topics
- Dockerfile fundamentals & multi-stage builds
- Image size/security best practices
- Docker Compose for local multi-service dev

## 🟣 C# / .NET Focus
- Write a multi-stage Dockerfile using `mcr.microsoft.com/dotnet/sdk` build stage + `aspnet` runtime stage.

## 🟠 Java Focus
- Write a multi-stage Dockerfile using `eclipse-temurin` build (Maven/Gradle) + a slim JRE runtime stage.

## 🛠️ Hands-On Task
Containerize your Catalog and Orders services (from Day 27) with multi-stage Dockerfiles, plus Postgres, Redis, and RabbitMQ/Kafka, all wired together in a single `docker-compose.yml`. Verify `docker compose up` brings up the entire system and the services can talk to each other and their databases.

## 🚀 Stretch Goal
Add a `.dockerignore`, non-root user in the final image, and health checks in Compose for each service.

## 🔗 Resources
- https://docs.docker.com/language/dotnet/
- https://docs.docker.com/language/java/

## ✅ Checklist
- [ ] Read/reviewed the topics above
- [ ] Completed the hands-on task in **C#**
- [ ] Completed the hands-on task in **Java**
- [ ] Committed code with a clear commit message
- [ ] (Optional) Attempted the stretch goal
- [ ] Wrote 2-3 sentences in your own words summarizing what you learned

---
[⬅ Previous Day](day-27.md) | [🏠 Home](README.md) | [Next Day ➡](day-29.md)
