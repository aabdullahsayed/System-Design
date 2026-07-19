# API Design Best Practices

## What is it?
A collection of practical conventions that make an API predictable, easy to integrate with, and maintainable over time — distinct from any specific technology (REST/GraphQL/gRPC), these apply broadly.

## Why care?
A technically-functional API that's inconsistent or confusing creates real integration pain for every client developer using it (including your own future teammates) — good API design is a genuinely evaluated, high-leverage skill.

## Consistency
- **Consistent naming**: `snake_case` or `camelCase` — pick one, use it everywhere. Same for pluralization (`/users` not a mix of `/user` and `/users` across endpoints).
- **Consistent response envelope**: decide on one response shape (e.g., always `{"data": ..., "meta": ...}` or always the bare resource) and don't deviate endpoint-to-endpoint.
- **Consistent error format**:
```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Email is required",
    "field": "email"
  }
}
```

## Validate input, fail clearly
```javascript
if (!req.body.email) {
    return res.status(400).json({
        error: { code: "MISSING_FIELD", message: "Email is required", field: "email" }
    });
}
```
Never trust client input — validate everything server-side, even if the frontend also validates (frontend validation can always be bypassed by a direct API call).

## Idempotency for critical operations
For operations like payments, support an `Idempotency-Key` header — if a client retries a request (e.g., after a timeout, unsure if the first attempt succeeded) with the same key, the server returns the ORIGINAL result instead of processing it twice. Essential for any "money-moving" or otherwise non-repeatable-safe operation.

## Rate limiting and clear communication of limits
```
HTTP/1.1 429 Too Many Requests
Retry-After: 30
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 0
```
See `12-Scalability-and-Performance/Rate-Limiting.md` for implementation — but at minimum, communicate limits clearly via headers so well-behaved clients can self-throttle.

## Documentation (non-negotiable for any real API)
- **OpenAPI/Swagger spec**: a machine-readable API contract that can auto-generate interactive documentation, client SDKs, and even server stub code.
- Include realistic example requests/responses, not just abstract type definitions — this is often what saves an integrating developer the most time.

## Backward compatibility mindset
Treat any deployed, externally-used API as a promise — adding new optional fields is safe; removing/renaming fields or changing their type is a **breaking change** requiring careful versioning (`API-Versioning.md`) and migration communication to consumers.

## Practical example
Stripe's API is widely regarded as an example of excellent API design: consistent resource naming, thorough documentation with live examples, clear and well-differentiated error codes, built-in idempotency key support for payment operations, and careful, well-communicated versioning — worth studying directly as a real-world reference.
