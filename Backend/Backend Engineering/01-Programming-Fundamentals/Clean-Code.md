# Clean Code Principles

## What is it?
Practices for writing code that's easy for OTHER humans (including future-you) to read, understand, and safely modify — not just code that technically works.

## Why care?
Code is read far more often than it's written. In a real team/production environment, unclear code directly costs time (slower code review, more bugs, harder onboarding) — this is a genuinely evaluated skill in code reviews and technical interviews.

## Naming
```python
# Bad
def calc(x, y, z):
    return x * y * (1 - z)

# Good — names explain intent without needing a comment
def calculate_discounted_price(price, quantity, discount_rate):
    return price * quantity * (1 - discount_rate)
```
Rule of thumb: a good name answers "what does this do/represent" without requiring the reader to read the implementation.

## Functions should do ONE thing
```python
# Bad — does validation, calculation, AND persistence all in one function
def process_order(order):
    if order.total <= 0: raise ValueError("Invalid")
    tax = order.total * 0.08
    order.total += tax
    db.save(order)
    send_email(order.customer_email)

# Good — each function has a single, clear responsibility
def validate_order(order): ...
def apply_tax(order): ...
def save_order(order): ...
def notify_customer(order): ...

def process_order(order):
    validate_order(order)
    apply_tax(order)
    save_order(order)
    notify_customer(order)
```
This is the "Single Responsibility Principle" from SOLID, applied at the function level — and it directly makes each piece independently testable (`09-Testing`).

## Avoid deep nesting — use early returns / guard clauses
```python
# Bad — deeply nested, hard to follow
def get_discount(user):
    if user is not None:
        if user.is_active:
            if user.membership_level == "gold":
                return 0.2
    return 0

# Good — guard clauses handle edge cases upfront, main logic stays flat
def get_discount(user):
    if user is None or not user.is_active:
        return 0
    if user.membership_level == "gold":
        return 0.2
    return 0
```

## Comments explain WHY, not WHAT
```python
# Bad — comment just restates the obvious code
# increment counter by 1
counter += 1

# Good — explains non-obvious reasoning
# Retry once before failing — third-party API is known to have transient timeouts
retry_count = 1
```
If code needs a comment to explain WHAT it does, that's often a sign the code itself should be clearer (better naming, smaller functions) — comments are best reserved for explaining WHY a non-obvious decision was made.

## Don't Repeat Yourself (DRY) — but don't over-apply it either
Duplicated logic scattered across a codebase means a bug fix has to be applied in multiple places (and is easy to miss one). But over-applying DRY too early (abstracting code that merely LOOKS similar but represents different concerns) creates fragile, confusing shared code — a genuinely nuanced, experience-based judgment call.

## In backend engineering
- **Code review culture**: clean code principles are what most real code review comments are actually about (naming, function size, nesting) — internalizing these reduces review cycles and friction with your team.
- **Onboarding speed**: a new engineer's ability to safely make their first change to a codebase depends directly on how much of this discipline was applied by the people before them.
- **Technical debt**: unclear code is a major hidden cost — it slows down every future change to that code, compounding over the life of a project.
