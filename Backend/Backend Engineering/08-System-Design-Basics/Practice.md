# Practice — System Design Basics

1. Draw (on paper or a whiteboard tool) a simple 3-tier architecture: load balancer → multiple app servers → a primary database with 2 read replicas. Label which parts handle reads vs writes.
2. Explain in your own words: why must an application be "stateless" to scale horizontally, and give a concrete example of stateful code that would break under horizontal scaling.
3. Pick a real-world system (Twitter, Instagram, an e-commerce checkout) and classify whether it should lean CP or AP for its core data, and justify your reasoning using CAP theorem concepts.
4. Fully work through the URL Shortener design in `Designing-a-URL-Shortener.md` yourself, from scratch, without looking at the answer first — then compare your design's trade-offs.
5. Design (on paper) a simple sharding strategy for a hypothetical "messages" table with 500 million rows — choose range-based or hash-based, and explain why.
6. Research: read about a real production incident caused by database replication lag (search "read your own writes problem") and explain how you'd mitigate it in application code.

✅ Done? Move to `09-Testing`.
