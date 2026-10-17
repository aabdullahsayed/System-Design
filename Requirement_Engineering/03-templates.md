# 03 - Templates (Copy & Use)

## 1. Project Vision (1 page)

```
Project Name:
Problem:
Target Users:
Goal / Success Metrics (measurable):
Key Features (top 5):
Out of Scope:
Constraints (budget / deadline / tech):
Assumptions & Risks:
```

---

## 2. Stakeholder Table

| Name | Role | Interest / Need | Influence (H/M/L) | Contact |
|------|------|-----------------|-------------------|---------|
|      |      |                 |                   |         |

---

## 3. User Story

```
ID: US-001
As a <type of user>,
I want <goal / action>,
So that <benefit / reason>.

Priority: Must / Should / Could / Won't
Acceptance Criteria:
  Given <starting situation>
  When <user does something>
  Then <expected result>
```

**Example**
```
ID: US-001
As a shop owner,
I want to get an alert when stock is below 10 items,
So that I can reorder before running out.

Priority: Must
Acceptance Criteria:
  Given an item has 10 or fewer units left
  When the stock level updates
  Then the owner receives an app notification within 1 minute
```

---

## 4. Requirement Table (Simple SRS Style)

| ID | Type | Requirement | Priority | Source | Status |
|----|------|-------------|----------|--------|--------|
| REQ-001 | Functional | User can register with email and password | Must | Client meeting | Approved |
| REQ-002 | Non-functional | Pages load within 2 seconds | Should | Tech lead | Draft |
| REQ-003 | Constraint | Must support Android 10+ | Must | Client | Approved |

---

## 5. Mini SRS Outline

```
1. Introduction
   1.1 Purpose
   1.2 Scope
   1.3 Definitions / Glossary
2. Overall Description
   2.1 Product perspective
   2.2 User types
   2.3 Assumptions & dependencies
3. Functional Requirements
4. Non-Functional Requirements
   (performance, security, usability, reliability, compatibility)
5. External Interfaces (UI, APIs, hardware)
6. Constraints
7. Appendix (diagrams, wireframes, references)
```

---

## 6. Change Request Form

```
CR ID:
Date:
Requested by:
Affected requirement(s):
Description of change:
Reason:
Impact (time / cost / risk):
Decision: Approved / Rejected / Postponed
Approved by:
```

---

## 7. Traceability Matrix

| Req ID | Design Doc | Code Module | Test Case | Status |
|--------|-----------|-------------|-----------|--------|
| REQ-001 | UI-Login | auth.js | TC-001 | Done |

---

## 8. Interview Question Starter List

- Describe your current process step by step.
- What is the biggest problem in it today?
- What would an ideal solution do for you?
- What must the system never do?
- Which tasks do you do most often?
- Who else should I talk to?
- What does "success" look like in 6 months?

---

## 9. Glossary

| Term | Meaning |
|------|---------|
|      |         |
