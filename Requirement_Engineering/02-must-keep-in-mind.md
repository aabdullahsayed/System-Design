# 02 - Must Keep in Mind

## Golden Rules

1. **Problem first, solution later.** Don't jump to "we need an app" - ask what problem the app solves.
2. **Talk to real users.** Assumptions are the #1 cause of rework.
3. **Every requirement must be testable.** If you can't test it, it's not a requirement yet.
4. **Write in plain language.** Anyone (client, tester, new developer) should understand it.
5. **One requirement = one idea.** Avoid "and/or" sentences.
6. **Prioritize ruthlessly.** You can't build everything at once. Define an MVP.
7. **Get it in writing.** Verbal agreements get forgotten.
8. **Expect change.** Plan a change process instead of pretending change won't happen.
9. **Don't forget non-functional needs** (speed, security, usability, reliability).
10. **Involve the whole team early** - developers, testers, designers catch problems BEFORE they get built.

---

## Good vs Bad Requirements

| Bad (vague) | Good (clear & testable) |
|-------------|-------------------------|
| "The system should be fast." | "Search results appear within 2 seconds for 95% of requests." |
| "User-friendly interface." | "A new user can place an order in under 3 minutes without training." |
| "Secure login." | "Passwords are hashed; account locks after 5 failed attempts." |
| "Support many users." | "Supports 5,000 concurrent users." |
| "Easy reports." | "Admin can export monthly sales report as PDF and Excel." |

---

## Qualities of a Good Requirement (check each one)

- **Clear** - only one meaning
- **Complete** - nothing missing
- **Consistent** - doesn't contradict others
- **Testable** - pass/fail can be checked
- **Feasible** - possible within time, cost, tech
- **Necessary** - solves a real need
- **Traceable** - has an ID and owner
- **Prioritized** - we know how important it is

---

## Common Pitfalls (and fixes)

| Pitfall | Fix |
|---------|-----|
| Unclear goals | Write a one-page vision and get it approved |
| Missing stakeholders | Do stakeholder mapping in week 1 |
| Scope creep | Use change requests; protect the MVP |
| Gold plating (extra unrequested features) | Build only what's approved |
| Ambiguous words ("fast", "easy", "etc.") | Replace with numbers and examples |
| Only asking "what do you want?" | Also ask "why?" and "what happens if it fails?" |
| Skipping non-functional requirements | Add a checklist: performance, security, usability, availability |
| No acceptance criteria | Add "Given / When / Then" to every story |
| Requirements only in one person's head | Document and share them |
| Never revisiting requirements | Review every sprint/phase |

---

## Questions to Always Ask

- **Why** do we need this?
- **Who** will use it? Who else is affected?
- **What** happens if we don't build it?
- **What** are the rules/edge cases? (errors, empty data, no internet)
- **How will we know** it's done? (acceptance criteria)
- **What is out of scope?**
- **What are the risks and assumptions?**
- **What systems must it connect to?** (payment, SMS, email, other APIs)

---

## Don't Forget These Non-Functional Areas

- **Performance** - speed, load capacity
- **Security** - login, roles, data protection, encryption
- **Usability** - ease of use, accessibility, language support
- **Reliability** - uptime, backups, error handling
- **Compatibility** - devices, browsers, OS versions
- **Scalability** - growth in users/data
- **Maintainability** - easy to update and fix
- **Legal / Compliance** - privacy laws, licenses

---

## Team Habits That Help

- Short, regular check-ins with the client
- Show early prototypes (wireframes cost less than rewritten code)
- Keep a **glossary** so everyone uses the same words
- Record decisions and the reason behind them
- Keep documents small and updated - not huge and forgotten
