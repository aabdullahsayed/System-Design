# Unit Testing

## What is it?
Testing individual units of code (typically a single function or class) in isolation, verifying they behave correctly for a range of inputs — the foundation of any automated testing strategy.

## Why care?
Unit tests catch regressions automatically, let you refactor with confidence, and serve as executable documentation of what code is actually supposed to do. Writing them is a core, daily, evaluated skill in professional backend engineering.

## Anatomy of a good unit test (AAA pattern)
```python
def test_calculate_discount():
    # Arrange — set up inputs
    price = 100
    discount_rate = 0.2

    # Act — call the code under test
    result = calculate_discount(price, discount_rate)

    # Assert — verify the expected outcome
    assert result == 80
```

## What makes a GOOD unit test
- **Fast**: unit tests should run in milliseconds — no real database, network, or file I/O (that's integration testing's job, see `Integration-Testing.md`).
- **Isolated**: doesn't depend on other tests' state or execution order.
- **Deterministic**: same result every single run — no reliance on current time, randomness, or external state without controlling for it.
- **Tests ONE thing**: a failing test name should immediately tell you what broke, without needing to read the test body.

## Testing edge cases, not just the happy path
```python
def test_calculate_discount_edge_cases():
    assert calculate_discount(100, 0) == 100        # no discount
    assert calculate_discount(100, 1) == 0             # 100% discount
    assert calculate_discount(0, 0.5) == 0                # zero price

    with pytest.raises(ValueError):
        calculate_discount(100, 1.5)                        # invalid discount > 100%
```
A test suite that only checks the "obvious happy path" gives false confidence — the real value of tests often comes from deliberately checking boundary conditions, invalid inputs, and error handling.

## Test frameworks by language
```
Python:      pytest, unittest
JavaScript:  Jest, Mocha/Chai, Vitest
Java:        JUnit
C#:          xUnit, NUnit
Go:          built-in testing package
```

## Code coverage — a useful signal, not a target to game
```bash
pytest --cov=myapp tests/
```
Coverage tells you WHAT PERCENTAGE of your code is executed by tests — useful for spotting completely untested code paths, but **100% coverage does NOT mean bug-free code** (a test can execute a line without actually asserting anything meaningful about its behavior) — treat coverage as a helpful signal, not the actual goal itself.

## In backend engineering
- **Tests as a safety net for refactoring**: a well-tested function can be confidently rewritten/optimized, because the test suite will immediately catch any behavioral regression — without tests, refactoring is a much higher-risk activity.
- **Writing testable code**: functions with clear inputs/outputs and minimal hidden dependencies (global state, direct database calls buried deep inside business logic) are naturally easier to unit test — this is a real, practical reason the clean code and dependency injection principles from `01-Programming-Fundamentals` matter in daily engineering work.
