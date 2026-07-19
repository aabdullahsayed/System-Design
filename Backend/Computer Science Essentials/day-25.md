# Day 25: Security Fundamentals

**Week 4 · Systems, Design & Practice**

## 🎯 Learning Objectives
- Explain core cryptography concepts (hashing vs encryption)
- Recognize and prevent common web vulnerabilities
- Reason about authentication vs authorization

## 📚 Topics
- Hashing vs symmetric vs asymmetric encryption
- Authentication vs authorization
- OWASP Top 10 essentials: SQL injection, XSS, CSRF
- Secure password storage

## 💡 Core Ideas
- Hashing is one-way (for passwords, integrity checks); encryption is two-way (for confidentiality, needs a key to reverse) — mixing these up is a classic mistake.
- Authentication answers 'who are you?'; authorization answers 'what are you allowed to do?' — a system can authenticate correctly and still authorize incorrectly.
- Most classic web vulnerabilities (SQL injection, XSS) come from mixing untrusted user input with executable code/queries without proper escaping or parameterization.

## 🛠️ Practice Exercises
- [ ] Write (pseudocode or real code) a vulnerable SQL query built via string concatenation, then rewrite it using parameterized queries and explain why the fix works.
- [ ] Explain the difference between storing a password with plain hashing (e.g., SHA-256) vs a proper password hash (e.g., bcrypt/argon2) — what's missing from plain hashing?
- [ ] Read about XSS and CSRF, and write one sentence each on how to prevent them.

## 🎤 Interview-Style Questions
Try answering these out loud, without notes, before checking any reference:

- Why is SHA-256 alone a bad way to store passwords, even though it's a strong hash function?
- What's the difference between authentication and authorization, with a concrete example where a system gets one right and the other wrong?
- How does a parameterized query prevent SQL injection where string concatenation doesn't?

## 🔗 Resources
- https://owasp.org/www-project-top-ten/
- https://www.cloudflare.com/learning/security/

## ✅ Daily Checklist
- [ ] Read/reviewed the topics and core ideas above
- [ ] Completed all practice exercises
- [ ] Answered the interview-style questions out loud or in writing
- [ ] Committed any code to your repo
- [ ] Wrote 2-3 sentences in your own words summarizing the day's key insight

---
[⬅ Previous Day](day-24.md) | [🏠 Home](README.md) | [Next Day ➡](day-26.md)
