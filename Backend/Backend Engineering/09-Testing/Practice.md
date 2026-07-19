# Practice — Testing

1. Write unit tests (using pytest, Jest, or your framework of choice) for a simple pricing/discount calculation function, covering the happy path AND at least 3 edge cases (zero, negative, boundary values).
2. Set up a test database (or an in-memory/ephemeral one) and write an integration test for a real API endpoint that creates a record and verifies it was actually persisted.
3. Practice TDD directly: pick a small, well-defined function you haven't written yet (e.g., "validate an email format"), write the test FIRST, watch it fail, then implement just enough to pass, then refactor.
4. Mock an external dependency (simulate a slow/paid third-party API call) in a test, and verify your code calls it with the correct arguments without actually making a real network call.
5. Deliberately write a "flaky" test (one that depends on the current real time, unmocked) and observe/explain why it's unreliable — then fix it using a time-mocking library.
6. Run a code coverage report on your test suite (`pytest --cov` or equivalent) — identify one meaningfully untested code path and add a test for it.

✅ Done? Move to `10-DevOps-and-Deployment`.
