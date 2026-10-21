# 05. Engineering Mindset: Thinking About JWT So It Works Everywhere

Principles that hold regardless of language, framework, or company.

## 1. Start from the threat, not the token

Ask before writing any code:
- Who are the **issuers** and who are the **verifiers**?
- What happens if a token is **stolen** (and for how long is it usable)?
- What happens if a **verifier is compromised** (can it forge tokens)?
- What happens if a **signing key leaks**? Can we rotate without downtime?
- How do we **revoke** one user, one session, everyone?

If you can answer these five, the design is usually sound.

## 2. Decide: do you even need JWT?

| Situation | Better choice |
|-----------|---------------|
| Single web app + single backend, same domain | **Server session** with an opaque id in an HttpOnly cookie. Simpler, instantly revocable |
| Many services need to verify identity without calling a central store | **JWT** (asymmetric) |
| Third-party/partner APIs, federation, SSO (OIDC) | **JWT** (it is the standard) |
| Mobile + web + API with shared identity provider | JWT access + opaque refresh |
| Need instant revocation, real-time permission changes | Opaque tokens + introspection, or sessions |
| Short-lived signed links (reset, verify, download) | Signed token (JWT or HMAC-signed payload) with short `exp`, single use |
| Storing user preferences/cart/state | **Not JWT**; use a database |

> "JWT is not a better cookie." Choose it for **stateless cross-service verification**, not by default.

## 3. Minimize trust; verify everything on the receiving side

The verifier owns the policy, not the token:
- **Algorithm:** from config, not the header.
- **Key:** from your trusted key set, not the header.
- **Issuer / audience / type / expiry:** always checked.
- **Claims are inputs, not commands.** A claim saying `admin: true` is only as trustworthy as the issuer's logic.

## 4. Short-lived + replaceable beats long-lived + protected

You can never fully protect a bearer token. You *can* make it nearly worthless quickly. Minutes of validity, refresh rotation, and reuse detection give far more safety than clever storage tricks.

## 5. Design for rotation and failure from day one

- Every key has a `kid`; verifiers accept a **set** of keys.
- Rotation is a routine, rehearsed operation, not an incident.
- Auth server down? Verifiers keep working until tokens expire (a JWT strength). Cache JWKS with sensible TTL and stale-while-error.
- Clock drift, cache poisoning, and unknown `kid` floods are failure modes; rate-limit and bound them.

## 6. Keep the token small and boring

- Identity + a few authorization hints. Nothing else.
- If you are tempted to add a 20th claim, you want a database lookup (or an introspection call) instead.
- Stable identifiers (`sub` as UUID), never mutable ones (email, username).

## 7. Separate concerns

- **Authentication** (who are you) vs **authorization** (what may you do): token carries the first and coarse hints of the second; services enforce the rest.
- **Token types** are different things: access, refresh, ID, reset-link. Different `typ`, `aud`, TTL, and ideally keys.
- **Environments** never share keys or issuers.

## 8. Use boring, proven building blocks

- Use a maintained library and a standard flow (OIDC, Authorization Code + PKCE).
- Don't write your own crypto, parser, or signature comparison.
- If you can buy/use an identity provider (Keycloak, Auth0, Cognito, Entra ID, Firebase Auth, Ory, etc.), you inherit years of hardening. Build the auth server yourself only with a clear reason.

## 9. Observability without leakage

- Log: issuer, `sub`, `sid`, `jti`, `kid`, failure reason (`expired`, `bad_signature`, `aud_mismatch`).
- Never log: the token itself, refresh tokens, secrets.
- Alert on: spikes in signature failures, refresh-token reuse events, unknown `kid`, unusual geographic/device changes per session.

## 10. Make secure the default and test the failure paths

Write tests that must **fail** verification:
- Expired token, not-yet-valid token
- Wrong `iss`, wrong `aud`, wrong `typ`
- `alg: none`, `alg` swapped to HS256 with the public key
- Tampered payload with original signature
- Unknown `kid`, malformed token, missing parts
- Refresh token reuse (family revoked)
- Token signed by a staging key presented to production

A JWT implementation is only as good as the negative tests.

## Decision flow (quick)

```
Need cross-service/third-party verification?
 |-- No  -> use server sessions (opaque id, HttpOnly cookie). Done.
 |-- Yes -> JWT access token (asymmetric, ES256/EdDSA, 5-15 min, aud/iss/typ enforced)
            + opaque rotating refresh token (hashed in DB, reuse detection)
            + JWKS with kid rotation
            + revocation: short TTL + (sid check | jti denylist | user ver)
            + resource-level authorization inside each service
```

## Common mental traps

- "It's signed, so it's secure." Signed means *untampered*, not *safe to trust blindly* or *private*.
- "Stateless means no state." You still need state for refresh tokens, revocation, and keys.
- "We'll add expiry later." Tokens with no expiry are permanent passwords.
- "The gateway checks it, so services don't have to." One misrouted request bypasses everything.
- "Roles in the token are always correct." They are correct as of issue time.
