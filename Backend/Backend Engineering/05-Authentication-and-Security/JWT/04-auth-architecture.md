# 04. Auth Architecture with JWT

## The standard pattern: short access token + rotating refresh token

```
Client                         Auth server                    API
  | -- login (credentials) ----> |                             |
  | <- access JWT (10m)          |                             |
  | <- refresh token (30d) ----- |                             |
  |                                                            |
  | -- Authorization: Bearer <access JWT> -------------------> |  verify locally
  |                                                            |
  | (access expired)                                           |
  | -- refresh token ----------> |  validate, rotate           |
  | <- new access JWT + NEW refresh token                      |
```

### Access token
- JWT, 5 to 15 minutes.
- Verified locally by every API; no network call.
- Carries `sub`, `sid`, `aud`, `iss`, `exp`, coarse roles/scope.

### Refresh token
- Usually an **opaque random string** (32+ bytes from a CSPRNG), not a JWT. You need server-side state for it anyway, so you gain nothing from JWT here.
- Store only a **hash** (SHA-256) in the database, never the raw value.
- Bind to: user, session/device, created-at, expires-at, `family_id`, `used_at`.
- **Rotate on every use**: issue a new refresh token, invalidate the old one.
- **Reuse detection**: if an already-used refresh token is presented, someone copied it. Revoke the entire family (all descendants) and force re-login.

## Where to store tokens

| Client | Recommended | Avoid |
|--------|-------------|-------|
| Browser SPA / web app | `HttpOnly; Secure; SameSite=Lax/Strict` cookie, scoped by `Path` | `localStorage`, `sessionStorage`, JS-readable cookies |
| Server-rendered app | Same cookie approach, or a server session instead | |
| Mobile | Keychain (iOS) / Keystore (Android) | Plain preferences / files |
| Server to server | Env/secret manager, short-lived, fetched on demand | Hard-coded tokens |

### Cookies and CSRF
HttpOnly cookies protect against token theft via XSS, but cookies are sent automatically, so you now need CSRF defenses:
- `SameSite=Lax` (or `Strict`) as the baseline.
- CSRF token or custom-header check for state-changing requests if you need cross-site flows.
- Keep refresh token cookie on a narrow `Path` (e.g. `/auth/refresh`).

Pragmatic split often used: **access token in memory (JS variable), refresh token in HttpOnly cookie.** XSS can still call the API as the user while the page is open, but cannot exfiltrate a long-lived credential.

## Revocation strategies (pick by risk)

| Strategy | How | Cost |
|----------|-----|------|
| Short TTL only | Wait for expiry (<= 10 min) | Zero infra; accept the window |
| Session check | Token has `sid`; API checks a fast store (Redis) that session is alive | One cache lookup per request (less stateless) |
| `jti` denylist | Store revoked `jti` until their `exp` | Small set; checked on each request |
| User version / cutoff | `ver` claim or "tokens issued before T are invalid" per user | One lookup; great for "log out everywhere", password change |
| Introspection | API calls auth server (RFC 7662) | Network hop; fully authoritative |

Combine cheaply: short TTL for everyone, plus a Redis denylist/session check for sensitive endpoints.

## Key management

- Publish public keys at `/.well-known/jwks.json`. Verifiers cache the JWKS and refetch on unknown `kid` (with rate limiting).
- **Every key has a `kid`.** Sign with the newest key; verifiers accept all current public keys.
- **Rotation procedure:**
  1. Generate new key pair, add its public key to JWKS.
  2. Wait for caches to refresh (more than cache TTL).
  3. Start signing with the new key.
  4. Keep the old public key until all tokens it signed have expired (max token TTL + skew).
  5. Remove old key.
- Private keys live in a KMS/HSM or secret manager, never in the repo or the image.
- Rotate on a schedule and immediately on suspected compromise.
- Separate keys per environment and ideally per token purpose.

## Microservices

```
Browser -> API Gateway --verify JWT--> Service A -> Service B
```

Options:
1. **Gateway verifies, services trust the gateway** over a private network. Simple, but internal services are blind if the network is breached.
2. **Every service verifies** the JWT using JWKS. Defense in depth. Recommended: verification is cheap.
3. **Token exchange** (RFC 8693) for downstream calls: exchange the user's token for a narrow, short-lived token with a different `aud` for service B. Avoids passing a broad user token everywhere and limits blast radius.

Rules:
- Each service checks its own `aud`.
- Don't forward the end-user token to unrelated services blindly.
- Service identities (machine tokens) should be separate from user tokens (`sub` is the service, scopes minimal).

## Authorization: authentication is not authorization

A valid JWT says *who* and maybe *coarse roles*. It does not mean the request is allowed.
- Check **resource-level** permissions in your service (this user may access *this* order).
- Don't rely only on a `role` claim for object-level checks (this is the root of IDOR/BOLA bugs).
- Keep policy logic server-side (RBAC/ABAC/ReBAC); the token provides inputs.

## OAuth 2.0 / OIDC notes

- **ID token** = proof of *authentication* for the client app. Don't send it to APIs as an access token.
- **Access token** = for the resource server. Client treats it as opaque; only the API inspects it.
- Use Authorization Code + PKCE for public clients (SPA/mobile). Avoid the implicit flow.
- Validate ID tokens: `iss`, `aud` (= your client id), `exp`, `nonce`, signature.
- Follow RFC 9068 for JWT-formatted access tokens (`typ: at+jwt`, required claims).
- Prefer a maintained identity provider/library over building the auth server yourself.

## Logout

- Delete cookies/memory on the client.
- Invalidate the refresh token family and the session (`sid`) on the server.
- Remember the access token remains valid until expiry unless you use a session check or denylist.
