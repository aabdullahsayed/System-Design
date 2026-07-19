# HTTPS & TLS

## What is it?
**TLS** (Transport Layer Security, the successor to SSL) encrypts data in transit between a client and server. **HTTPS** is simply HTTP running over a TLS-encrypted connection — the standard, expected way any real backend serves traffic today.

## Why care?
Without TLS, any data sent between client and server (passwords, tokens, personal data) can be intercepted and read by anyone on the network path ("man-in-the-middle") — HTTPS is a non-negotiable baseline for any production backend, not an optional extra.

## What TLS actually provides
1. **Encryption**: data in transit can't be read by an eavesdropper.
2. **Integrity**: data can't be silently modified in transit without detection.
3. **Authentication**: the client can verify it's actually talking to the real server (not an imposter), via a certificate signed by a trusted Certificate Authority (CA).

## The TLS handshake (conceptual overview)
```
1. Client connects, requests a secure connection.
2. Server presents its TLS certificate (containing its public key, signed by a trusted CA).
3. Client verifies the certificate is valid and trusted (checks the CA signature, expiration, domain match).
4. Client and server negotiate a shared SYMMETRIC encryption key (via asymmetric key exchange) for the actual session.
5. All subsequent communication is encrypted using that fast, shared symmetric key.
```
Asymmetric (public/private key) cryptography is used only briefly, during the handshake, to securely establish a symmetric key — because symmetric encryption is much faster for the bulk of ongoing data transfer.

## Getting a TLS certificate
- **Let's Encrypt**: a free, automated, widely-trusted Certificate Authority — the modern default for most projects, especially combined with automatic renewal tools (Certbot).
- **Cloud provider-managed certificates** (AWS Certificate Manager, Cloudflare): often the simplest option when already using a cloud provider/CDN, since they handle provisioning and renewal automatically.

## Practical enforcement in a backend
```javascript
// Express.js — redirect any HTTP traffic to HTTPS
app.use((req, res, next) => {
    if (req.headers['x-forwarded-proto'] !== 'https') {
        return res.redirect(`https://${req.headers.host}${req.url}`);
    }
    next();
});
```
In practice, TLS termination often happens at a load balancer or reverse proxy (Nginx, a cloud load balancer) in front of your application servers, rather than in the application code itself — the app servers may only receive plain HTTP internally, within a trusted private network, while the public-facing edge handles TLS termination.

## HSTS (HTTP Strict Transport Security)
A response header telling browsers to ALWAYS use HTTPS for your domain going forward, even if a user types `http://` — protects against a specific downgrade-attack window on first connection.
```
Strict-Transport-Security: max-age=31536000; includeSubDomains
```

## In backend engineering
- **Certificate expiration is a real, common production incident cause** — an expired TLS certificate abruptly breaks all HTTPS traffic to your service; automated renewal (via Let's Encrypt's Certbot, or a cloud provider's managed certificates) is standard practice specifically to avoid this.
- **Never send sensitive data over plain HTTP**, even "just for internal testing" — habits formed in development often leak into production configuration mistakes.
- **mTLS (mutual TLS)**: an extension where BOTH client and server present certificates to verify each other's identity — common for securing service-to-service communication within microservices architectures, beyond just the public-facing client-to-server case.
