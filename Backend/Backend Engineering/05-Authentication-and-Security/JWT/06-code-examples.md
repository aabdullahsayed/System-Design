# 06. Code Examples

Reference implementations showing the safe defaults: explicit algorithm, `iss`/`aud`/`typ` checks, short expiry, `kid`, JWKS, and hashed rotating refresh tokens. Adapt, don't paste blindly.

## Node / TypeScript with `jose`

```bash
npm install jose
```

### Issue an access token (auth server)

```ts
import { SignJWT, importPKCS8, exportJWK, importSPKI } from "jose";
import { randomUUID } from "node:crypto";

const ISSUER = "https://auth.example.com";
const AUDIENCE = "https://api.example.com";
const ALG = "ES256";
const KID = "key-2026-10";

// Load from a secret manager / KMS in real systems, not from the repo.
const privateKey = await importPKCS8(process.env.JWT_PRIVATE_KEY_PEM!, ALG);

export async function issueAccessToken(params: {
  userId: string;
  sessionId: string;
  roles: string[];
}) {
  return new SignJWT({ sid: params.sessionId, roles: params.roles })
    .setProtectedHeader({ alg: ALG, kid: KID, typ: "at+jwt" })
    .setIssuer(ISSUER)
    .setAudience(AUDIENCE)
    .setSubject(params.userId)
    .setIssuedAt()
    .setExpirationTime("10m")
    .setJti(randomUUID())
    .sign(privateKey);
}
```

### Verify an access token (any API)

```ts
import { jwtVerify, createRemoteJWKSet, errors } from "jose";

const JWKS = createRemoteJWKSet(
  new URL("https://auth.example.com/.well-known/jwks.json"),
  { cooldownDuration: 30_000, cacheMaxAge: 10 * 60_000 }
);

export async function verifyAccessToken(token: string) {
  try {
    const { payload } = await jwtVerify(token, JWKS, {
      issuer: "https://auth.example.com",
      audience: "https://api.example.com",
      algorithms: ["ES256"],      // allowlist, never from the token
      typ: "at+jwt",              // reject other token kinds
      clockTolerance: 30,         // seconds of skew
    });

    if (typeof payload.sub !== "string") throw new Error("missing sub");
    return payload;
  } catch (err) {
    // Log the category, never the token.
    if (err instanceof errors.JWTExpired) throw new HttpError(401, "token_expired");
    throw new HttpError(401, "invalid_token");
  }
}
```

### Express middleware

```ts
import type { Request, Response, NextFunction } from "express";

export async function requireAuth(req: Request, res: Response, next: NextFunction) {
  const header = req.get("authorization") ?? "";
  const [scheme, token] = header.split(" ");
  if (scheme !== "Bearer" || !token) return res.status(401).json({ error: "missing_token" });

  try {
    const claims = await verifyAccessToken(token);
    // Optional: revocation check for sensitive routes
    // if (await redis.exists(`revoked:sid:${claims.sid}`)) throw new Error("revoked");
    (req as any).auth = { userId: claims.sub, sid: claims.sid, roles: claims.roles ?? [] };
    next();
  } catch {
    res.status(401).json({ error: "invalid_token" });
  }
}

// Resource-level authorization still happens in the handler:
// const order = await db.orders.get(id);
// if (order.ownerId !== req.auth.userId && !req.auth.roles.includes("admin")) return res.sendStatus(403);
```

### Publish JWKS (public key)

```ts
const publicKey = await importSPKI(process.env.JWT_PUBLIC_KEY_PEM!, ALG);
const jwk = { ...(await exportJWK(publicKey)), kid: KID, alg: ALG, use: "sig" };

app.get("/.well-known/jwks.json", (_req, res) => {
  res.set("Cache-Control", "public, max-age=600");
  res.json({ keys: [jwk /*, previousKeyJwk during rotation */] });
});
```

### Refresh token: opaque, hashed, rotating, reuse-detecting

```ts
import { randomBytes, createHash } from "node:crypto";

const sha256 = (s: string) => createHash("sha256").update(s).digest("hex");

export async function createRefreshToken(userId: string, sessionId: string, familyId = randomUUID()) {
  const raw = randomBytes(32).toString("base64url");
  await db.refreshTokens.insert({
    hash: sha256(raw),
    userId, sessionId, familyId,
    expiresAt: new Date(Date.now() + 30 * 24 * 3600 * 1000),
    usedAt: null,
  });
  return raw; // return to client once; never store raw
}

export async function rotateRefreshToken(raw: string) {
  const row = await db.refreshTokens.findByHash(sha256(raw));
  if (!row || row.expiresAt < new Date()) throw new HttpError(401, "invalid_refresh");

  if (row.usedAt) {
    // Reuse detected: token was copied. Kill the whole family.
    await db.refreshTokens.revokeFamily(row.familyId);
    await db.sessions.revoke(row.sessionId);
    throw new HttpError(401, "refresh_reuse_detected");
  }

  // Mark used atomically (UPDATE ... WHERE used_at IS NULL) to avoid races.
  const ok = await db.refreshTokens.markUsed(row.hash);
  if (!ok) throw new HttpError(401, "invalid_refresh");

  const user = await db.users.get(row.userId);
  if (!user || user.disabled) throw new HttpError(401, "user_disabled");

  const access = await issueAccessToken({
    userId: user.id, sessionId: row.sessionId, roles: user.roles,
  });
  const nextRefresh = await createRefreshToken(user.id, row.sessionId, row.familyId);
  return { access, refresh: nextRefresh };
}
```

### Setting the refresh cookie

```ts
res.cookie("refresh_token", refresh, {
  httpOnly: true,
  secure: true,
  sameSite: "strict",
  path: "/auth/refresh",
  maxAge: 30 * 24 * 3600 * 1000,
});
```

### Negative tests (must all reject)

```ts
test.each([
  ["expired", makeToken({ exp: now - 120 })],
  ["wrong aud", makeToken({ aud: "other-api" })],
  ["wrong iss", makeToken({ iss: "https://evil.example" })],
  ["alg none", makeUnsignedToken()],
  ["HS256 signed with public key", makeHs256WithPublicKey()],
  ["tampered payload", tamperPayload(validToken)],
  ["wrong typ", makeToken({ typ: "JWT" })],
  ["unknown kid", makeToken({ kid: "nope" })],
])("rejects %s", async (_name, token) => {
  await expect(verifyAccessToken(token)).rejects.toBeTruthy();
});
```

## Python with `PyJWT`

```bash
pip install "pyjwt[crypto]"
```

```python
import jwt  # PyJWT
from jwt import PyJWKClient, InvalidTokenError, ExpiredSignatureError
from datetime import datetime, timedelta, timezone
import uuid

ISSUER = "https://auth.example.com"
AUDIENCE = "https://api.example.com"

def issue_access_token(private_key_pem: str, user_id: str, session_id: str, roles: list[str]) -> str:
    now = datetime.now(timezone.utc)
    claims = {
        "iss": ISSUER,
        "aud": AUDIENCE,
        "sub": user_id,
        "sid": session_id,
        "roles": roles,
        "iat": now,
        "exp": now + timedelta(minutes=10),
        "jti": str(uuid.uuid4()),
    }
    return jwt.encode(
        claims,
        private_key_pem,
        algorithm="ES256",
        headers={"kid": "key-2026-10", "typ": "at+jwt"},
    )

_jwks = PyJWKClient("https://auth.example.com/.well-known/jwks.json", cache_keys=True)

def verify_access_token(token: str) -> dict:
    try:
        header = jwt.get_unverified_header(token)
        if header.get("typ") != "at+jwt":
            raise InvalidTokenError("bad typ")
        signing_key = _jwks.get_signing_key_from_jwt(token)
        return jwt.decode(
            token,
            signing_key.key,
            algorithms=["ES256"],            # allowlist
            audience=AUDIENCE,
            issuer=ISSUER,
            leeway=30,
            options={"require": ["exp", "iat", "iss", "aud", "sub"]},
        )
    except ExpiredSignatureError:
        raise  # map to 401 token_expired
    except InvalidTokenError:
        raise  # map to 401 invalid_token
```

Note: reading the unverified header only to check `typ` is fine; the actual trust decision comes from `decode` with a fixed algorithm list and a key from your JWKS.

## Generating keys

```bash
# ES256 (P-256) key pair
openssl ecparam -name prime256v1 -genkey -noout -out ec-private.pem
openssl pkcs8 -topk8 -nocrypt -in ec-private.pem -out private-pkcs8.pem
openssl ec -in ec-private.pem -pubout -out public.pem

# HS256 secret (only if you truly use HMAC)
openssl rand -base64 32
```
