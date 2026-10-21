# JWT Notes

A practical engineering guide to JSON Web Tokens: what they are, what can go wrong, and how to design with them so the result holds up in any system.

## Contents

| File | What it covers |
|------|----------------|
| `01-fundamentals.md` | What a JWT is, structure, JWS vs JWE, how verification works |
| `02-claims-and-algorithms.md` | Registered claims, custom claims, signing algorithms and how to choose |
| `03-security-pitfalls.md` | Known attacks and misconfigurations, with the fix for each |
| `04-auth-architecture.md` | Access/refresh tokens, storage, revocation, key rotation, microservices |
| `05-engineering-mindset.md` | When to use JWT and when not to, decision rules, principles that apply everywhere |
| `06-code-examples.md` | Node/TypeScript (`jose`) and Python (`PyJWT`) reference implementations |
| `07-cheatsheet.md` | One-page checklist and review questions |

## One-paragraph summary

A JWT is a signed (and optionally encrypted) JSON payload that lets one party prove claims to another without a database lookup. The signature gives you **integrity and authenticity**, not secrecy. It is stateless, which is both its main benefit (scales, no session store) and its main cost (hard to revoke). Used well: short-lived, strictly validated, asymmetric keys when many services verify, refresh tokens that rotate. Used badly: long-lived, in `localStorage`, `alg` chosen by the token, no `aud` check.

## Key standards

- RFC 7519: JSON Web Token (JWT)
- RFC 7515: JSON Web Signature (JWS)
- RFC 7516: JSON Web Encryption (JWE)
- RFC 7517: JSON Web Key (JWK)
- RFC 7518: JSON Web Algorithms (JWA)
- RFC 8725: JWT Best Current Practices
- RFC 9068: JWT Profile for OAuth 2.0 Access Tokens
