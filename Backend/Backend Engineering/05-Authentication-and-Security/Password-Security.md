# Password Security

## What is it?
The practices for safely storing and verifying user passwords such that even if your database is breached, attackers can't easily recover the actual passwords.

## Why care?
Storing passwords incorrectly (plaintext, or weakly hashed) is one of the most damaging, entirely preventable security failures a backend engineer can be responsible for — real, well-known company breaches have stemmed directly from this exact mistake.

## NEVER store passwords in plaintext
```python
# NEVER DO THIS
user.password = request.form["password"]   # storing raw password — catastrophic if breached
```
If your database is ever breached (and assume, eventually, it might be), plaintext passwords hand attackers immediate, complete access to every user's account — and since many people reuse passwords across sites, the damage extends far beyond just your application.

## Never use plain hashing (MD5, SHA-256) alone either
```python
# ALSO WRONG — fast hashes are crackable via precomputed "rainbow tables" and brute force
password_hash = hashlib.sha256(password.encode()).hexdigest()
```
MD5/SHA-256 are designed to be FAST — which is exactly the wrong property for password storage, since it means an attacker with a stolen hash database can brute-force enormous numbers of guesses per second.

## Use a proper password hashing algorithm: bcrypt, scrypt, or Argon2
These are deliberately SLOW and include a random **salt** automatically, specifically to resist brute-force and rainbow table attacks.
```python
import bcrypt

# Hashing at signup
hashed = bcrypt.hashpw(password.encode(), bcrypt.gensalt())
user.password_hash = hashed

# Verifying at login
if bcrypt.checkpw(entered_password.encode(), user.password_hash):
    # correct password
    ...
```
- **Salt**: a random value mixed into each password before hashing, unique per user — ensures two users with the identical password get completely different hashes, defeating precomputed rainbow table attacks. bcrypt handles this automatically.
- **Work factor / cost**: bcrypt/Argon2 let you tune how computationally expensive the hash is — deliberately slow enough to make brute-forcing impractical, but still fast enough for legitimate login attempts (typically well under a second).

## Additional real practices
- **Rate limit login attempts** (`12-Scalability-and-Performance/Rate-Limiting.md`) — prevents brute-force credential-stuffing attacks against your login endpoint.
- **Never reveal whether it was the username or password that was wrong**: `"Invalid username or password"` (generic) instead of `"Password incorrect"` (which confirms the username exists) — a small but real practice preventing account enumeration.
- **Enforce reasonable password policies** without going overboard: modern guidance (NIST) favors LENGTH over complex character requirements — encourage long passphrases rather than forcing awkward `P@ssw0rd1!` patterns that users just write down anyway.
- **Support MFA** as an additional, genuinely strong layer beyond password strength alone.

## Practical example
A real breach scenario: Company A stores plaintext passwords, gets breached — every user's actual password is exposed, and since many reused that password elsewhere, attackers gain access to those OTHER accounts too ("credential stuffing"). Company B, using bcrypt, gets breached the same way — but attackers only get computationally expensive HASHES, not usable passwords, buying time for users to change their passwords elsewhere before real damage occurs. Same breach, dramatically different real-world consequences, entirely due to this one architectural decision.
