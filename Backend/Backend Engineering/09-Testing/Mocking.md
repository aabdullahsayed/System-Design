# Mocking

## What is it?
Replacing a real dependency (a database, an external API, the current time) with a controlled, fake stand-in during a test — letting you test your code's logic in isolation, without depending on real, slow, or unpredictable external systems.

## Why care?
Without mocking, testing code that calls a third-party API would require that API to actually be available (and cost money, and be slow, and be unpredictable) during every single test run — mocking makes tests fast, reliable, and independent of external systems.

## A motivating example
```python
def send_welcome_email(user):
    # calls a real, slow, external email service
    email_service.send(user.email, "Welcome!")
    return True

# Testing this WITHOUT mocking would actually send a real email every test run — bad!
```

## Mocking with `unittest.mock` (Python)
```python
from unittest.mock import Mock, patch

def test_send_welcome_email():
    mock_email_service = Mock()
    result = send_welcome_email(user, email_service=mock_email_service)

    mock_email_service.send.assert_called_once_with(user.email, "Welcome!")   # verify it WAS called correctly
    assert result == True
```
```python
# patch() temporarily replaces a real dependency for the duration of the test
@patch('myapp.email_service.send')
def test_send_welcome_email(mock_send):
    send_welcome_email(user)
    mock_send.assert_called_once()
```

## Mocks vs Stubs vs Fakes (useful, precise vocabulary)
- **Stub**: a simple fake that returns a fixed, canned response, without verifying HOW it was called.
- **Mock**: like a stub, but also lets you ASSERT it was called correctly (right arguments, right number of times) — used above.
- **Fake**: a lightweight, working implementation of a dependency, simpler than the real thing (e.g., an in-memory database instead of a real one) — often more realistic than a mock, at the cost of more setup.

## When to mock, and when NOT to
```
Mock external dependencies:        third-party APIs, email/SMS services, payment gateways,
                                     the current time/date (for deterministic tests), randomness

Prefer a REAL test database         a real (test) database catches real SQL bugs that
over mocking the database:           a mocked database call structurally cannot catch
                                       (see Integration-Testing.md)
```
**A common, real mistake**: over-mocking to the point that tests no longer verify anything meaningful about actual behavior — a test that mocks EVERYTHING might pass even if the real integration is completely broken. Mock the genuinely external/slow/unpredictable boundaries; use real (test) implementations for your own database and internal logic wherever practical.

## Mocking time (a very common, genuinely useful pattern)
```python
from freezegun import freeze_time

@freeze_time("2026-01-01")
def test_subscription_expires_correctly():
    subscription = create_subscription(duration_days=30)
    assert subscription.expires_at == datetime(2026, 1, 31)
```
Without controlling the current time, any test involving dates/expiration becomes non-deterministic (passes today, might fail differently a month from now) — a real, common source of flaky tests without this technique.

## In backend engineering
- **Dependency injection** (`01-Programming-Fundamentals/OOP-Principles.md`) is what makes mocking clean and easy — if your code depends on an INTERFACE (passed in) rather than directly instantiating/importing a concrete external service, swapping in a mock for tests becomes trivial.
- **Flaky tests** (tests that sometimes pass, sometimes fail, with no code changes) are very often caused by insufficiently mocked non-determinism — real network calls, unmocked current time, or reliance on test execution order — genuinely one of the most common, annoying real-world testing problems, and mocking correctly is the primary fix.
