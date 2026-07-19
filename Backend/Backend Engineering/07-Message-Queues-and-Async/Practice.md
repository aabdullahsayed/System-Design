# Practice — Message Queues & Async Processing

1. Identify 3 operations in a hypothetical e-commerce checkout flow that SHOULD be asynchronous (moved out of the request/response cycle) and explain why for each.
2. Set up a simple job queue locally (Celery + Redis, or BullMQ + Redis) and implement one background task: sending a "welcome email" (can just log to console instead of a real email for practice).
3. Modify your task to simulate an occasional failure (randomly raise an exception ~30% of the time), then add retry logic with a delay, and confirm it eventually succeeds across retries.
4. Implement a simple Pub/Sub pattern: one publisher emitting an "OrderCreated" event, and two independent subscribers reacting to it (e.g., one logs "inventory updated," another logs "email sent").
5. Set up a scheduled/recurring job (e.g., using Celery Beat or a simple cron-style scheduler) that runs every minute and logs a timestamp — confirm it fires reliably.
6. Explain in your own words: why does "at least once" message delivery mean your consumer logic needs to be idempotent, and give a concrete example of a NON-idempotent operation that would cause a real bug if processed twice.

✅ Done? Move to `08-System-Design-Basics`.
