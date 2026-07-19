# OAuth2

## What is it?
OAuth2 is an authorization framework that lets a user grant a THIRD-PARTY application limited access to their data on another service, WITHOUT sharing their password with that third party — the standard behind "Sign in with Google/GitHub/Facebook."

## Why care?
Almost every modern backend integrates with at least one OAuth2 provider (for social login) or implements OAuth2 itself (to let third-party developers securely access your API on a user's behalf). Understanding the flow is essential, and it's frequently misunderstood/conflated with plain authentication.

## Key roles in OAuth2
```
Resource Owner:    the user, who owns the data
Client:            the application requesting access (e.g., a third-party app)
Authorization Server: issues tokens after the user grants consent (e.g., Google's login screen)
Resource Server:     the API that actually holds the protected data (e.g., Google's API)
```

## The Authorization Code flow (the most common, most secure flow)
```
1. User clicks "Sign in with Google" on your app.
2. Your app redirects the user to Google's authorization server, with your app's client_id and requested scopes (e.g., "read profile, read email").
3. User logs into Google (if not already) and approves the requested permissions.
4. Google redirects back to your app with a temporary AUTHORIZATION CODE.
5. Your BACKEND (never the frontend directly, for security) exchanges this code + your client_secret for an ACCESS TOKEN, via a direct server-to-server call to Google.
6. Your backend uses that access token to call Google's API on the user's behalf (e.g., fetch their profile info).
```
The key security property: the actual access token exchange (step 5) happens server-to-server, never exposing your `client_secret` to the browser/frontend — this is why the authorization code flow is preferred over older, less secure flows (like the deprecated "implicit flow").

## OAuth2 is authorization, NOT authentication (an important, commonly-confused distinction)
OAuth2 itself only proves "this app has permission to access X data on the user's behalf" — it does NOT, by itself, prove identity in a standardized way. **OpenID Connect (OIDC)** is a thin identity layer BUILT ON TOP of OAuth2 specifically to standardize authentication ("who is this user") — this is what actually powers "Sign in with Google" style login, returning a standardized **ID Token** (a JWT containing verified identity claims) alongside the OAuth2 access token.

## Scopes (limiting what access is granted)
```
scope=profile email          -- only requests read access to basic profile and email
scope=profile email calendar   -- also requests calendar access
```
Always request the MINIMUM scopes your application actually needs — both a security best practice and something users notice/trust more on the consent screen.

## In backend engineering
- **Implementing "Sign in with X"**: use a well-established library (Passport.js, Authlib, Spring Security OAuth) rather than hand-rolling the OAuth2 flow yourself — the protocol has many security-sensitive details (state parameter for CSRF protection, PKCE for public clients) that are easy to get wrong.
- **Building your own OAuth2 provider** (letting third-party developers integrate with YOUR API) is a significantly more involved undertaking — most companies use established libraries/services (Auth0, Okta, AWS Cognito) rather than building this from scratch.
- **PKCE (Proof Key for Code Exchange)**: an extension to the authorization code flow, now recommended even for confidential clients, that prevents authorization code interception attacks — worth knowing the term even if the underlying library handles the implementation details.
