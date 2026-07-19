# Practice — Microservices

1. Take a monolithic app you've built (or a simple to-do/blog app) and identify: which parts, if any, would genuinely benefit from being split into separate services? Write down your reasoning — resist the urge to split everything "because microservices."
2. Design (on paper) a simple 3-service system (e.g., Users, Orders, Notifications) — sketch which calls should be synchronous (REST/gRPC) vs asynchronous (events/queue), and justify each choice.
3. Implement a basic circuit breaker (as shown in `Inter-Service-Communication.md`) around a flaky API call in a small script, and demonstrate it "opening" after repeated failures.
4. Set up two simple services locally (e.g., two small Flask/Express apps) and route between them through a basic API Gateway (even a simple Nginx reverse proxy config counts) — confirm requests are correctly routed based on path.
5. Design a saga (choreography or orchestration, your choice) for a 3-step business process of your choosing (e.g., "book a flight + hotel + rental car," where any failure should cancel the successful earlier bookings) — write out each step's compensating action.
6. Explain in your own words: why is "just add a database transaction" not a valid solution to the distributed consistency problem in microservices?

✅ Done? Move to `12-Scalability-and-Performance`.
