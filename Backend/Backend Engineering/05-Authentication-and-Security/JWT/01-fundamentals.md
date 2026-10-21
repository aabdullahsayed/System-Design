# 01. JWT Fundamentals

## What a JWT is

A compact, URL-safe string that carries a set of **claims** (statements about a subject) and is protected by a cryptographic signature or encryption.

```
eyJhbGciOiJFUzI1NiIsInR5cCI6ImF0K2p3dCIsImtpZCI6ImtleS0xIn0.eyJzdWIiOiJ1XzEyMyIsImV4cCI6MTc2MTA0MDAwMH0.<signature>
|-------------------- header ---------------------------|.|------------ payload ------------|.|-- sig --|
```

Three parts, separated by dots, each **base64url** encoded:

1. **Header**: metadata about the token. Algorithm (`alg`), key id (`kid`), type (`typ`).
2. **Payload**: the claims.
3. **Signature**: computed over `base64url(header) + "." + base64url(payload)` using the key.

Example decoded:

```json
// header
{ "alg": "ES256", "typ": "at+jwt", "kid": "key-2026-10" }

// payload
{
  "iss": "https://auth.example.com",
  "sub": "u_123",
  "aud": "https://api.example.com",
  "exp": 1761040000,
  "iat": 1761039100,
  "jti": "b1c7...",
  "roles": ["editor"]
}
```

## The single most important fact

**Base64url is encoding, not encryption.** Anyone holding a JWT can read the payload. The signature only proves it was not altered and who issued it.

> Never put passwords, secrets, full PII, or anything you would not print in a log into a signed JWT.

## JWS vs JWE vs JWT

| Term | Meaning |
|------|---------|
| JWT | The claims format (RFC 7519) |
| JWS | A JWT that is **signed**. This is what people almost always mean by "JWT" |
| JWE | A JWT that is **encrypted** (5 parts). Content is hidden from the bearer |
| JWK / JWKS | Key / set of keys in JSON form, used to publish public keys |

Most systems use JWS. Use JWE only if the bearer (browser, mobile client, third party) must not see the contents. Often a better answer is to keep the sensitive data server-side and put only an identifier in the token.

## Verification: what the receiver must do

1. Split into three parts; reject if not exactly three (or five for JWE).
2. Parse the header. **Do not trust it.**
3. Pick the key from your own configuration (using `kid` only as a lookup hint into a trusted key set).
4. Enforce an **allowlist of algorithms** that you configured. Never take the algorithm from the token.
5. Verify the signature.
6. Validate claims: `exp`, `nbf`, `iss`, `aud`, and `typ`. Then your own checks (`sub` exists, not disabled, etc.).
7. Only now treat the payload as trustworthy.

Decoding a token is not verifying it. Many real bugs are code that decodes and uses the payload without step 5.

## Stateless vs stateful

| | Server session (opaque id) | JWT |
|---|---|---|
| Lookup per request | Yes (store/cache) | No (verify signature locally) |
| Revoke instantly | Easy (delete session) | Hard (needs extra mechanism) |
| Scale across services | Needs shared store | Verify with a public key |
| Token size | Tiny | Hundreds of bytes to KB |
| Data freshness | Always current | Frozen at issue time |

Neither is "better". JWT trades revocability and freshness for lookup-free verification.

## Where JWTs commonly appear

- OAuth 2.0 / OpenID Connect: ID tokens (always JWT), access tokens (often JWT)
- API authentication between a frontend and a backend
- Service-to-service authentication in microservices
- Short-lived signed links (email verification, password reset, download URLs)
- Webhook / event signatures

## Size and transport

- Sent as `Authorization: Bearer <token>` or in an HttpOnly cookie.
- Keep tokens small. Headers are usually limited (often 8 KB total per request); large role/permission arrays break this and add bandwidth to every call.
- Never put a JWT in a URL query string (it lands in logs, history, referrers).
