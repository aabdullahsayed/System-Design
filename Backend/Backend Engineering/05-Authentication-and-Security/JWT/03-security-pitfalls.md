# 03. Security Pitfalls and Fixes

Each item: **what goes wrong, why, and the fix.**

## 1. `alg: none` accepted

- **Attack:** attacker sets `"alg":"none"`, removes the signature, edits claims. A naive library accepts it.
- **Fix:** pass an explicit algorithm allowlist to verify. Never derive it from the token. Use a current library.

## 2. Algorithm confusion (RS256 to HS256)

- **Attack:** the server expects RS256 and has a public key. Attacker sets `alg: HS256` and signs with the **public key as the HMAC secret**. A verifier that reads `alg` from the header and uses the same key variable will validate it.
- **Fix:** allowlist exactly the algorithm(s) you issue. Keep key objects typed per algorithm (modern libs like `jose` enforce this).

## 3. Weak HMAC secret

- **Attack:** short or guessable HS256 secret gets brute-forced offline from any one captured token.
- **Fix:** 256+ bits of random from a CSPRNG (`openssl rand -base64 32`). Store in a secret manager. Rotate.

## 4. Not validating `iss` / `aud`

- **Attack:** a token from another tenant, environment (staging), or another service in the same org is accepted. This is the **confused deputy** problem: a valid token meant for service A is replayed to service B.
- **Fix:** validate `iss` exactly and require your own identifier in `aud`. Use different keys/audiences per environment.

## 5. Not checking `exp` (or tokens that never expire)

- **Attack:** a stolen token works forever.
- **Fix:** always set and enforce `exp`. Short access tokens.

## 6. Cross-JWT confusion

- **Attack:** an ID token or a password-reset token is signed by the same key and gets accepted as an access token.
- **Fix:** distinct `typ` values (`at+jwt`), distinct `aud`, ideally distinct keys per purpose; verify `typ`.

## 7. Header-supplied keys (`jwk`, `jku`, `x5u`, `x5c`)

- **Attack:** token header points to an attacker-hosted key or embeds its own key; verifier trusts it.
- **Fix:** ignore these headers. Load keys only from your configured JWKS URL. Pin the host.

## 8. `kid` injection

- **Attack:** `kid` is used in a file path or SQL query (`../../dev/null`, `' OR 1=1`) to force a predictable key.
- **Fix:** treat `kid` as an opaque string and look it up in an in-memory map of known keys. Never concatenate it into paths or queries.

## 9. Storing tokens in `localStorage` / `sessionStorage`

- **Attack:** any XSS (including a compromised npm dependency or third-party script) reads and exfiltrates the token.
- **Fix:** prefer `HttpOnly; Secure; SameSite` cookies for browsers. See 04-auth-architecture.md for the CSRF trade-off. Mobile: OS keystore/keychain.

## 10. Sensitive data in the payload

- **Problem:** payload is readable by anyone who holds the token; also lands in logs, proxies, browser extensions.
- **Fix:** minimal claims, opaque identifiers, no secrets/PII. Use JWE only if you truly must.

## 11. No revocation story

- **Problem:** user logs out, is banned, or changes password, but their JWT is valid until `exp`.
- **Fix:** short TTL + refresh tokens + at least one of: `sid` check against a session store, `jti` denylist, per-user `ver`/`iat` cutoff. See 04.

## 12. Trusting claims that should be authoritative elsewhere

- **Problem:** role/permission in the token is stale after a change (demoted admin still has `admin` for 10 more minutes).
- **Fix:** accept the window consciously, keep TTL short, and for high-risk actions (delete account, payments, role changes) re-check the source of truth.

## 13. Replay

- **Problem:** a captured token can be reused by anyone who holds it (bearer semantics).
- **Fix:** TLS everywhere, short TTL, scoped audiences. For high assurance: sender-constrained tokens (DPoP, mTLS-bound tokens), one-time `jti` for sensitive links.

## 14. Logging tokens

- **Problem:** tokens in access logs, error trackers, analytics are credentials in plaintext.
- **Fix:** redact `Authorization` headers and cookies in logging and APM.

## 15. Using JWT for things it is bad at

- Long-lived "remember me" sessions that must be revocable.
- Storing large or frequently changing state.
- **Fix:** opaque tokens or server sessions. See 05.

## 16. Weak timing and comparison practices

- **Problem:** comparing signatures manually with `==` allows timing leaks.
- **Fix:** never hand-roll verification. Use a maintained library (`jose`, `PyJWT`, `golang-jwt`, `jjwt`, `Nimbus JOSE`, etc.).

## 17. Library hygiene

- Pin and update the JWT library; historic CVEs were mostly `none`, confusion, and header-trust bugs.
- Don't write your own parser, signer, or base64url handling.
