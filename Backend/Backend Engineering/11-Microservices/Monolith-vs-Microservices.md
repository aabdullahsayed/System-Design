# Monolith vs Microservices

## What is it?
A **monolith** is a single, unified application containing all functionality (user management, orders, payments, etc.) deployed as one unit. **Microservices** split that same functionality into multiple small, independently deployable services, each owning its own data and communicating over the network (usually HTTP/gRPC or messaging).

## Why care?
This is one of the most consequential architecture decisions a backend team makes — and one of the most over-hyped/misapplied. Understanding the real trade-offs (not just "microservices are modern, monoliths are old") is a mark of engineering maturity.

## Monolith
```
┌─────────────────────────────┐
│         Single App           │
│  [Users] [Orders] [Payments]  │
│         [Shared DB]            │
└─────────────────────────────┘
```
**Pros**: simple to develop, test, deploy, and debug (one codebase, one deployment, one set of logs); easy to maintain data consistency (one database, real transactions); lower operational overhead (no service discovery, no distributed tracing needed).
**Cons**: the entire app must be redeployed for any change; scaling means scaling the WHOLE app even if only one part is under load; a bug in one module can crash the entire application; large teams working in one codebase can create merge conflicts and coordination overhead as the company grows.

## Microservices
```
┌────────┐   ┌────────┐   ┌──────────┐
│ Users  │   │ Orders │   │ Payments │
│  Svc   │   │  Svc   │   │   Svc    │
│ [DB]   │   │ [DB]   │   │  [DB]    │
└────────┘   └────────┘   └──────────┘
     ↑            ↑              ↑
     └──── API Gateway / Network ────┘
```
**Pros**: each service can be deployed, scaled, and even rewritten independently; teams can own individual services end-to-end (clear ownership boundaries at scale); a failure in one service doesn't necessarily crash others (with proper isolation).
**Cons**: massive operational complexity — network calls replace function calls (latency, partial failures now a constant concern), distributed data consistency is genuinely hard (no more simple database transactions across services), requires investment in service discovery, distributed tracing, centralized logging, and API contracts between teams.

## The honest, senior-engineer take
**Start with a well-structured monolith.** Most companies (and almost all early-stage products) are better served by a monolith with clean internal module boundaries than by premature microservices — the operational complexity of microservices is a real, ongoing tax that only pays off once you have genuine scaling or team-boundary problems a monolith can't solve. Many famous "microservices success stories" (Amazon, Netflix, Uber) only moved to microservices AFTER their monolith hit real organizational/scaling limits, not from day one.

## In practice
- **"Modular monolith"**: a middle ground — one deployable application, but with strict internal module boundaries (separate packages/namespaces, no direct cross-module database access) — gets much of the maintainability benefit of microservices without the operational cost, and can be split into real microservices later if/when needed.
- **The real trigger for microservices**: usually organizational (multiple independent teams needing to deploy without blocking each other) or genuinely differentiated scaling needs (one component needs 100x the resources of the rest) — not "microservices are best practice" as a blanket rule.
