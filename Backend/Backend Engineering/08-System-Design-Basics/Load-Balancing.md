# Load Balancing

## What is it?
A load balancer distributes incoming requests across multiple backend server instances, so no single server is overwhelmed, and traffic continues to be served even if one instance fails.

## Why care?
Load balancing is the essential enabling technology for horizontal scaling (`Scalability-Concepts.md`) — without it, you can't actually distribute traffic across multiple servers in a useful way.

## How it works (conceptually)
```
                    ┌──→ Server 1
Client → Load Balancer ──→ Server 2
                    └──→ Server 3
```
The load balancer sits in front of your application servers, receiving all incoming traffic and forwarding each request to one of the backend instances based on a chosen algorithm.

## Common load balancing algorithms
| Algorithm | How it works | Best for |
|---|---|---|
| **Round Robin** | Cycles through servers in order | Simple, servers of equal capacity |
| **Least Connections** | Routes to the server with fewest active connections | Uneven request durations |
| **IP Hash** | Routes based on a hash of the client's IP — same client consistently hits the same server | When some session affinity is needed without full stateless design |
| **Weighted** | Some servers get proportionally more traffic (e.g., more powerful hardware) | Heterogeneous server capacity |

## Health checks (a critical load balancer responsibility)
```
Load balancer periodically pings each server: GET /health
  → 200 OK: server stays in the rotation
  → timeout / non-200: server is temporarily REMOVED from rotation until it recovers
```
This is what allows a system to automatically route around a failed/unhealthy server instance without manual intervention — a core building block of fault tolerance.

## Layer 4 vs Layer 7 load balancing
- **Layer 4 (Transport layer)**: routes based on IP/port only, without inspecting the actual request content — faster, simpler, but less flexible.
- **Layer 7 (Application layer)**: inspects the actual HTTP request (path, headers, cookies) to make more intelligent routing decisions (e.g., route `/api/*` to one server pool, `/static/*` to another) — more flexible, common for modern web architectures (Nginx, AWS ALB operate at this layer).

## TLS Termination
Load balancers commonly handle TLS/HTTPS decryption (`05-Authentication-and-Security/HTTPS-TLS.md`) at the edge, then forward plain HTTP internally to backend servers within a trusted private network — centralizing certificate management and reducing CPU overhead on individual application servers.

## Popular load balancers
Nginx, HAProxy (self-managed); AWS Application Load Balancer, Google Cloud Load Balancing, Azure Load Balancer (cloud-managed).

## In backend engineering
- **This is exactly what makes horizontal scaling actually work in practice** — combined with statelessness (`Scalability-Concepts.md`), a load balancer is what turns "3 separate servers" into what feels, from the outside, like one large, resilient, capacity-scaled service.
- **Session affinity ("sticky sessions") is a trade-off, not a default best practice**: routing a client's requests consistently to the same server (via IP Hash or cookie-based affinity) can help with legacy stateful apps, but undermines the resilience benefits of load balancing (that server going down disrupts all its "sticky" clients) — a fully stateless design is generally the more robust modern approach.
