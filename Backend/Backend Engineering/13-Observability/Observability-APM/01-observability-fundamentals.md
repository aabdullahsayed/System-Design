# Software Observability: Fundamentals

## What is observability?
Observability is the ability to understand a system's **internal state from the data it emits** (telemetry), so you can answer questions you didn't anticipate in advance. It matters most in distributed systems (microservices, containers, serverless) where failures are emergent and hard to predict.

> Monitoring tells you **that** something is wrong. Observability helps you find out **why**.

## Monitoring vs Observability
| | Monitoring | Observability |
|---|---|---|
| Question type | Known questions ("is CPU > 90%?") | Unknown questions ("why are some EU users slow?") |
| Approach | Predefined dashboards and alerts | Exploratory, high-dimensional querying |
| Failure modes | Known-unknowns | Unknown-unknowns |
| Data | Aggregates, thresholds | Rich, high-cardinality, correlated events |
| Relationship | A subset/consumer of observability | Foundation that makes monitoring better |

## Telemetry signals
| Signal | What it is | Answers |
|--------|-----------|---------|
| **Metrics** | Numeric time series | Is it healthy? How is it trending? |
| **Logs** | Timestamped event records | What happened in detail? |
| **Traces** | End-to-end request path across services (spans) | Where is time spent? Which dependency failed? |
| **Profiles** | CPU/memory/allocation samples by code location | Which function/line is expensive? |
| **Events** | Discrete business/system occurrences (deploys, flags) | What changed? |
| **RUM / synthetic** | Real-user and scripted-probe data | What does the user experience? |

The "three pillars" (logs, metrics, traces) are a starting point; value comes from **correlating** them (e.g. jump from a latency spike -> slow trace -> span -> logs for that `trace_id` -> profile of the hot function).

## Wide events and high cardinality
Modern observability favors **wide, structured events** (many attributes per event: `user_id`, `region`, `build`, `feature_flag`, `db_shard`). High-cardinality fields let you slice to the exact cohort that's affected. Metrics aggregate and lose this detail; traces/events keep it.

## Why it matters
- Faster detection and **lower MTTR**
- Find unknown failure modes and performance regressions
- Safe, frequent releases (see the effect of each deploy)
- Understand dependencies and real user impact
- Data for capacity planning and cost optimization
- Supports SRE practices: SLOs, error budgets, blameless postmortems

## Maturity model
1. **Reactive**: basic infra metrics, unstructured logs, users report problems.
2. **Proactive**: dashboards, alerts, centralized logs, some tracing.
3. **Correlated**: unified telemetry via trace IDs, SLO-based alerting, deploy markers.
4. **Predictive / optimized**: anomaly detection, continuous profiling, automated remediation, observability-driven development.

## Observability-driven development
- Treat instrumentation as part of the feature's definition of done.
- Ask "how will I know this works in production, and how will I debug it?" at design time.
- Use feature flags + telemetry for progressive delivery and quick rollback.
