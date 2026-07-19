# Day 29: CI/CD Pipelines

**Week 4 · Production Readiness**

## 🎯 Learning Objectives
- Automate build, test, and deployment steps
- Run tests and quality checks on every push
- Publish container images automatically

## 📚 Topics
- CI/CD concepts
- GitHub Actions workflow syntax
- Build, test, lint, and image-publish stages
- Basic deployment strategies

## 🟣 C# / .NET Focus
- Write a GitHub Actions workflow: restore, build, `dotnet test`, then build & push Docker image.

## 🟠 Java Focus
- Write a GitHub Actions workflow: `mvn/gradle build`, run tests, then build & push Docker image.

## 🛠️ Hands-On Task
Add a GitHub Actions CI pipeline for your Bookstore repo(s) that: restores dependencies, builds, runs unit + integration tests (with Testcontainers or service containers), and on success builds & pushes a Docker image to a registry (e.g., GHCR). Add a status badge to your README.

## 🚀 Stretch Goal
Add a separate workflow that runs on PRs only (build + test, no publish) vs main branch (build + test + publish).

## 🔗 Resources
- https://docs.github.com/actions
- https://docs.github.com/actions/publishing-packages/publishing-docker-images

## ✅ Checklist
- [ ] Read/reviewed the topics above
- [ ] Completed the hands-on task in **C#**
- [ ] Completed the hands-on task in **Java**
- [ ] Committed code with a clear commit message
- [ ] (Optional) Attempted the stretch goal
- [ ] Wrote 2-3 sentences in your own words summarizing what you learned

---
[⬅ Previous Day](day-28.md) | [🏠 Home](README.md) | [Next Day ➡](day-30.md)
