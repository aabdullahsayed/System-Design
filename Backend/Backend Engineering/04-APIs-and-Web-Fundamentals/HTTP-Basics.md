# HTTP Basics

## What is it?
HTTP (HyperText Transfer Protocol) is the request-response protocol that powers essentially all web communication — every API call, every webpage load, is an HTTP request/response underneath.

## Why care?
Every backend engineer builds systems that speak HTTP. Understanding it deeply (not just "GET fetches, POST sends") is what lets you design correct, debuggable, standards-compliant APIs.

## Anatomy of a request
```
GET /api/users/123 HTTP/1.1
Host: api.example.com
Authorization: Bearer eyJhbGc...
Accept: application/json

(body — often empty for GET)
```

## Anatomy of a response
```
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 87

{"id": 123, "name": "Alice"}
```

## HTTP methods (verbs) and their meaning
| Method | Purpose | Idempotent? | Has body? |
|---|---|---|---|
| `GET` | Retrieve a resource | Yes | No |
| `POST` | Create a resource | No | Yes |
| `PUT` | Replace a resource entirely | Yes | Yes |
| `PATCH` | Partially update a resource | No (by convention, though could be) | Yes |
| `DELETE` | Remove a resource | Yes | Usually no |

**Idempotent** means calling it multiple times has the same effect as calling it once — `PUT /users/1 {name: "Bob"}` called 5 times leaves the same end state as calling it once; `POST /users` called 5 times creates 5 users. This distinction matters directly for retry logic — it's safe to blindly retry an idempotent request after a network timeout, but not a non-idempotent one.

## Status codes (know these categories)
```
2xx Success        200 OK, 201 Created, 204 No Content
3xx Redirection     301 Moved Permanently, 304 Not Modified
4xx Client Error      400 Bad Request, 401 Unauthorized, 403 Forbidden, 404 Not Found, 429 Too Many Requests
5xx Server Error        500 Internal Server Error, 502 Bad Gateway, 503 Service Unavailable
```
Using the RIGHT status code (not just `200` or `500` for everything) is a real, evaluated API design skill — it lets clients (and monitoring systems) programmatically understand what happened without parsing response bodies.

## Headers worth knowing
```
Content-Type: application/json        -- format of the request/response body
Authorization: Bearer <token>          -- auth credentials (05-Authentication-and-Security)
Cache-Control: max-age=3600              -- caching directives (06-Caching)
ETag: "33a64df..."                         -- a version identifier for conditional requests
```

## In backend engineering
- **REST APIs** (`REST-APIs.md`) are built entirely on correct, idiomatic use of these methods and status codes.
- **Debugging with curl/Postman**: understanding raw HTTP is what lets you debug an API issue directly, independent of any specific client library that might be hiding what's actually being sent/received.
```bash
curl -X POST https://api.example.com/users -H "Content-Type: application/json" -d '{"name":"Alice"}'
```
- **Load balancers, reverse proxies, and CDNs** (`10-DevOps-and-Deployment`, `06-Caching`) all operate by inspecting and routing based on these exact HTTP concepts (methods, headers, status codes).
