# Integration Testing

## What is it?
Testing how multiple components work TOGETHER — e.g., your API endpoint actually talking to a real (test) database, verifying the full request-to-response flow, not just an isolated function.

## Why care?
Unit tests verify individual pieces work correctly in isolation; integration tests verify those pieces actually work correctly WHEN COMBINED — a very common, real category of bug is when individually-correct units fail to integrate correctly (mismatched assumptions, wrong SQL, serialization issues).

## What integration tests typically cover
- API endpoint → business logic → real (test) database → response.
- Message queue producer → consumer, verifying an event actually triggers the expected side effect.
- Integration with an external service (often using a sandboxed/test version of that service, or a realistic mock — see `Mocking.md`).

## Example: testing a real API endpoint against a test database
```python
def test_create_user_endpoint(test_client, test_db):
    response = test_client.post("/api/users", json={
        "name": "Alice", "email": "alice@example.com"
    })

    assert response.status_code == 201
    assert response.json()["name"] == "Alice"

    # Verify it was ACTUALLY persisted, not just that the response looked right
    user_in_db = test_db.query("SELECT * FROM users WHERE email = %s", "alice@example.com")
    assert user_in_db is not None
```

## Using a real (but isolated) test database
- **Never test against your production database.**
- Common approaches: a dedicated test database, reset between test runs (or wrapped in a transaction that's rolled back after each test); or increasingly, **ephemeral containers** (spin up a real, fresh PostgreSQL/MySQL instance via Docker specifically for the test run, then discard it) — gives you a genuinely real database without the risk/mess of a persistent shared test database.
```python
# Common pattern: wrap each test in a transaction, roll back after
@pytest.fixture
def test_db():
    connection = db.connect()
    transaction = connection.begin()
    yield connection
    transaction.rollback()   # undo everything the test did — keeps tests isolated from each other
```

## Testing pyramid — how unit and integration tests relate
```
        /\
       /E2E\        few — slow, expensive, test entire user flows
      /------\
     /Integr. \     some — test component interactions
    /----------\
   / Unit Tests \   many — fast, cheap, test individual pieces
  /--------------\
```
The general guidance: have MANY fast unit tests covering detailed logic/edge cases, FEWER integration tests covering the most important component interactions, and only a SMALL number of full end-to-end tests covering critical user journeys — because integration/E2E tests are inherently slower and more brittle (more moving parts that can break for unrelated reasons).

## In backend engineering
- **CI pipelines** (`10-DevOps-and-Deployment/CI-CD.md`) typically run the full unit test suite on every commit (fast feedback), and integration tests either on every commit too (if fast enough) or before merging/deploying (if they're slower, e.g., requiring container startup time).
- **Integration tests catch a real, common class of bugs that unit tests structurally CANNOT**: e.g., a SQL query with a typo in a column name will often still pass a unit test (if the database call itself is mocked out) but fail immediately against a real test database.
