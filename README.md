# 📘 System Design Notes

This repository contains structured notes for core system design topics.

---

## 📂 Folder Structure

* 📁 [Introduction](./Introduction)
* 📁 [Single Server Setup](./Single_Server_Setup)
* 📁 [Databases (SQL, NoSQL, Graph)](./Databases_SQL_NoSQL_Graph)
* 📁 [Vertical vs Horizontal Scaling](./Vertical_vs_Horizontal_Scaling)
* 📁 [Load Balancing](./Load_Balancing)
* 📁 [Health Checks](./Health_Checks)
* 📁 [Single Point of Failure](./Single_Point_of_Failure)
* 📁 [API Design](./API_Design)
* 📁 [API Protocols](./API_Protocols)
* 📁 [Transport Layer (TCP, UDP)](./Transport_Layer_TCP_UDP)
* 📁 [RESTful APIs](./RESTful_APIs)
* 📁 [GraphQL](./GraphQL)
* 📁 [Authentication](./Authentication)
* 📁 [Authorization](./Authorization)
* 📁 [Security](./Security)

---


### 1. Performance vs. Scalability
A performance problem exists when a system is slow for a single user, whereas a scalability problem exists when the system is fast for one user but slows down under heavy load. A system is scalable if increased performance is proportional to the resources added, allowing it to handle more work or larger datasets.

---

### 2. CAP Theorem
The CAP theorem states that a distributed system can only simultaneously support two of three guarantees: Consistency, Availability, and Partition Tolerance. Because networks are inherently unreliable, systems must support partition tolerance, forcing a software trade-off between consistency (CP) and availability (AP).

---

### 3. Push vs. Pull CDNs
Push CDNs require the server to proactively upload new or changed content to the CDN, making them ideal for sites with low traffic or infrequently updated content. Pull CDNs grab new content from the server only when a user first requests it, which minimizes CDN storage but results in a slower initial request until the content is cached.

---

### 4. Layer 4 vs. Layer 7 Load Balancing
Layer 4 load balancing distributes traffic based on transport layer data like IP addresses and ports without inspecting packet contents, requiring fewer computing resources. Layer 7 load balancing inspects the application layer (headers, cookies, and messages) to make routing decisions, allowing it to direct specific types of traffic, such as video or billing, to specialized servers.

---

### 5. Database Federation
Federation, or functional partitioning, involves splitting a monolithic database into multiple databases based on function, such as separate databases for "users" and "products." This reduces read/write traffic and replication lag for each database while allowing more data to fit in memory for improved cache locality.


![Database Federation](./Digrams/db_fed.svg)

---

### 6. Disadvantages of Sharding
Sharding adds significant complexity to application logic and can result in "lopsided" shards if power users or geographic hotspots create uneven loads. Furthermore, joining data across multiple shards is highly complex, and rebalancing the cluster to address growth requires additional architectural overhead.

---

### 7. ACID vs. BASE
ACID (Atomicity, Consistency, Isolation, Durability) is used by relational databases to ensure strict transaction reliability and valid states. In contrast, BASE (Basically Available, Soft state, Eventual consistency) is used by NoSQL systems to prioritize availability over immediate consistency, allowing the system to reach a consistent state over time.

---

### 8. Cache-aside Strategy
In a cache-aside workflow, the application first checks the cache for an entry; if it results in a miss, the application must then load the data from the database and manually add it to the cache. This results in three separate trips (cache check, database read, cache write), which can cause a noticeable delay for the initial request.

---

### 9. Message Queues
Message queues allow expensive or slow operations to be performed in the background so the user is not blocked waiting for a task to complete. An application publishes a job to the queue and notifies the user of the status immediately, while a separate worker process handles the actual computation or delivery.

---

### 10. UDP vs. TCP
UDP should be chosen when the lowest possible latency is required and when late data is considered worse than lost data. It is ideal for real-time use cases like VoIP, video chat, and multiplayer games where the overhead of TCP's reliability guarantees (like retransmission and handshakes) would cause unacceptable delays.