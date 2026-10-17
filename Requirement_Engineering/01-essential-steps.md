# 01 - Essential Steps of Requirement Engineering

## Step 1: Understand the Problem & Vision
**Goal:** Know why this project exists.

Ask:
- What problem are we solving?
- Who has this problem?
- What does success look like? (measurable)
- What is the budget, deadline, and team size?

**Output:** One-page *Project Vision* (problem, goal, success metrics).

> Example: "Small shops lose sales because they track stock on paper. Goal: reduce stock-outs by 30% in 6 months."

---

## Step 2: Identify Stakeholders
**Goal:** Find everyone who is affected by or can influence the project.

| Type | Examples |
|------|----------|
| End users | Customers, staff, admins |
| Buyers/Sponsors | Client, management |
| Technical | Developers, DevOps, security |
| Others | Legal, support, regulators |

**Output:** Stakeholder list with name, role, and what they care about.

---

## Step 3: Elicit (Collect) Requirements
**Goal:** Gather needs from stakeholders.

Techniques (use a mix):
- **Interviews** - one-to-one, best for details
- **Workshops** - group discussion, best for alignment
- **Questionnaires/Surveys** - many users, quick
- **Observation** - watch users do their real work
- **Document analysis** - old systems, reports, forms
- **Prototypes / wireframes** - show, don't just tell
- **Competitor analysis** - see what others do

**Output:** Raw list of needs, notes, and sketches.

---

## Step 4: Analyze & Prioritize
**Goal:** Clean up the raw list.

- Remove duplicates and conflicts
- Split big items into small ones
- Check feasibility (time, cost, tech)
- Prioritize with **MoSCoW**:

| Level | Meaning |
|-------|---------|
| **Must have** | Project fails without it |
| **Should have** | Important, but workaround exists |
| **Could have** | Nice to have |
| **Won't have (now)** | Agreed to skip for this release |

**Output:** Prioritized requirement list + defined **MVP** (smallest useful version).

---

## Step 5: Specify (Write Down) Requirements
**Goal:** Make requirements clear, short, and testable.

Write three kinds:
1. **Functional** - what the system does
   *"The user can reset password via email."*
2. **Non-functional** - how well it does it
   *"Login page loads in under 2 seconds."*
3. **Constraints** - limits we must follow
   *"Must run on Android 10+ and comply with local data law."*

Choose a format: **User Stories** (agile) or **SRS document** (formal). See `03-templates.md`.

**Output:** Requirements document / backlog with unique IDs (e.g., `REQ-001`).

---

## Step 6: Validate & Agree
**Goal:** Confirm we understood correctly *before* building.

- Review with stakeholders (read it back to them)
- Show wireframes/prototype
- Check each requirement: clear? testable? consistent? possible?
- Get **written sign-off** (email approval is fine)

**Output:** Approved requirement **baseline**.

---

## Step 7: Manage Changes & Trace
**Goal:** Keep requirements under control during the project.

- Use a **change request process**: request -> impact check (time/cost) -> approve/reject -> update docs
- Keep a **traceability matrix**: Requirement -> Design -> Code -> Test
- Version every document (v1.0, v1.1...)
- Review requirements at the start of each sprint/phase

**Output:** Change log + traceability matrix.

---

## Quick Flow Recap

```
Vision -> Stakeholders -> Elicit -> Analyze/Prioritize
      -> Specify -> Validate/Sign-off -> Manage Changes
```
