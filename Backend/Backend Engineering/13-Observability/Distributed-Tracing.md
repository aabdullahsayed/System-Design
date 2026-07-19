# Distributed Tracing

## What is it?
A technique for tracking a single request's full journey as it flows through MULTIPLE services in a distributed/microservices system — visualizing exactly which services were called, in what order, and how long each step took.

## Why care?
In a microservices architecture, a single user-facing request might touch 10+ internal services — when it's slow or fails, logs and metrics from individual services alone don't easily reveal WHERE in that chain the problem occurred. Distributed tracing directly solves this, and is essentially mandatory tooling once you have more than a couple of interacting services.

## Core concepts
- **Trace**: the complete record of one request's journey across all services it touched.
- **Span**: one individual unit of work within a trace (e.g., "Orders Service: validate order" or "Payment Service: charge card") — a trace is made up of many nested/sequential spans.
- **Trace ID**: a unique identifier generated at the very start of a request (often at the API Gateway) and propagated through EVERY downstream service call (usually via an HTTP header) — this is what stitches together spans from different services into one coherent trace.

```
Trace ID: abc-123
├── Span: API Gateway (2ms)
│   └── Span: Orders Service (450ms)
│       ├── Span: Database query (30ms)
│       ├── Span: Payment Service call (380ms)  ← the bottleneck!
│       │   └── Span: External payment provider API (370ms)
│       └── Span: Inventory Service call (35ms)
```
A trace visualization like this instantly reveals that the External payment provider API call is the dominant cost in an otherwise-fast request — exactly the kind of insight that's very hard to get from separate services' individual logs/metrics alone.

## Propagating trace context
Every service must forward the trace ID (and span parent info) to any downstream service it calls, typically via a standard HTTP header (`traceparent`, following the W3C Trace Context standard) — most modern tracing libraries handle this propagation automatically once instrumented, but it's important to understand it's happening.

## Common tools
- **Jaeger**: a popular open-source distributed tracing system (originally built at Uber).
- **Zipkin**: another well-established open-source option (originally built at Twitter).
- **OpenTelemetry**: the modern, vendor-neutral standard for instrumenting applications to emit traces (and metrics/logs) — increasingly the recommended way to instrument new code, since it can export to Jaeger, Zipkin, Datadog, or other backends without changing your instrumentation code.

## Practical example
A "why is checkout slow sometimes" investigation, without tracing, requires manually cross-referencing timestamps across 5 separate services' logs — tedious and error-prone. With distributed tracing, you search for a specific slow trace ID (or filter for the slowest traces overall) and immediately SEE, visually, which specific span/service is responsible — turning a multi-hour cross-team investigation into a few-minutes lookup.
