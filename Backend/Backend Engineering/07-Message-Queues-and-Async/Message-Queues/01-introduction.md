# Message Queues: Introduction

## What is a message queue?
A message queue is a form of asynchronous service-to-service communication. A **producer** sends a message to a queue (held by a **broker**), and a **consumer** retrieves and processes it later. The producer and consumer don't need to be online at the same time or know about each other.

## Why use one?
- **Decoupling**: services evolve and deploy independently.
- **Asynchronous processing**: return to the user quickly, do slow work in the background.
- **Load leveling / buffering**: absorb traffic spikes; consumers process at their own pace.
- **Resilience**: if a consumer is down, messages wait instead of being lost.
- **Scalability**: add more consumers to increase throughput.
- **Ordering and workflow**: coordinate multi-step processes.

## Trade-offs
- Added operational complexity (broker to run and monitor).
- Eventual consistency instead of immediate results.
- Harder debugging and tracing across async boundaries.
- Need to handle duplicates, retries, and poison messages.

## Typical use cases
- Sending emails / notifications
- Image and video processing
- Order and payment pipelines
- Log and event ingestion
- Microservice integration
- Scheduled / delayed jobs
