# Day 25: Logging, Monitoring & Observability

**Week 4 · Production Readiness**

## 🎯 Learning Objectives
- Emit structured, actionable logs
- Expose health and metrics endpoints
- Understand the three pillars: logs, metrics, traces

## 📚 Topics
- Structured logging & correlation IDs
- Health checks
- Metrics (request rate, latency, error rate)
- Distributed tracing overview

## 🟣 C# / .NET Focus
- Use Serilog with structured sinks (Console + file/Seq).
- Add `AddHealthChecks()` and `/health` endpoint.
- Expose metrics via `OpenTelemetry` + Prometheus exporter.

## 🟠 Java Focus
- Use SLF4J + Logback with JSON encoder.
- Add Spring Boot Actuator (`/actuator/health`, `/actuator/metrics`).
- Expose metrics via Micrometer + Prometheus.

## 🛠️ Hands-On Task
Add structured logging with a correlation/request ID propagated through every log line for a single request. Expose a `/health` endpoint checking DB and cache connectivity, and a `/metrics` endpoint (or Actuator equivalent) reporting request count and average latency.

## 🚀 Stretch Goal
Wire up a local Grafana + Prometheus stack in Docker Compose to visualize your metrics.

## 🔗 Resources
- https://opentelemetry.io/docs/languages/net/
- https://docs.spring.io/spring-boot/reference/actuator/index.html

## ✅ Checklist
- [ ] Read/reviewed the topics above
- [ ] Completed the hands-on task in **C#**
- [ ] Completed the hands-on task in **Java**
- [ ] Committed code with a clear commit message
- [ ] (Optional) Attempted the stretch goal
- [ ] Wrote 2-3 sentences in your own words summarizing what you learned

---
[⬅ Previous Day](day-24.md) | [🏠 Home](README.md) | [Next Day ➡](day-26.md)
