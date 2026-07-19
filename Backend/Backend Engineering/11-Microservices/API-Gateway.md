# API Gateway

## What is it?
A single entry point that sits in front of a microservices architecture, routing incoming requests to the appropriate backend service, and often handling cross-cutting concerns (auth, rate limiting, logging) in one centralized place instead of duplicating that logic in every service.

## Why care?
Without a gateway, clients (web/mobile apps) would need to know the address of every individual microservice, and every service would need to independently implement authentication, rate limiting, and CORS handling — an API Gateway centralizes this, dramatically simplifying both client integration and cross-cutting operational concerns.

## Core responsibilities
- **Routing**: `/users/*` → Users Service, `/orders/*` → Orders Service.
- **Authentication**: validate a JWT/API key once at the gateway, instead of every downstream service re-implementing auth checks.
- **Rate limiting**: enforce request limits per client/API key centrally.
- **Request/response transformation**: adapt a legacy service's response shape to a cleaner public API contract.
- **Aggregation**: combine responses from multiple services into a single response for the client (reducing the number of round-trips a mobile client needs to make) — sometimes called the "Backend for Frontend" (BFF) pattern.
- **Load balancing**: distribute requests across multiple instances of a backend service.
- **Observability**: centralized logging/metrics for all incoming traffic, before it's split across services.

## Common implementations
- **Managed cloud services**: AWS API Gateway, Google Cloud Endpoints, Azure API Management.
- **Self-hosted/open-source**: Kong, Nginx (as a reverse proxy + gateway), Envoy, Traefik.
- **Framework-level**: many microservices frameworks (Spring Cloud Gateway) provide this as a library/service you deploy yourself.

## The gateway as a single point of failure (a real risk to design around)
Since ALL traffic flows through the gateway, it must be highly available itself (typically deployed with multiple redundant instances behind a load balancer) — if the gateway goes down, the entire system becomes unreachable even if every individual microservice is healthy.

## Practical example
A mobile app's "product detail" screen needs data from the Products service, Reviews service, and Inventory service. Instead of the mobile app making 3 separate network calls (slow on mobile networks, more battery/data usage), a BFF-style API Gateway endpoint aggregates all 3 backend calls server-side and returns one combined response — a common, genuinely valuable pattern in real microservices architectures.
