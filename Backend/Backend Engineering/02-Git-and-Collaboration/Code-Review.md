# Code Review

## What is it?
The process of having another engineer review your code changes (typically via a Pull Request/Merge Request) before they're merged into the shared codebase.

## Why care?
Code review is one of the most consistent, highest-leverage quality practices in software engineering — it catches bugs before production, spreads knowledge across the team, and is where a huge amount of real engineering mentorship/learning happens on the job.

## As the author — writing a reviewable PR
- **Keep PRs small and focused** — a 2,000-line PR is nearly impossible to review carefully; a 200-line PR focused on one thing gets thorough, fast feedback.
- **Write a clear PR description**: what changed, why, and how to test it — don't make the reviewer reverse-engineer your intent from the diff alone.
- **Self-review first**: read your own diff before requesting review — you'll often catch your own mistakes (leftover debug code, typos) before anyone else needs to point them out.
- **Include tests** for new behavior — reviewers should be able to trust that the change is verified, not just visually plausible.

## As the reviewer — giving useful feedback
- **Review for correctness AND readability** — does it work, AND will the next person understand it?
- **Distinguish blocking issues from suggestions**: a common convention is prefixing optional comments with "nit:" (nitpick) so the author knows what's a must-fix vs a nice-to-have.
- **Ask questions instead of asserting** when unsure: "Is there a reason this doesn't handle the empty-list case?" invites explanation, rather than "This is wrong" which can feel confrontational and might miss context you don't have.
- **Focus on the code, not the person** — feedback should be about the change, phrased constructively, never personal.

## What good reviewers actually check
```
Correctness: does the logic actually do what it claims?
Edge cases: null/empty inputs, error handling, race conditions
Tests: are the new code paths actually covered?
Security: any injection risks, exposed secrets, missing auth checks?
Readability: will someone unfamiliar with this PR understand it in 6 months?
Consistency: does it follow the codebase's existing patterns/conventions?
```

## Practical example
A PR adding a new API endpoint gets review comments: "This endpoint doesn't check if the user is authorized to access this resource — can you add that check?" (a **blocking** security issue) and "nit: consider renaming `data` to `userProfile` for clarity" (a **non-blocking** suggestion) — the author addresses the security issue before merge, and can choose to apply or skip the naming nit at their discretion. This exact blocking/non-blocking distinction is what keeps code review fast without sacrificing quality on what actually matters.
