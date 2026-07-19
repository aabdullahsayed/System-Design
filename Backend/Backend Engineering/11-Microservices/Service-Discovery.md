# Service Discovery

## What is it?
The mechanism by which services find the network location (IP address + port) of other services they need to call — necessary because, in modern deployments (containers, auto-scaling), service instances are created/destroyed dynamically and don't have fixed, predictable addresses.

## Why care?
In a monolith, calling another module is just a function call — no addressing problem exists. In microservices, "where do I even send this request?" becomes a genuine, non-trivial infrastructure problem that must be solved before inter-service communication can work reliably.

## Client-side discovery
The calling service queries a **service registry** directly to get a list of healthy instances, then picks one (often via a load-balancing strategy like round-robin) and calls it directly.
```
Service A → [Query Registry: "where is Service B?"] → Registry returns [10.0.1.5:8080, 10.0.1.9:8080]
Service A → picks one → calls it directly
```
Requires client-side logic in every service to query the registry and load-balance — more complex per-service, but avoids an extra network hop.

## Server-side discovery
The calling service sends its request to a fixed, well-known load balancer/proxy, which itself queries the registry and forwards the request to a healthy instance.
```
Service A → Load Balancer (fixed address) → Load Balancer queries registry → forwards to a healthy Service B instance
```
Simpler for individual services (they just call one fixed address), with the discovery/routing complexity centralized in the load balancer/proxy layer — this is the more common pattern in modern container orchestration (e.g., Kubernetes Services work this way).

## Service registries (common tools)
- **Consul**: a popular, general-purpose service registry with health checking.
- **etcd**: a distributed key-value store, often used as the backing registry for Kubernetes itself.
- **Eureka** (Netflix): a well-known service registry from the early microservices era, still used in many Spring-based systems.
- **Kubernetes' built-in service discovery**: Kubernetes Services provide DNS-based discovery automatically — `http://orders-service` resolves to a healthy pod IP, with the complexity handled entirely by the platform, which is a major reason Kubernetes became the default microservices deployment platform.

## Health checks (a prerequisite for discovery to actually work safely)
A service registry is only useful if it accurately knows which instances are actually healthy — this requires every service to expose a health check endpoint (`GET /health`) that the registry (or orchestrator) polls regularly, automatically removing unhealthy instances from the pool of addresses returned to callers.

## Practical example
In a Kubernetes deployment, calling another microservice is as simple as making an HTTP request to `http://payments-service.default.svc.cluster.local` — Kubernetes' internal DNS and service discovery handle resolving that name to a currently-healthy pod IP automatically, completely abstracting away the dynamic, ever-changing nature of container IP addresses from the calling application code.
