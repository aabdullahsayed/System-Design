# 02. Claims and Algorithms

## Registered claims (RFC 7519)

| Claim | Name | Meaning | Validate? |
|-------|------|---------|-----------|
| `iss` | Issuer | Who created the token | **Yes**, exact match against expected issuer |
| `sub` | Subject | Who the token is about (stable user/service id) | Yes, must exist; use an immutable id, not an email |
| `aud` | Audience | Who the token is intended for | **Yes**, your service must be in it |
| `exp` | Expiration | Unix time after which it is invalid | **Always** |
| `nbf` | Not before | Unix time before which it is invalid | If present |
| `iat` | Issued at | When it was created | Useful for "issued before password change" checks |
| `jti` | JWT ID | Unique id | Needed for denylisting / one-time use |

Times are **seconds** since epoch (not milliseconds). A classic bug is `Date.now()` (ms) put into `exp`.

## Custom claims

Add only what the verifier needs to make a decision without a lookup.

Good candidates:
- `sid`: session id (lets you revoke a session server-side)
- `roles` / `scope`: coarse permissions
- `tenant_id` / `org_id`: multi-tenant routing
- `ver`: token/user version for bulk invalidation

Poor candidates:
- Email, name, phone, address (PII, stale, bloats size)
- Fine-grained permission lists that change often
- Anything secret
- Anything you expect to be "live" (balance, plan status)

Namespace custom claims that might collide with public claims (e.g. `https://example.com/claims/tenant`) when interoperating with third parties.

## Header parameters you will see

| Param | Meaning | Note |
|-------|---------|------|
| `alg` | Signing algorithm | **Never trust; enforce an allowlist** |
| `typ` | Token type | Use explicit types like `at+jwt` to stop cross-use between token kinds |
| `kid` | Key ID | Lookup hint into your trusted key set only |
| `jku`, `x5u`, `jwk`, `x5c` | Key location/embedded key | **Ignore** unless you have a strict allowlist; classic attack vector |

## Signing algorithms

### Symmetric (shared secret)

| Alg | Notes |
|-----|-------|
| HS256 / HS384 / HS512 | HMAC-SHA2. One secret both signs and verifies. |

- Anyone who can verify can also **forge**. Fine for a single service that both issues and verifies.
- The secret must be high entropy (at least 32 random bytes for HS256), not a human password.

### Asymmetric (key pair)

| Alg | Notes |
|-----|-------|
| RS256 / RS384 / RS512 | RSA PKCS#1 v1.5. Widely supported, large keys/signatures (2048+ bit, ideally 3072) |
| PS256 | RSA-PSS. Better construction than RS256, less widely supported |
| ES256 / ES384 | ECDSA on P-256/P-384. Small tokens, fast, widely supported. **Good default** |
| EdDSA (Ed25519) | Modern, fast, small, deterministic. Excellent where libraries support it |

- Private key signs (only the issuer has it). Public key verifies (anyone can have it, via JWKS).
- **Prefer asymmetric whenever more than one party verifies.** A compromised verifier then cannot mint tokens.

### `none`

`alg: none` means unsigned. Libraries have historically accepted it. Reject it always.

## How to choose

```
Only one service issues AND verifies?        -> HS256 is acceptable (strong random secret)
Multiple services / third parties verify?    -> ES256 (or EdDSA / RS256 if required for compatibility)
Need to hide payload from the bearer?        -> JWE, or better: don't put it in the token
Legacy/enterprise integration demands RSA?   -> RS256 with 2048+ bit keys, ideally PS256
```

## Choosing lifetimes

| Token | Typical lifetime |
|-------|------------------|
| Access token | 5 to 15 minutes |
| ID token | Minutes (used once at login) |
| Refresh token | Days to weeks, rotated, **not a JWT necessarily** |
| Email verify / reset link | 10 to 60 minutes, single use |
| Service-to-service | 1 to 5 minutes |

Shorter lifetime is the main lever you have for limiting damage from a leaked token.

## Clock skew

Servers disagree on time by a little. Allow **30 to 60 seconds** of leeway on `exp`/`nbf`/`iat` checks, not minutes. Keep servers NTP-synced.
