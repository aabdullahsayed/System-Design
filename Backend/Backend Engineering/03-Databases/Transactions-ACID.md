# Transactions & ACID

## What is it?
A **transaction** groups multiple database operations into a single, all-or-nothing unit — either everything succeeds, or everything is rolled back, guaranteed even under concurrent access or system failure. **ACID** describes the guarantees a proper transactional database provides.

## Why care?
Real business operations (transferring money, placing an order that decrements inventory) involve MULTIPLE related writes that must all succeed or all fail together — without transactions, a crash halfway through could leave your data in a corrupted, inconsistent state (money deducted but never credited, an order placed but inventory not decremented).

## ACID, explained
- **Atomicity**: all operations in a transaction succeed, or none do — no partial completion.
- **Consistency**: a transaction takes the database from one valid state to another, never violating defined constraints (foreign keys, unique constraints, etc.).
- **Isolation**: concurrent transactions don't interfere with each other's intermediate state — each transaction behaves as if it's running alone (to varying degrees, depending on isolation level).
- **Durability**: once a transaction commits, it survives even a crash immediately afterward (written to durable storage, not just memory).

## Using transactions
```sql
BEGIN;

UPDATE accounts SET balance = balance - 100 WHERE id = 1;   -- debit
UPDATE accounts SET balance = balance + 100 WHERE id = 2;    -- credit

COMMIT;    -- both succeed together
-- or: ROLLBACK;  -- if something goes wrong, undo both
```
If the process crashes after the first UPDATE but before COMMIT, the ENTIRE transaction is rolled back automatically on restart — the first account's debit never actually persists, avoiding a corrupted, inconsistent state.

## Isolation levels (the "I" in ACID, with real trade-offs)
| Level | Prevents | Allows |
|---|---|---|
| **Read Uncommitted** | nothing | dirty reads (seeing another transaction's uncommitted changes) |
| **Read Committed** (common default) | dirty reads | non-repeatable reads (same query returns different results within one transaction) |
| **Repeatable Read** | dirty + non-repeatable reads | phantom reads (new rows appearing between reads) |
| **Serializable** (strictest) | all of the above | — behaves as if transactions ran one at a time |

**Trade-off**: stricter isolation = stronger correctness guarantees, but more locking/blocking = lower concurrency/throughput. Most applications use "Read Committed" (the common default) and only reach for stricter levels for specific operations that genuinely need it (e.g., financial transfers).

## In backend engineering
- **Application code pattern**: wrap related database writes in a transaction at the application/ORM level:
```python
# Pseudocode (e.g., using SQLAlchemy or Django ORM)
with db.transaction():
    account1.balance -= 100
    account2.balance += 100
    account1.save()
    account2.save()
# if any exception occurs, the whole block automatically rolls back
```
- **Deadlocks**: two transactions can end up waiting on each other's locked rows forever — directly connects to `Operating-System-For-SDE`'s `07-Deadlocks` concepts; the fix is the same: consistent lock/access ordering across your application code.
- **Distributed transactions** (across multiple services/databases in a microservices architecture) are significantly harder — classic ACID transactions don't span network boundaries cleanly, which is why patterns like the **Saga pattern** exist (`11-Microservices/Distributed-Transactions.md`).
