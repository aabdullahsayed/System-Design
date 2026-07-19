# Test-Driven Development (TDD)

## What is it?
A development workflow where you write the TEST first (before the implementation), watch it fail, then write just enough code to make it pass, then refactor — the "Red-Green-Refactor" cycle.

## Why care?
TDD is a widely-discussed practice (with genuine advocates and genuine skeptics) that, when applied well, produces naturally well-tested, more deliberately-designed code — worth understanding the workflow and its actual trade-offs, even if you don't practice it strictly for every single piece of code.

## The Red-Green-Refactor cycle
```
1. RED:      Write a test for behavior that doesn't exist yet. Run it — it FAILS (there's no implementation).
2. GREEN:    Write the SIMPLEST possible code that makes the test pass — don't over-engineer yet.
3. REFACTOR: Now that you have a passing test as a safety net, clean up the implementation
             (better names, remove duplication) — re-run tests to confirm you haven't broken anything.
4. Repeat for the next small piece of behavior.
```

## Worked example
```python
# 1. RED — write the test first, it fails because calculate_shipping doesn't exist yet
def test_free_shipping_over_50():
    assert calculate_shipping(order_total=60) == 0

# 2. GREEN — simplest implementation that passes
def calculate_shipping(order_total):
    if order_total >= 50:
        return 0
    return 5.99

# 3. Add the NEXT test case, driving the next bit of behavior
def test_standard_shipping_under_50():
    assert calculate_shipping(order_total=30) == 5.99

# implementation above already handles this — test passes immediately, no change needed
# (this is a real, common TDD experience — sometimes the next test just passes already)
```

## Claimed benefits
- **Forces you to think about the interface/API before the implementation** — often results in cleaner, more usable function signatures.
- **Guarantees test coverage** for the behavior you actually built (since the test came first, by definition).
- **Fast feedback loop** while actively developing — you know immediately when something breaks.
- **Naturally avoids over-engineering** ("simplest code that passes the test") — discourages building speculative flexibility you don't actually need yet.

## Real, honest trade-offs and criticisms
- **Not always practical for exploratory/uncertain design work** — when you genuinely don't yet know what the right interface/approach is, writing tests first can feel premature; some engineers prototype first, then backfill tests once the design settles.
- **Can lead to over-mocked, brittle tests** if applied too rigidly at too low a level, testing implementation details rather than actual behavior — tests that break on every minor refactor even when behavior is unchanged are a real, common TDD anti-pattern.
- **UI/integration-heavy work** is often less naturally suited to strict TDD than pure business logic.

## A pragmatic take
Most experienced engineers use TDD selectively — strictly for complex business logic with clear input/output expectations (where it shines), more loosely (tests written soon after, not strictly before) for exploratory or UI-heavy work. The genuinely important, transferable habit — regardless of strict test-FIRST discipline — is: **write the tests, and think about testability as you design**, not "as an afterthought once everything already works."

## In backend engineering
Backend business logic (pricing calculations, validation rules, state transitions) is often an excellent fit for TDD's tight feedback loop — it has clear inputs/outputs and is exactly the kind of logic where a comprehensive, fast-running unit test suite pays for itself many times over as the codebase evolves.
