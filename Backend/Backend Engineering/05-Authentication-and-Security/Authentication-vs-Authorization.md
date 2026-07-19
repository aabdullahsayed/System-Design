# Authentication vs Authorization

## What is it?
**Authentication** answers "who are you?" (verifying identity). **Authorization** answers "what are you allowed to do?" (permission checking) — two related but distinct concerns, often confused, that every backend system needs to handle correctly.

## Why care?
Mixing these up (or skipping one) is a genuinely common, serious security bug — e.g., checking that a user is LOGGED IN (authentication) but forgetting to check whether they OWN the specific resource they're trying to access (authorization) is a classic vulnerability (an "IDOR" — Insecure Direct Object Reference).

## Authentication (AuthN) — verifying identity
Common mechanisms:
- **Username/password** — the most familiar, requires careful password handling (`Password-Security.md`).
- **Tokens (JWT)** — a signed credential proving identity without re-checking a database every request (`JWT.md`).
- **OAuth2 / "Sign in with Google"** — delegating identity verification to a trusted third party (`OAuth2.md`).
- **API keys** — common for service-to-service or third-party developer access.
- **Multi-Factor Authentication (MFA)**: adding a second verification factor (SMS code, authenticator app) beyond just a password, significantly raising the bar against credential theft.

## Authorization (AuthZ) — verifying permission
Common models:
- **Role-Based Access Control (RBAC)**: users have roles (`admin`, `editor`, `viewer`), and roles have permissions.
```python
if user.role != "admin":
    return 403  # Forbidden
```
- **Attribute-Based Access Control (ABAC)**: more granular — permissions depend on attributes of the user, resource, and context (e.g., "editors can edit posts they authored, during business hours").
- **Resource ownership checks**: the most commonly-missed check — confirming the authenticated user actually owns/has rights to the SPECIFIC resource they're requesting, not just that they're logged in generally.
```python
# WRONG: only checks the user is authenticated, not that they own this specific order
@login_required
def get_order(order_id):
    return Order.find(order_id)

# RIGHT: also verifies ownership
@login_required
def get_order(order_id):
    order = Order.find(order_id)
    if order.user_id != current_user.id:
        return 403  # Forbidden — this isn't YOUR order
    return order
```

## The critical distinction, in one sentence
`401 Unauthorized` means authentication failed (you're not logged in, or your credentials are invalid). `403 Forbidden` means authentication succeeded but authorization failed (we know who you are, but you're not allowed to do this) — using these status codes correctly and consistently is a real, checkable API design detail.

## In backend engineering
- **Middleware pattern**: most frameworks implement authentication as middleware that runs before route handlers, attaching the authenticated user to the request — authorization checks then happen either in shared middleware (for role checks) or explicitly inside each handler (for resource-specific ownership checks).
- **Never trust the client** to enforce authorization — a "hide this button if not admin" frontend check is a UX nicety, not a security control; the actual authorization check MUST happen server-side, on every request, since a client can always be bypassed (a modified request, a direct API call).
