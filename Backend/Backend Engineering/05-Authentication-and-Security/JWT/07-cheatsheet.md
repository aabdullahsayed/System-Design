# 07. JWT Cheatsheet

## Build checklist

**Issuing**
- [ ] Asymmetric alg (ES256 / EdDSA) if more than one verifier; HS256 only for single-service with 32+ random bytes
- [ ] `kid` in header, `typ` set (`at+jwt` for access tokens)
- [ ] Claims: `iss`, `sub` (immutable id), `aud`, `exp`, `iat`, `jti`; coarse `roles`/`scope`, `sid`
- [ ] Access TTL 5 to 15 min
- [ ] No secrets or PII in payload
- [ ] Private key in KMS / secret manager

**Verifying**
- [ ] Algorithm allowlist from config (reject `none`, never use header `alg` blindly)
- [ ] Key from trusted JWKS / config; ignore `jwk`, `jku`, `x5u`, `x5c`
- [ ] Check `exp`, `nbf`, `iss`, `aud`, `typ`
- [ ] Leeway 30 to 60 s
- [ ] Resource-level authorization after authentication
- [ ] Sensitive actions re-check the source of truth

**Sessions**
- [ ] Opaque refresh token, hashed at rest, rotated each use, reuse detection revokes family
- [ ] Browser: HttpOnly + Secure + SameSite cookie (or access token in memory + refresh in cookie)
- [ ] Revocation path: short TTL plus `sid` check / `jti` denylist / user `ver`
- [ ] "Log out everywhere" and password-change invalidation work

**Operations**
- [ ] JWKS endpoint with cache headers; rotation procedure rehearsed
- [ ] Separate keys per environment
- [ ] Tokens redacted from logs/APM
- [ ] Alerts: signature failures, refresh reuse, unknown `kid`
- [ ] Negative tests in CI
- [ ] JWT library pinned and updated

## Do / Don't

| Do | Don't |
|----|-------|
| Enforce algorithm allowlist | Trust `alg` from the token |
| Validate `aud` and `iss` | Accept any validly signed token |
| Keep tokens short-lived | Issue non-expiring tokens |
| Use `HttpOnly` cookies in browsers | Put tokens in `localStorage` |
| Keep payload minimal | Store PII/secrets/state in the token |
| Rotate keys with `kid` + JWKS | Hard-code one key forever |
| Use maintained libraries | Hand-roll verification |
| Separate token types (`typ`, `aud`) | Reuse ID token as access token |
| Plan revocation up front | Assume `exp` is enough |
| Check object-level permissions | Rely on `role` claim alone |

## Common error mapping (HTTP)

| Situation | Status | Hint |
|-----------|--------|------|
| No/malformed token | 401 | `WWW-Authenticate: Bearer` |
| Expired | 401 | Client should refresh |
| Bad signature / wrong aud / iss | 401 | Don't reveal which check failed to the caller in detail |
| Valid token, not allowed | 403 | Authorization failure, not authentication |

## Review questions for a design or PR

1. What is the lifetime of each token and why that number?
2. Who can mint tokens? Who can only verify?
3. How do we revoke a single session? A user? Everyone?
4. How do we rotate the signing key without downtime?
5. Where does the browser/mobile client keep tokens? What does XSS or device theft yield?
6. What stops a token meant for service A from being used on service B?
7. What claims could be stale, and what is the impact?
8. Do we have tests that prove forged/expired/wrong-audience tokens fail?
9. Could a plain server session solve this more simply?

## Quick decode (debugging only)

```bash
# Decode payload locally (do NOT paste production tokens into online tools)
echo "$TOKEN" | cut -d. -f2 | tr '_-' '/+' | base64 -d 2>/dev/null | jq .
```

## Remember

1. Signed is not secret.
2. Decode is not verify.
3. Short-lived beats well-protected.
4. The verifier decides policy; the token never does.
5. Stateless is a trade-off, not a free lunch.
