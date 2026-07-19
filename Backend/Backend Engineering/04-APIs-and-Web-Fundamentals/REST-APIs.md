# REST APIs

## What is it?
REST (REpresentational State Transfer) is an architectural style for designing web APIs around **resources** (nouns, like `users` or `orders`), manipulated via standard HTTP methods — the dominant API style for web backends.

## Why care?
The vast majority of backend engineering job postings mention "building REST APIs." It's the default, expected skill for connecting a backend to any frontend, mobile app, or third-party integration.

## Core principles
1. **Resources are nouns, identified by URLs**: `/users`, `/users/123`, `/users/123/orders` — not verbs in the URL (`/getUser` is NOT RESTful).
2. **HTTP methods express the action**: `GET /users/123` (read), `POST /users` (create), `PUT/PATCH /users/123` (update), `DELETE /users/123` (remove).
3. **Stateless**: each request contains everything needed to process it (e.g., auth token) — the server doesn't rely on remembered session state between requests, which is essential for horizontal scaling (any server instance can handle any request).
4. **Standard status codes and JSON** (typically) as the data format.

## Designing REST endpoints — a worked example
```
GET    /api/orders              → list all orders
GET    /api/orders/42            → get order 42
POST   /api/orders                → create a new order
PUT    /api/orders/42               → replace order 42 entirely
PATCH  /api/orders/42                → partially update order 42
DELETE /api/orders/42                 → delete order 42

GET    /api/orders/42/items             → list items within order 42 (nested resource)
```

## Query parameters for filtering, sorting, pagination
```
GET /api/orders?status=shipped&sort=-created_at&page=2&limit=20
```
Standard, expected conventions — filtering via query params, `-field` for descending sort, `page`/`limit` (or cursor-based, see below) for pagination.

## Example implementation (Express.js)
```javascript
app.get('/api/orders/:id', async (req, res) => {
    const order = await Order.findById(req.params.id);
    if (!order) {
        return res.status(404).json({ error: "Order not found" });
    }
    res.status(200).json(order);
});

app.post('/api/orders', async (req, res) => {
    const order = await Order.create(req.body);
    res.status(201).json(order);   // 201 Created, not 200 — a real convention worth knowing
});
```

## Pagination (essential for any list endpoint at scale)
- **Offset-based** (`?page=2&limit=20`): simple, but can skip/duplicate rows if data changes between page loads, and gets slower on large offsets.
- **Cursor-based** (`?after=<last_seen_id>&limit=20`): more robust for large, frequently-changing datasets — the standard approach used by most large-scale APIs (Twitter, GitHub, Stripe).

## In backend engineering
- **Versioning** (`API-Versioning.md`) is essential once an API has real external consumers — breaking changes without a versioning strategy break every client relying on the old behavior.
- **Consistent error response shape** across all endpoints (`{"error": {"code": "...", "message": "..."}}`) makes an API dramatically easier for client developers to handle reliably.
- **HATEOAS** (Hypermedia As The Engine Of Application State) is the "purist" full REST vision (responses include links to related actions) — rarely fully implemented in practice, but worth knowing the term for interviews/discussions about REST maturity levels.
