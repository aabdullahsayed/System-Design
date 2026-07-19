# JWT (JSON Web Tokens)

## What is it?
A JWT is a compact, self-contained, cryptographically SIGNED token that encodes claims (e.g., "user 123, role: admin, expires at...") — commonly used for stateless authentication in APIs.

## Why care?
JWTs are extremely widely used in modern backend systems for authentication, especially in stateless, horizontally-scaled architectures — understanding both how to use them AND their real security pitfalls is essential.

## Structure
A JWT is 3 base64-encoded, dot-separated parts: `header.payload.signature`
```
eyJhbGciOiJIUzI1NiJ9.eyJ1c2VySWQiOiIxMjMiLCJyb2xlIjoiYWRtaW4ifQ.4f3a2b...

Header:    {"alg": "HS256", "typ": "JWT"}
Payload:   {"userId": "123", "role": "admin", "exp": 1735689600}
Signature: HMACSHA256(base64(header) + "." + base64(payload), secret_key)
```
**Critical point**: the header and payload are only BASE64-ENCODED, not encrypted — anyone can decode and read them (try it at jwt.io). The signature is what prevents TAMPERING (you can't modify the payload without invalidating the signature, since you don't have the secret key) — but never put sensitive data (passwords, secrets) directly in a JWT payload, since it's trivially readable by anyone who has the token.

## How stateless auth with JWT works
```
1. User logs in with username/password.
2. Server verifies credentials, issues a signed JWT.
3. Client stores the JWT (typically in an httpOnly cookie, or less securely, localStorage) and sends it with every request:
   Authorization: Bearer eyJhbGc...
4. Server verifies the SIGNATURE (using its secret key) on each request — no database lookup needed to confirm identity, since the signature alone proves the token is authentic and unmodified.
```
This "no database lookup needed" property is exactly why JWTs are popular for horizontally-scaled systems — any server instance can independently verify a token without needing shared session state.

## Common real security pitfalls
- **Storing JWTs in localStorage**: vulnerable to XSS attacks (malicious JS can read localStorage and steal the token) — httpOnly cookies are generally safer, since JavaScript can't access them.
- **No expiration, or overly long expiration**: a stolen JWT with a long/no expiry is valid indefinitely — use short expiration times (e.g., 15 minutes) combined with a refresh token mechanism.
- **Can't be revoked before expiry** (a real, structural limitation): since verification doesn't check a database, there's no built-in way to instantly invalidate a specific JWT before its natural expiration (e.g., on logout, or if compromised) — mitigated via short expiry + a server-side revocation/blocklist for the rare cases that truly need immediate invalidation.
- **Algorithm confusion attacks**: always explicitly specify and verify the expected signing algorithm server-side — a known historical vulnerability class involved attackers manipulating the `alg` header to bypass signature verification entirely.

## Refresh token pattern (mitigating the short-expiry / no-revocation trade-off)
```
Access Token:  short-lived (e.g., 15 min), sent with every API request
Refresh Token: longer-lived (e.g., 7 days), stored securely, used ONLY to obtain a new access token
```
This limits the exposure window of a stolen access token while avoiding forcing users to re-login every 15 minutes — and refresh tokens CAN be checked against a database/revocation list (since they're used far less frequently), giving you a practical way to revoke a compromised session.

## Practical example
```python
import jwt
from datetime import datetime, timedelta

payload = {"user_id": "123", "role": "admin", "exp": datetime.utcnow() + timedelta(minutes=15)}
token = jwt.encode(payload, "your-secret-key", algorithm="HS256")

# Verifying (on every subsequent request)
decoded = jwt.decode(token, "your-secret-key", algorithms=["HS256"])   # raises an exception if signature/expiry is invalid
```
