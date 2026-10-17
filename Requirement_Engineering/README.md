# Requirement Engineering Guide

A simple guide to figure out **what we actually need to build** before writing code.

## Why this matters
- Most project failures come from unclear or changing requirements, not bad code.
- Fixing a mistake in requirements costs **1x**. Fixing it after release costs **10-100x**.
- Goal: everyone (client, team, testers) understands the same thing.

## Folder Contents

| File | Purpose |
|------|---------|
| `README.md` | Overview (this file) |
| `01-essential-steps.md` | The 7 steps of requirement engineering |
| `02-must-keep-in-mind.md` | Rules, pitfalls, and good habits |
| `03-templates.md` | Copy-paste templates (user story, SRS, etc.) |
| `04-quick-checklist.md` | One-page checklist before you start coding |

## The Big Picture

```
Understand the problem -> Collect needs -> Analyze & prioritize
        -> Write them down -> Validate -> Agree (baseline) -> Manage changes
```

## 30-Second Summary
1. **Know the WHY** - what problem are we solving?
2. **Know the WHO** - who uses it, who pays, who is affected?
3. **Know the WHAT** - features (functional) + qualities (non-functional).
4. **Prioritize** - what is a must, what can wait?
5. **Write it clearly** - simple, testable, no guessing.
6. **Get sign-off** - everyone agrees.
7. **Control change** - changes will come; manage them, don't fear them.
