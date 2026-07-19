# Background Jobs

## What is it?
Discrete units of work processed OUTSIDE the request-response cycle — either triggered by an event (like a message queue consumer) or run on a SCHEDULE (like a nightly cleanup task), typically using a dedicated job processing library/framework.

## Why care?
Nearly every real backend system has recurring, deferred, or scheduled work: sending digest emails, cleaning up expired records, generating reports, syncing data with a third-party system — background job systems are the standard, practical tool for this.

## Common background job frameworks
```
Python:      Celery, RQ (Redis Queue), Dramatiq
Node.js:     Bull/BullMQ (Redis-backed), Agenda
Ruby:        Sidekiq, Resque
Java:        Quartz Scheduler, Spring Batch
```
Most of these are built on top of a message queue or Redis under the hood (`Message-Queues.md`, `06-Caching/Redis.md`) — providing a higher-level, more convenient API specifically for the "background job" use case (retries, scheduling, monitoring) on top of that underlying infrastructure.

## Enqueueing and processing a job (example with Celery/Python)
```python
# tasks.py
@celery_app.task
def send_report_email(user_id, report_data):
    report = generate_pdf(report_data)
    email_service.send(user_id, attachment=report)

# In your API endpoint — hand off and return immediately
@app.post("/reports/generate")
def generate_report(request):
    send_report_email.delay(request.user_id, request.report_params)  # enqueue, don't wait
    return {"status": "Report generation started"}
```

## Retries and failure handling (a critical, non-optional design consideration)
```python
@celery_app.task(bind=True, max_retries=3, default_retry_delay=60)
def call_flaky_third_party_api(self, data):
    try:
        return external_api.call(data)
    except TimeoutError as exc:
        raise self.retry(exc=exc)   # retry after 60s, up to 3 times
```
Real-world background jobs WILL occasionally fail (network blips, third-party outages) — a job system without retry logic silently drops work; robust retry logic (often with exponential backoff — increasing delay between attempts) is standard, expected practice.

## Scheduled/recurring jobs (cron-like)
```python
# Celery Beat schedule example
CELERYBEAT_SCHEDULE = {
    'cleanup-expired-sessions': {
        'task': 'tasks.cleanup_expired_sessions',
        'schedule': crontab(hour=3, minute=0),   # every day at 3 AM
    },
}
```
Common use cases: nightly data aggregation/reporting, expiring old records, sending scheduled digest emails, periodic health checks/reconciliation jobs.

## Idempotency matters here too (see `14-Advanced-Topics/Idempotency.md`)
Because retries can cause a job to run more than once, background job logic should be designed to be safely re-runnable — e.g., "mark order as shipped" should check current state first, not blindly re-apply an action that assumes it's the first execution.

## Monitoring background jobs
A job silently failing (and never being retried/alerted on) is a very real, common production issue — proper job systems provide dashboards/metrics on queue depth (is work backing up faster than it's processed?), failure rates, and processing times, directly connecting to `13-Observability`.

## In backend engineering
Background jobs and message queues are closely related but distinct concepts: a message queue is the underlying transport/durability mechanism; a background job framework is typically a higher-level, more ergonomic layer built for the specific common pattern of "run this function later, with retries and scheduling," often (but not always) built on top of a message queue or Redis.
