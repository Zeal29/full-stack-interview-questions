# System Design Basics Interview Questions — Top 50

> **Difficulty Split:** Basic (Q1–Q20) | Intermediate (Q21–Q40) | Advanced (Q41–Q50)

---

## Basic Questions (Q1–Q20)

### Q1. What is system design?

**Answer:** System design is the process of defining the architecture, components, interfaces, and data for a system to satisfy specified requirements. It involves making decisions about: servers, databases, caching, load balancing, scalability, and availability.

**💡 Tip:** System design = **"how will you build this?"** Think about: users → requests → servers → databases → responses. Every component has trade-offs.

---

### Q2. What is scalability?

**Answer:** Ability of a system to handle increased load:
- **Vertical (scale up)**: add more CPU/RAM to one server (expensive, limited)
- **Horizontal (scale out)**: add more servers (cost-effective, nearly unlimited)

Horizontal scaling is preferred for large systems. Requires stateless design.

**💡 Tip:** Vertical = **bigger machine**. Horizontal = **more machines**. Design for horizontal from the start — stateless servers behind a load balancer.

---

### Q3. What is a load balancer?

**Answer:** Distributes incoming traffic across multiple servers:
- Algorithms: Round Robin, Least Connections, IP Hash, Weighted
- Health checks: remove unhealthy servers
- SSL termination: handle HTTPS at load balancer

Examples: Nginx, HAProxy, AWS ALB, Cloudflare.

**💡 Tip:** Load balancer = **traffic cop** — distributes requests evenly. If one server dies, traffic goes to others. Always have at least 2 servers behind a load balancer.

---

### Q4. What is caching?

**Answer:** Store frequently accessed data in fast storage (memory) to reduce database load:
- **Application cache**: in-memory (Redis, Memcached)
- **CDN**: cache static assets globally (Cloudflare, CloudFront)
- **Browser cache**: client-side caching with HTTP headers
- **Database cache**: built-in query cache

Cache invalidation is one of the hardest problems in CS.

**💡 Tip:** Cache = **"shortcut for frequent answers."** Redis for API responses. CDN for static files. Cache hits = fast. Cache misses = fetch from DB. TTL for freshness.

---

### Q5. What is a CDN?

**Answer:** Content Delivery Network — distributed servers that deliver content from the nearest location to the user:
- Static assets: images, CSS, JS, videos
- Dynamic content: API responses (some CDNs)
- Reduces latency, handles traffic spikes

Examples: Cloudflare, AWS CloudFront, Akamai.

**💡 Tip:** CDN = **"copies of your content everywhere."** User in Lahore gets content from Lahore server, not US server. Use for all static assets.

---

### Q6. What is a database replica?

**Answer:** Copy of the database for:
- **Read replicas**: distribute read queries (improves read performance)
- **Failover**: if primary dies, replica becomes primary
- **Analytics**: run heavy queries on replica without affecting primary

Read from replicas, write to primary. Asynchronous replication = eventual consistency.

**💡 Tip:** Primary = **writes**. Replicas = **reads**. Most apps are read-heavy (90/10 read/write). Replicas handle the read load.

---

### Q7. What is CAP theorem?

**Answer:** Distributed systems can guarantee at most 2 of 3:
- **Consistency**: all nodes see the same data simultaneously
- **Availability**: every request gets a response
- **Partition tolerance**: works despite network failures

Since network failures are inevitable (P is mandatory), choose: CP or AP.

**💡 Tip:** CAP = **"pick your sacrifice."** Network fails: either be consistent (block until fixed) or available (serve stale data). Most choose AP (availability + eventual consistency).

---

### Q8. What is latency vs throughput?

**Answer:**
- **Latency**: time for a single request to complete (ms) — how fast?
- **Throughput**: number of requests processed per second (RPS) — how many?

Low latency + high throughput = ideal. Often a trade-off (batching increases throughput but latency).

**💡 Tip:** Latency = **"how fast is one request?"** Throughput = **"how many requests per second?"** Optimize both, but they sometimes conflict.

---

### Q9. What is a microservice architecture?

**Answer:** Split application into small, independent services:
- Each service owns its data and logic
- Communicate via APIs (REST/gRPC) or message queues
- Independently deployable and scalable
- Organized around business capability

Opposite of monolith (everything in one app).

**💡 Tip:** Microservices = **"each feature is its own app."** User service, order service, payment service. Independent deployment, scaling, teams. Adds complexity — use when the team is ready.

---

### Q10. What is a monolithic architecture?

**Answer:** All code in one application:
- Pros: simple to develop, easy to test, simple deployment
- Cons: hard to scale parts independently, one bug can crash everything, slow builds

Start monolithic, extract microservices when needed. "Monolith first" is a valid strategy.

**💡 Tip:** Monolith = **"everything in one box."** Simple to start, harder at scale. Start here, split when needed. Don't start with microservices for a small team.

---

### Q11. What is a reverse proxy?

**Answer:** Server that sits between clients and your servers:
- Receives requests → forwards to appropriate server → returns response
- Handles: SSL termination, load balancing, rate limiting, caching
- Hides internal server architecture

Nginx and HAProxy are common reverse proxies.

**💡 Tip:** Reverse proxy = **"receptionist"** — receives visitors, directs them to the right department. Clients never see internal servers. Nginx = most common choice.

---

### Q12. What is horizontal partitioning (sharding)?

**Answer:** Distribute data across multiple database servers:
- Shard by: user ID, geographic region, date range
- Each shard holds a subset of data
- Challenges: cross-shard queries, uneven distribution, resharding

Used when single database can't handle the data volume or traffic.

**💡 Tip:** Sharding = **"split data across machines."** Like splitting a phone book by last name. Each shard = smaller dataset = faster queries. Plan shard key carefully.

---

### Q13. What is a message queue?

**Answer:** Asynchronous communication between services:
- Producer sends message to queue
- Consumer processes it at its own pace
- Decouples services, handles traffic spikes

Examples: RabbitMQ, Kafka, SQS, Redis Pub/Sub.

**💡 Tip:** Message queue = **"mailbox between services."** Sender drops message, receiver picks up when ready. Decouples timing. Absorbs traffic spikes.

---

### Q14. What is the difference between SQL and NoSQL in system design?

**Answer:**
- **SQL**: structured data, ACID transactions, complex queries, vertical scaling
- **NoSQL**: flexible schema, horizontal scaling, high throughput, eventual consistency

Most real systems use both (polyglot persistence). SQL for transactions, NoSQL for scale.

**💡 Tip:** SQL = **structured + consistent** (users, orders). NoSQL = **flexible + scalable** (logs, analytics). Use both for different parts of your system.

---

### Q15. What is a proxy server?

**Answer:**
- **Forward proxy**: acts on behalf of the client (hides client identity, filters requests)
- **Reverse proxy**: acts on behalf of the server (load balancing, SSL, caching)

System design usually refers to reverse proxies. VPNs use forward proxies.

**💡 Tip:** Forward proxy = **"hides the client"** (like a VPN). Reverse proxy = **"hides the server"** (like Nginx). Know the difference.

---

### Q16. What is high availability?

**Answer:** System remains operational even when components fail:
- Redundancy: multiple servers, databases, availability zones
- Failover: automatic switch to backup when primary fails
- Measured in "nines": 99.9% = 8.76 hours downtime/year, 99.99% = 52.6 minutes/year

Achieve with: load balancers, replicas, multi-AZ deployment.

**💡 Tip:** High availability = **"no single point of failure."** If any one thing breaks, the system keeps running. Redundancy + automatic failover.

---

### Q17. What is rate limiting in system design?

**Answer:** Control request rate to protect services:
- **Token bucket**: tokens refill at fixed rate, each request uses one
- **Leaky bucket**: process at fixed rate, queue excess
- **Sliding window**: count requests in rolling time window

Apply at: API gateway, load balancer, application level.

**💡 Tip:** Rate limiting = **"slow down, too many people."** Protects backend from spikes and attacks. Apply at multiple layers (gateway + app).

---

### Q18. What is eventual consistency?

**Answer:** Data updates propagate asynchronously — all replicas will eventually have the same data, but not immediately. Used in distributed systems (DNS, MongoDB, DynamoDB).

Trade-off: higher availability and partition tolerance for temporary inconsistency.

**💡 Tip:** Eventual consistency = **"everyone agrees... eventually."** Like viral news — not everyone knows immediately, but they all will. Trade freshness for availability.

---

### Q19. What is a data lake vs data warehouse?

**Answer:**
- **Data lake**: raw, unstructured data (logs, events, images). Hadoop, S3.
- **Data warehouse**: structured, processed data for analytics. Snowflake, BigQuery, Redshift.

ETL (Extract Transform Load) moves data from lake to warehouse.

**💡 Tip:** Data lake = **"raw storage"** (everything goes in). Data warehouse = **"organized for analysis"** (structured, queried). Lake → ETL → Warehouse.

---

### Q20. What is the N+1 query problem?

**Answer:** Making N+1 database queries instead of 1:
```
// N+1: 1 query for users + N queries for each user's orders
users = SELECT * FROM users;  // 1 query
for each user: SELECT * FROM orders WHERE user_id = ?; // N queries

// Fixed: 1 query using JOIN or IN
SELECT * FROM users JOIN orders ON users.id = orders.user_id; // 1 query
```

**💡 Tip:** N+1 = **"one for the list, then one for each item."** 101 users = 101 queries. Fix with JOIN, batch loading, or DataLoader. Always check your ORM's generated SQL.

---

## Intermediate Questions (Q21–Q40)

### Q21. How to design a URL shortener?

**Answer:**
1. **API**: `POST /shorten` → returns short URL, `GET /{code}` → redirects
2. **Encoding**: base62 (a-z, A-Z, 0-9) for short codes
3. **Database**: `short_code → long_url` mapping (key-value store or SQL)
4. **Cache**: Redis for hot URLs (reduce DB hits)
5. **Redirect**: 301 (permanent, cached) or 302 (temporary, trackable)

**💡 Tip:** URL shortener = **hash + redirect.** Generate short code, store mapping, redirect on access. Cache hot URLs. Track clicks with 302.

---

### Q22. How to design a rate limiter?

**Answer:**
1. **Algorithm**: token bucket (most common)
2. **Storage**: Redis for distributed rate limiting
3. **Implementation**: middleware at API gateway
4. **Headers**: `X-RateLimit-Limit`, `X-RateLimit-Remaining`, `Retry-After`
5. **Response**: 429 Too Many Requests

**💡 Tip:** Rate limiter = **Redis counter + window.** `INCR` + `EXPIRE` per client ID. Token bucket = bursty traffic friendly. Sliding window = precise.

---

### Q23. What is the CQRS pattern?

**Answer:** Separate read and write models:
- **Command** (write): normalized database, optimized for writes
- **Query** (read): denormalized/read replicas, optimized for reads
- Sync via events

Useful when read/write patterns differ significantly (e.g., write to SQL, read from Elasticsearch).

**💡 Tip:** CQRS = **"different databases for reading and writing."** Write to PostgreSQL, read from Redis/Elasticsearch. Sync via events. Complex but powerful for high-scale.

---

### Q24. What is the saga pattern?

**Answer:** Manage distributed transactions without 2PC:
- **Choreography**: services emit events, trigger next step
- **Orchestration**: central coordinator manages the flow
- Each step has a compensating transaction (undo)

Example: Order → Payment → Shipping. If shipping fails → refund payment → cancel order.

**💡 Tip:** Saga = **"chain of local transactions with undo."** Each service does its part. If something fails, run compensating actions in reverse. No distributed locks.

---

### Q25. What is the difference between push and pull architecture?

**Answer:**
- **Push**: server sends data to clients (webhooks, WebSockets, server-sent events)
- **Pull**: clients request data from server (REST APIs, polling)

Push = real-time, complex. Pull = simple, request-driven. Use push for notifications, pull for standard CRUD.

**💡 Tip:** Push = **"server initiates"** (WebSocket, SSE). Pull = **"client asks"** (REST, polling). Push for real-time. Pull for most APIs.

---

### Q26. What is the fan-out problem?

**Answer:** When one action triggers many downstream operations:
- Celebrity posts → notify 10M followers → massive fan-out
- Solutions: pre-compute feeds, lazy fan-out (compute on read), message queues

Choose: fan-out on write (push) vs fan-out on read (pull).

**💡 Tip:** Fan-out = **"one action → many updates."** 1 post → 10M feed entries. Solutions: pre-compute (write time), lazy-compute (read time), or hybrid.

---

### Q27. How to handle hot spots in databases?

**Answer:**
- **Hot key**: one key receives disproportionate traffic → cache it
- **Hot shard**: uneven data distribution → re-shard with better key
- **Hot table**: too many queries → read replicas, caching, partitioning

Identify with: query profiling, monitoring, access patterns.

**💡 Tip:** Hot spot = **"everyone wants the same data."** Cache it (Redis), replicate it, or distribute more evenly. Monitor query patterns.

---

### Q28. What is the API gateway pattern?

**Answer:** Single entry point for all API requests:
- Routing: direct to appropriate service
- Auth: verify tokens centrally
- Rate limiting: protect services
- Transformation: request/response modification
- Aggregation: combine multiple service calls

Kong, AWS API Gateway, Nginx, Traefik.

**💡 Tip:** API gateway = **"smart front door."** All traffic enters here. Handles cross-cutting concerns (auth, rate limiting, logging). Services focus on business logic.

---

### Q29. What is circuit breaker pattern?

**Answer:** Prevent cascading failures:
- **Closed**: normal operation (requests flow)
- **Open**: failures detected (requests rejected immediately)
- **Half-open**: testing recovery (limited requests allowed)

If a service fails N times → open circuit → reject requests → give it time to recover.

**💡 Tip:** Circuit breaker = **"stop trying if it's broken."** Like electrical circuit breaker. Service failing? Stop calling it. Check periodically if it's back.

---

### Q30. What is the bulkhead pattern?

**Answer:** Isolate resources so failure in one area doesn't bring down the whole system:
- Thread pool per service (not shared)
- Connection pool per service
- Separate resources for critical vs non-critical operations

Like ship bulkheads — if one compartment floods, the ship stays afloat.

**💡 Tip:** Bulkhead = **"watertight compartments."** If the email service dies, payment service keeps working. Isolate resources per service/function.

---

### Q31. What is the backpressure pattern?

**Answer:** When a downstream service can't keep up, signal upstream to slow down:
- Consumer tells producer "I'm busy, send less"
- Implemented with: reactive streams, message queue depth
- Prevents system overload by propagating pressure upstream

**💡 Tip:** Backpressure = **"slow down, I'm drowning!"** Consumer signals producer to send less. Like a sink drain backing up — stop the water.

---

### Q32. What is the strangler fig pattern?

**Answer:** Gradually migrate from monolith to microservices:
1. Route new features to microservices
2. Gradually redirect existing features
3. Old monolith shrinks over time
4. Eventually remove monolith

No big-bang rewrite — incremental migration.

**💡 Tip:** Strangler fig = **"replace bit by bit."** Named after a tree that grows around another. Route new traffic to new services. Old system slowly dies off.

---

### Q33. How to estimate system capacity?

**Answer:** Back-of-envelope calculations:
- **Users**: 10M users, 20% daily active = 2M DAU
- **Requests**: 10 requests/day = 20M requests/day ≈ 230 QPS
- **Storage**: 2M users × 1MB data = 2TB
- **Bandwidth**: 20M requests × 10KB = 200GB/day
- Always add 3-5x headroom

**💡 Tip:** Capacity estimation = **"how much do we need?"** DAU → QPS → storage → bandwidth. Always multiply by 3-5x for headroom. Round up.

---

### Q34. What is the difference between consistency patterns?

**Answer:**
- **Strong consistency**: read always returns latest write (SQL default)
- **Weak consistency**: read may not return latest write
- **Eventual consistency**: all replicas converge eventually (many NoSQL)
- **Causal consistency**: preserves cause-effect order

Choose based on business requirements (banking = strong, social media = eventual).

**💡 Tip:** Strong = **"always fresh."** Eventual = **"eventually fresh."** Causal = **"causes come before effects."** Match to business need.

---

### Q35. What is the sidecar pattern?

**Answer:** Helper service running alongside your main service:
- Handles: logging, monitoring, configuration, health checks
- Same pod (Kubernetes) as main service
- Decouples infrastructure concerns from business logic

Example: Envoy proxy as sidecar in service mesh.

**💡 Tip:** Sidecar = **"personal assistant for your service."** Handles cross-cutting concerns. Main service focuses on business logic. Common in Kubernetes.

---

### Q36. What is the ambassador pattern?

**Answer:** Helper service that handles network communication on behalf of your service:
- Service discovery
- Circuit breaking
- Retries and timeouts
- TLS termination

Client → Ambassador → External service. Decouples network logic from business logic.

**💡 Tip:** Ambassador = **"diplomat for your service."** Handles all external communication. Service doesn't know about network complexity.

---

### Q37. What is the leader election pattern?

**Answer:** In a cluster, one node becomes the leader:
- Leader handles writes, coordinates tasks
- Followers handle reads, stand by for failover
- If leader dies → election → new leader

Tools: ZooKeeper, etcd, Consul. Used in: Kafka, MongoDB, PostgreSQL replication.

**💡 Tip:** Leader election = **"pick a boss."** One leader, many followers. Leader dies → vote for new one. Prevents split-brain (two leaders).

---

### Q38. What is the difference between stateful and stateless services?

**Answer:**
- **Stateless**: no session data stored on server. Any server can handle any request. Easy to scale horizontally.
- **Stateful**: server stores session/context. Client must hit the same server (sticky sessions). Harder to scale.

Design for stateless — store state externally (Redis, database).

**💡 Tip:** Stateless = **"any server can handle any request"** (scale easily). Stateful = **"client must return to same server"** (sticky sessions). Always design for stateless.

---

### Q39. What is a service mesh?

**Answer:** Infrastructure layer for service-to-service communication:
- **Data plane**: proxies (sidecars) handle traffic between services
- **Control plane**: manages proxies, policies, routing

Tools: Istio, Linkerd. Handles: mTLS, retries, circuit breaking, observability.

**💡 Tip:** Service mesh = **"communication layer for microservices."** Proxies handle networking concerns. Services focus on business logic. Adds complexity — use when you have many services.

---

### Q40. What is idempotency in system design?

**Answer:** Operations that produce the same result no matter how many times executed:
- Payment processing: charge once, not twice
- Message processing: process once, not on retry
- Use: idempotency keys, deduplication, unique constraints

Critical for distributed systems with retries.

**💡 Tip:** Idempotent = **"safe to retry."** Payment with same idempotency key = charge once. Always design for idempotency in distributed systems.

---

## Advanced Questions (Q41–Q50)

### Q41. How to design a notification system?

**Answer:**
1. **Event sources**: order placed, payment received, etc.
2. **Message queue**: decouple event from notification
3. **Notification service**: processes events, determines channels
4. **Channels**: email (SendGrid), push (FCM/APNs), SMS (Twilio), in-app
5. **Templates**: per-channel templates
6. **Preferences**: user opt-in/opt-out per channel
7. **Rate limiting**: prevent spam
8. **Analytics**: track delivery, open rates

**💡 Tip:** Notification system = **event → queue → service → channels.** Decouple with queues. Respect user preferences. Track delivery metrics.

---

### Q42. How to design a news feed system?

**Answer:**
1. **Fan-out on write**: pre-compute feeds when user posts (push model)
2. **Fan-out on read**: compute feed when user requests (pull model)
3. **Hybrid**: pre-compute for active users, compute on read for celebrities
4. **Storage**: `feed:userId → [post1, post2, ...]` in Redis
5. **Ranking**: score posts by recency, engagement, relevance
6. **Pagination**: cursor-based for infinite scroll

**💡 Tip:** Feed = **fan-out + ranking.** Pre-compute for regular users. Lazy-compute for celebrities. Redis for fast feed retrieval. Rank by recency + engagement.

---

### Q43. What is the two-phase commit (2PC)?

**Answer:** Distributed transaction protocol:
1. **Prepare phase**: coordinator asks all participants "ready to commit?"
2. **Commit phase**: if all say yes → commit. If any say no → rollback.

Strong consistency but: blocking (waits for all), slow, single point of failure (coordinator).

**💡 Tip:** 2PC = **"everyone promises, then everyone commits."** Strong but slow. Blocking if a participant crashes. Prefer saga for modern microservices.

---

### Q44. What is database connection pooling?

**Answer:** Maintain pool of reusable database connections:
- Creating connections is expensive (TCP + auth)
- Pool of N connections shared across requests
- Configure: min/max connections, timeout, idle timeout

Tools: HikariCP (Java), PgBouncer (PostgreSQL), built-in in Node.js drivers.

**💡 Tip:** Connection pool = **"recycle connections."** Creating = slow. Reusing = fast. Size pool based on: concurrent users, query time. Too many = database overload.

---

### Q45. What is a Bloom filter?

**Answer:** Probabilistic data structure that tests set membership:
- **"Probably in set"** → might be wrong (false positive)
- **"Definitely not in set"** → always correct (no false negative)
- Very space-efficient (uses bits, not full data)

Use for: "has this URL been seen?", "might this username exist?", caching pre-check.

**💡 Tip:** Bloom filter = **"quick maybe-check before expensive lookup."** "Probably seen" = check DB. "Definitely not seen" = skip DB. Saves expensive queries.

---

### Q46. What is consistent hashing?

**Answer:** Distribute data across servers with minimal redistribution on changes:
- Hash ring: servers and keys mapped to same hash space
- Key assigned to nearest clockwise server
- Adding/removing server: only K/N keys move (K=keys, N=servers)

Used in: Cassandra, DynamoDB, distributed caches.

**💡 Tip:** Consistent hashing = **"add a server, move minimal data."** Hash ring places servers and keys. New server only steals keys from neighbor. No massive reshuffling.

---

### Q47. What is a vector clock?

**Answer:** Track causality in distributed systems:
- Each node maintains a counter
- On event: increment own counter
- On receive: take max of own and received counters
- Compare: if A ≤ B in all positions → A happened before B

Solves: ordering events without central clock.

**💡 Tip:** Vector clock = **"who happened first?"** in distributed systems. Each node has a counter. Compare vectors to determine ordering. DynamoDB uses this.

---

### Q48. How to handle distributed system failures?

**Answer:**
- **Retry**: exponential backoff + jitter
- **Circuit breaker**: stop calling failing services
- **Timeout**: don't wait forever
- **Fallback**: return cached/default response
- **Bulkhead**: isolate failing components
- **Health checks**: detect failures early
- **Monitoring**: alert on anomalies

Design for failure — everything will fail eventually.

**💡 Tip:** Design for failure = **"expect everything to break."** Retry with backoff. Circuit break. Timeout. Fallback. Monitor. "Everything fails, all the time." — Werner Vogels

---

### Q49. What is the difference between RPC and REST in microservices?

**Answer:**
- **REST**: HTTP-based, text (JSON), human-readable, widely supported, higher latency
- **gRPC**: HTTP/2-based, binary (protobuf), faster, strict schema, streaming support

Use gRPC for internal service-to-service communication. Use REST for external APIs.

**💡 Tip:** gRPC = **internal** (fast, typed, streaming). REST = **external** (simple, readable, universal). Many companies use REST externally, gRPC internally.

---

### Q50. How to approach a system design interview?

**Answer:**
1. **Clarify requirements**: functional + non-functional (scale, latency, availability)
2. **Estimate scale**: QPS, storage, bandwidth (back-of-envelope)
3. **Define API**: endpoints, data models
4. **High-level design**: draw the architecture (clients → LB → servers → DB → cache)
5. **Deep dive**: discuss specific components in detail
6. **Identify bottlenecks**: single points of failure, scaling limits
7. **Trade-offs**: justify every decision

**💡 Tip:** System design = **"ask → estimate → sketch → deep dive → optimize."** Always clarify first. Draw diagrams. Discuss trade-offs. There's no perfect answer — show your thinking.

---
