# Day 26: Message Queues (RabbitMQ/Kafka Basics)

**Week 4 · Production Readiness**

## 🎯 Learning Objectives
- Understand asynchronous, decoupled communication
- Publish and consume messages
- Reason about at-least-once delivery and idempotency

## 📚 Topics
- Pub/sub vs point-to-point messaging
- Producers, consumers, topics/queues
- At-least-once delivery & idempotent consumers
- When to use async messaging vs sync HTTP calls

## 🟣 C# / .NET Focus
- Use `RabbitMQ.Client` or MassTransit for RabbitMQ.
- Alternatively use `Confluent.Kafka` for Kafka.

## 🟠 Java Focus
- Use `spring-boot-starter-amqp` for RabbitMQ.
- Alternatively use `spring-kafka` for Kafka.

## 🛠️ Hands-On Task
Introduce an `OrderPlaced` event: when a book purchase completes, publish an event to a queue/topic. Build a separate consumer (can be in the same app for simplicity) that listens and sends a 'confirmation' (log/print) asynchronously. Make the consumer idempotent (safe to process the same message twice).

## 🚀 Stretch Goal
Add a dead-letter queue for messages that fail processing after 3 retries.

## 🔗 Resources
- https://www.rabbitmq.com/tutorials
- https://kafka.apache.org/quickstart

## ✅ Checklist
- [ ] Read/reviewed the topics above
- [ ] Completed the hands-on task in **C#**
- [ ] Completed the hands-on task in **Java**
- [ ] Committed code with a clear commit message
- [ ] (Optional) Attempted the stretch goal
- [ ] Wrote 2-3 sentences in your own words summarizing what you learned

---
[⬅ Previous Day](day-25.md) | [🏠 Home](README.md) | [Next Day ➡](day-27.md)
