# Message Queues (RabbitMQ, Kafka, SQS)

## What is it?
A message queue is an intermediary system that lets one part of your application (a "producer") hand off work/events to another part (a "consumer") asynchronously and reliably, decoupling them in time and often in deployment.

## Why care?
Message queues are the standard infrastructure enabling async processing (`Why-Async-Processing.md`) reliably at scale — understanding them is essential for any backend system beyond the simplest scale, and extremely common in real production architectures.

## The core pattern
```
Producer → [Queue] → Consumer(s)
```
- **Producer**: the part of your app that generates work/events (e.g., the signup endpoint).
- **Queue**: durable storage holding messages until they're processed — critically, this persists messages even if consumers are temporarily down, unlike a simple in-memory job list.
- **Consumer(s)**: one or more worker processes that pull messages off the queue and do the actual work.

## Why not just use a background thread instead?
A simple in-memory background thread/task loses everything if the server process crashes or restarts — a real message queue PERSISTS messages durably, survives restarts, and can distribute work across MULTIPLE consumer instances for scaling, none of which a simple in-process thread provides.

## RabbitMQ — traditional message broker
Good for: task queues, RPC-style request/reply patterns, complex routing (exchanges/routing keys let you flexibly direct messages to different queues based on rules).
```python
# Producer
channel.basic_publish(exchange='', routing_key='email_queue', body=json.dumps(email_data))

# Consumer
def callback(ch, method, properties, body):
    send_email(json.loads(body))
    ch.basic_ack(delivery_tag=method.delivery_tag)   # confirm processing succeeded

channel.basic_consume(queue='email_queue', on_message_callback=callback)
```

## Kafka — distributed event streaming platform
Different mental model from RabbitMQ: Kafka retains a durable, ordered LOG of events (not just a transient queue that empties once consumed), and multiple independent consumer groups can each read the same stream of events at their own pace. Built for very high throughput and event-driven architectures at scale.
```
Producer writes events to a "topic" (e.g., "order_created")
   → Kafka durably stores them, in order, partitioned across brokers
   → Multiple consumer groups can independently read/replay the same topic
      (e.g., one group updates a search index, another sends notifications,
       fully decoupled from each other)
```

## Cloud-managed queues (SQS, Google Pub/Sub, Azure Service Bus)
Fully-managed alternatives that remove the operational burden of running your own message broker — a very common practical choice, especially for teams without dedicated infrastructure expertise, trading some flexibility/cost for significantly reduced operational overhead.

## Choosing between them (a real architecture decision)
```
Simple task queue (send emails, process uploads):        RabbitMQ, SQS — simpler, well-suited to this pattern
High-throughput event streaming, multiple consumers,       Kafka — built specifically for this scale and
  need to replay/reprocess historical events:                the "durable event log" access pattern
```

## In backend engineering
- **At-least-once vs exactly-once delivery**: most message queues guarantee AT LEAST ONCE delivery (a message might be delivered more than once, e.g., after a consumer crash before acknowledging) — this is exactly why **idempotency** (`14-Advanced-Topics/Idempotency.md`) in your consumer logic is critical; processing the same message twice should be safe.
- **Dead Letter Queues (DLQs)**: messages that repeatedly fail processing get routed to a separate queue for manual inspection, instead of endlessly retrying or silently disappearing — a standard, important production safety pattern.
