# Practice — Authentication and Security

1. Implement basic username/password signup and login endpoints using bcrypt for password hashing — verify that the stored value is a hash, not plaintext.
2. Implement JWT-based authentication: issue a signed JWT on login, then build a middleware that verifies the JWT on protected routes.
3. Deliberately implement an IDOR vulnerability (an endpoint that returns any order by ID without checking ownership), demonstrate the exploit, then fix it with a proper ownership check.
4. Try a basic SQL injection against a deliberately vulnerable query (in a local test environment only), then fix it using a parameterized query and confirm the injection no longer works.
5. Set up "Sign in with Google" (or GitHub) using a library like Passport.js or Authlib — trace through which step is the redirect, which is the code exchange, and which is the final token usage.
6. Add rate limiting to a login endpoint (even a simple in-memory counter) to prevent brute-force attempts.
7. Explain in your own words: why is a JWT with no expiration date a security risk, even though its signature can't be forged?

✅ Done? Move to `06-Caching`.
