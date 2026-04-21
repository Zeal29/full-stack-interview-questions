# SQL, NoSQL & SQL vs NoSQL Interview Questions — Top 50

> **Difficulty Split:** Basic (Q1–Q20) | Intermediate (Q21–Q40) | Advanced (Q41–Q50)

---

## Basic Questions (Q1–Q20)

### Q1. What is SQL?

**Answer:** SQL (Structured Query Language) is the standard language for managing relational databases. It handles data definition (DDL: CREATE, ALTER, DROP), data manipulation (DML: SELECT, INSERT, UPDATE, DELETE), data control (DCL: GRANT, REVOKE), and transaction control (TCL: COMMIT, ROLLBACK).

**💡 Tip:** SQL = **language for relational databases**. CRUD operations + schema management + access control. One language, many databases (PostgreSQL, MySQL, SQL Server).

---

### Q2. What is NoSQL?

**Answer:** NoSQL (Not Only SQL) databases store and retrieve data using non-tabular formats: documents (MongoDB), key-value (Redis), wide-column (Cassandra), graphs (Neo4j). Designed for flexibility, horizontal scaling, and high performance on specific workloads.

**💡 Tip:** NoSQL = **"Not ONLY SQL"** (not "no SQL"). Non-tabular, flexible schema, scales horizontally. Different types for different needs.

---

### Q3. What are the types of NoSQL databases?

**Answer:**
- **Document**: MongoDB, CouchDB — JSON-like documents
- **Key-Value**: Redis, DynamoDB — simple key → value pairs
- **Wide-column**: Cassandra, HBase — rows with flexible columns
- **Graph**: Neo4j, ArangoDB — nodes and relationships

**💡 Tip:** **D-K-W-G**: Document (JSON), Key-Value (dictionary), Wide-column (spreadsheet), Graph (network). Each optimized for specific data patterns.

---

### Q4. What is the difference between SQL and NoSQL?

**Answer:**
| Feature | SQL | NoSQL |
|---|---|---|
| Schema | Fixed, predefined | Flexible, dynamic |
| Scaling | Vertical (scale up) | Horizontal (scale out) |
| Queries | SQL (standardized) | Varies by database |
| ACID | Full support | Varies (some support) |
| Relationships | Foreign keys, JOINs | Embedding, references |
| Best for | Complex queries, transactions | Flexible data, high scale |

**💡 Tip:** SQL = **structured, relational, consistent**. NoSQL = **flexible, scalable, fast**. Not "which is better" — "which fits your use case."

---

### Q5. What is ACID compliance?

**Answer:**
- **Atomicity**: all operations succeed or all fail
- **Consistency**: database always in valid state
- **Isolation**: concurrent transactions don't interfere
- **Durability**: committed data survives crashes

SQL databases are typically ACID-compliant. NoSQL varies: MongoDB supports ACID transactions, Cassandra offers tunable consistency.

**💡 Tip:** ACID = **"transactions you can trust."** Atomic (all-or-nothing), Consistent (valid state), Isolated (no interference), Durable (survives crashes). SQL = ACID by default.

---

### Q6. What is the CAP theorem?

**Answer:** Distributed systems can guarantee at most TWO of three:
- **Consistency**: all nodes see the same data
- **Availability**: every request gets a response (not error)
- **Partition tolerance**: system works despite network failures

Since network failures are inevitable, the real choice is: CP (consistency) or AP (availability).

**💡 Tip:** CAP = **pick 2 of 3**. In reality, always have P (networks fail). So choose: CP (consistent but may be unavailable) or AP (available but may be stale).

---

### Q7. What is eventual consistency?

**Answer:** If no new updates are made, eventually all nodes will have the same data. Writes propagate asynchronously — reads may return stale data temporarily. Used by: DynamoDB, Cassandra, MongoDB (with read concern "local").

Strong consistency = always fresh. Eventual = eventually fresh, faster reads.

**💡 Tip:** Eventual consistency = **"everyone will agree, just not immediately."** Like gossip — news spreads but takes time. Trade freshness for speed/availability.

---

### Q8. What is the BASE model?

**Answer:** NoSQL alternative to ACID:
- **Basically Available**: system responds (may be stale)
- **Soft state**: data can change without input (replication lag)
- **Eventually consistent**: data converges over time

BASE is the trade-off for high availability and partition tolerance in distributed systems.

**💡 Tip:** BASE = **"probably right, eventually."** ACID = guaranteed correct NOW. BASE = correct EVENTUALLY. BASE for performance, ACID for correctness.

---

### Q9. When should you use SQL over NoSQL?

**Answer:** Use SQL when:
- Data is structured with clear relationships (users → orders → items)
- ACID transactions are critical (banking, inventory)
- Complex queries with JOINs, aggregations needed
- Data integrity is paramount
- Reporting and analytics are important

**💡 Tip:** SQL = **structured, relational, critical transactions**. Banking, ERP, CRM, e-commerce. If your data has clear tables and relationships → SQL.

---

### Q10. When should you use NoSQL over SQL?

**Answer:** Use NoSQL when:
- Schema is flexible or evolving (user-generated content)
- Need horizontal scaling across many servers
- High write throughput is required (IoT, logging)
- Data is hierarchical/document-like (CMS, catalogs)
- Rapid development with changing requirements

**💡 Tip:** NoSQL = **flexible, high-scale, fast iteration**. IoT data, real-time analytics, content management, social media. If schema changes often → NoSQL.

---

### Q11. What is vertical vs horizontal scaling?

**Answer:**
- **Vertical (scale up)**: add more CPU/RAM to existing server. Limited by hardware. SQL databases typically scale vertically.
- **Horizontal (scale out)**: add more servers. Virtually unlimited. NoSQL databases designed for horizontal scaling.

SQL: harder to shard across servers. NoSQL: built-in sharding.

**💡 Tip:** Vertical = **bigger machine** (expensive, has limits). Horizontal = **more machines** (cheap, unlimited). NoSQL wins at horizontal scale.

---

### Q12. What is a JOIN and why do NoSQL databases avoid them?

**Answer:** JOINs combine data from multiple tables in SQL. They're powerful but expensive (especially across servers in a sharded setup).

NoSQL avoids JOINs by: embedding related data in one document, denormalizing data, or using application-level joins ($lookup in MongoDB). This trades storage for query speed.

**💡 Tip:** JOIN = **"combine at query time"** (SQL). Embedding = **"combine at write time"** (NoSQL). JOINs don't scale well across servers — that's why NoSQL avoids them.

---

### Q13. What is a foreign key and do NoSQL databases have them?

**Answer:** Foreign key = column referencing another table's primary key. Enforces referential integrity in SQL.

Most NoSQL databases don't have native foreign keys. References are managed at the application level. Some (like MongoDB) support manual references and $lookup.

**💡 Tip:** SQL FK = **database-enforced relationship**. NoSQL = **application-enforced relationship**. SQL guarantees integrity. NoSQL gives flexibility but requires discipline.

---

### Q14. What is a schema in SQL?

**Answer:** Schema defines the structure: tables, columns, data types, constraints, relationships. Must be defined before inserting data. Changes require migrations (ALTER TABLE).

NoSQL has dynamic/flexible schema — each document can have different fields.

**💡 Tip:** SQL schema = **"dress code required."** NoSQL = **"come as you are."** Schema = safety net + upfront design work. No schema = flexibility + potential mess.

---

### Q15. What is sharding?

**Answer:** Distributing data across multiple servers (shards):
- **SQL sharding**: complex, application-managed, cross-shard JOINs expensive
- **NoSQL sharding**: built-in, automatic (MongoDB, Cassandra)

Shard key determines data placement. Bad shard key = uneven distribution (hot shards).

**💡 Tip:** Sharding = **splitting data across machines.** NoSQL does it natively. SQL requires manual setup (Citus for PostgreSQL). Choose shard key wisely — can't change easily.

---

### Q16. What is a data model?

**Answer:** How data is organized and related:
- **Relational model**: tables with rows/columns, relationships via foreign keys (SQL)
- **Document model**: nested JSON-like objects (MongoDB)
- **Key-value model**: simple key → value pairs (Redis)
- **Graph model**: nodes and edges (Neo4j)
- **Wide-column model**: rows with flexible column families (Cassandra)

**💡 Tip:** Data model = **"how data is shaped."** Relational = tables. Document = JSON. Key-value = dictionary. Choose based on your data's natural shape.

---

### Q17. What is a database migration?

**Answer:** Controlled way to evolve database schema over time:
- **SQL**: ALTER TABLE, CREATE TABLE in versioned migration files
- **NoSQL**: often application-level (add new fields without breaking old code)

Tools: Flyway, Liquibase (SQL), Prisma Migrate, custom scripts.

**💡 Tip:** Migrations = **version control for your database schema**. Apply incrementally. Never manually ALTER production databases. Use migration tools.

---

### Q18. What is data denormalization and when is it needed?

**Answer:** Storing redundant data for read performance:
- **SQL**: normalize first, denormalize selectively (materialized views, caching)
- **NoSQL**: denormalize by default (embed related data)

Trade storage for speed. Needed when JOINs become too expensive or in distributed systems.

**💡 Tip:** Denormalize = **"store the same thing twice for speed."** SQL: normalize, then denormalize hot paths. NoSQL: denormalize by design.

---

### Q19. What is an ORM and how does it relate to SQL/NoSQL?

**Answer:** ORM (Object-Relational Mapping) maps database records to programming language objects:
- **SQL ORMs**: Prisma, TypeORM, Sequelize, Django ORM
- **NoSQL ODMs**: Mongoose (MongoDB)

Abstracts database operations. Can generate inefficient queries — know what SQL your ORM produces.

**💡 Tip:** ORM = **"talk to the database in your programming language."** Convenient but can hide inefficiencies. Always check generated SQL. Know raw queries for complex cases.

---

### Q20. What is polyglot persistence?

**Answer:** Using different databases for different services/use cases within the same application:
- PostgreSQL for user data (ACID)
- Redis for caching (fast key-value)
- Elasticsearch for search (full-text)
- MongoDB for content (flexible schema)

Microservices commonly use this pattern.

**💡 Tip:** Polyglot persistence = **"right tool for each job."** Don't force one database for everything. Match the database to the data pattern.

---

## Intermediate Questions (Q21–Q40)

### Q21. What is the difference between MongoDB and PostgreSQL?

**Answer:**
| Feature | PostgreSQL | MongoDB |
|---|---|---|
| Type | Relational (SQL) | Document (NoSQL) |
| Schema | Fixed | Flexible |
| Queries | SQL | MongoDB Query Language |
| JOINs | Native | $lookup (limited) |
| Scaling | Vertical | Horizontal (sharding) |
| ACID | Full | Supported (4.0+) |
| Best for | Complex relations | Flexible data, rapid dev |

**💡 Tip:** PostgreSQL = **structured + complex queries**. MongoDB = **flexible + horizontal scale**. Many apps use both (polyglot persistence).

---

### Q22. What is Cassandra and when to use it?

**Answer:** Apache Cassandra is a wide-column NoSQL database optimized for:
- High write throughput
- Linear horizontal scalability
- Multi-datacenter replication
- No single point of failure

Use for: time-series data, IoT, messaging, logging. Not for: complex queries, JOINs, ACID transactions.

**💡 Tip:** Cassandra = **"write anything, anywhere, at massive scale."** Netflix, Apple, Instagram use it. Not for relational data. Great for append-heavy workloads.

---

### Q23. What is Redis and when to use it?

**Answer:** Redis is an in-memory key-value store:
- **Use for**: caching, session storage, rate limiting, queues, leaderboards, pub/sub
- **Data types**: strings, lists, sets, sorted sets, hashes, streams
- **Speed**: sub-millisecond latency
- **Persistence**: optional (RDB snapshots, AOF)

Not a primary database — use alongside SQL/NoSQL.

**💡 Tip:** Redis = **blazing fast cache + data structures.** Cache, sessions, rate limits, leaderboards. In-memory = fast but volatile. Always pair with a persistent database.

---

### Q24. What is the difference between SQL and NewSQL?

**Answer:**
- **SQL**: traditional RDBMS (PostgreSQL, MySQL) — ACID, vertical scaling
- **NoSQL**: non-relational — flexible, horizontal, eventual consistency
- **NewSQL**: relational with horizontal scaling (CockroachDB, Google Spanner, TiDB) — ACID + distributed

NewSQL = SQL semantics + NoSQL scalability.

**💡 Tip:** NewSQL = **"best of both worlds"** (ACID + horizontal scale). CockroachDB, Spanner, TiDB. SQL syntax, distributed architecture. For when you need both.

---

### Q25. What is a materialized view vs a regular view?

**Answer:**
- **View**: virtual table — runs the query each time (no storage)
- **Materialized view**: pre-computed results stored on disk — fast reads but stale until refreshed

SQL databases support both. NoSQL achieves similar results with pre-aggregated collections.

**💡 Tip:** View = **"saved query, always fresh."** Materialized view = **"saved results, refresh periodically."** Use materialized views for expensive aggregations.

---

### Q26. What is database replication?

**Answer:** Copying data across multiple servers:
- **SQL**: Primary-replica (read replicas), synchronous/asynchronous
- **NoSQL**: Replica sets (MongoDB), eventual consistency (Cassandra)

Benefits: high availability, read scaling, disaster recovery.

**💡 Tip:** Replication = **"copy data for safety and speed."** Primary handles writes, replicas handle reads. SQL = read replicas. NoSQL = replica sets.

---

### Q27. What is a transaction log (WAL)?

**Answer:** Write-Ahead Log records all changes before applying them:
- SQL: WAL (PostgreSQL), binlog (MySQL), transaction log (SQL Server)
- NoSQL: oplog (MongoDB), commitlog (Cassandra)

Enables: crash recovery, replication, point-in-time recovery.

**💡 Tip:** WAL = **"journal before action."** Write change to log → then apply. Crash → replay log → recover. The foundation of durability.

---

### Q28. What is a database connection pool?

**Answer:** Pool of reusable database connections:
- Creating connections is expensive (TCP handshake, auth)
- Pool maintains N connections, reuses them
- SQL: PgBouncer (PostgreSQL), HikariCP (Java)
- NoSQL: built-in connection pooling in drivers

**💡 Tip:** Connection pool = **"recycle connections."** Creating = expensive. Reusing = fast. Always use pooling in production. Default pool size: 10-20.

---

### Q29. What is time-series data and which database to use?

**Answer:** Data points indexed by time (metrics, IoT, stock prices):
- **SQL**: PostgreSQL with TimescaleDB extension, partitioned tables
- **NoSQL**: InfluxDB, Timeseries collections (MongoDB), Cassandra
- **Specialized**: Prometheus, QuestDB

Choose based on: write volume, query patterns, retention policy.

**💡 Tip:** Time-series = **timestamp + value**. High write volume, time-range queries. TimescaleDB (SQL) or InfluxDB (specialized) for serious time-series workloads.

---

### Q30. What is graph data and when to use a graph database?

**Answer:** Data modeled as nodes (entities) and edges (relationships):
- Social networks (friends, followers)
- Recommendation engines
- Fraud detection (connected patterns)
- Knowledge graphs

Databases: Neo4j, ArangoDB, Amazon Neptune.

**💡 Tip:** Graph databases = **"relationships are the data."** SQL can do graphs (recursive CTEs) but slowly. Neo4j traverses relationships in milliseconds.

---

### Q31. What is data consistency in distributed databases?

**Answer:** Consistency models (strongest to weakest):
- **Strong/Linearizable**: always read latest write (SQL default)
- **Sequential**: see writes in order (but may be delayed)
- **Causal**: see causally related writes in order
- **Eventual**: all nodes converge eventually (many NoSQL)
- **Read-your-writes**: see your own writes immediately

**💡 Tip:** Strong = **"always fresh"** (slower). Eventual = **"eventually fresh"** (faster). Choose based on business requirements.

---

### Q32. What is a database cursor?

**Answer:** A pointer that iterates through query results:
- **SQL**: server-side cursors for large result sets (FETCH 100 rows at a time)
- **NoSQL**: MongoDB `find()` returns a cursor (lazy evaluation)

Avoids loading entire result sets into memory.

**💡 Tip:** Cursor = **"iterate without loading everything."** Process results in batches. Both SQL and NoSQL use cursors internally for large queries.

---

### Q33. What is a dead tuple and how does it affect performance?

**Answer:** Updated/deleted rows that still occupy disk space (MVCC side effect):
- **SQL (PostgreSQL)**: VACUUM reclaims space
- **NoSQL (MongoDB)**: WiredTiger reclaims during checkpoints

Accumulated dead tuples = bloated tables, slower scans, wasted disk.

**💡 Tip:** Dead tuples = **"ghost rows"** taking space. VACUUM (PostgreSQL) cleans them up. Auto-vacuum handles it, but monitor `n_dead_tup` in production.

---

### Q34. What is a stored procedure and do NoSQL databases have them?

**Answer:**
- **SQL**: server-side functions (PL/pgSQL, T-SQL) running in the database
- **NoSQL**: generally no stored procedures. MongoDB has limited server-side JS.

Modern approach: keep logic in the application, use databases for storage.

**💡 Tip:** Stored procedures = **"code in the database."** SQL supports them, NoSQL generally doesn't. Modern trend: business logic in app, database for data.

---

### Q35. What is the difference between SQL and NoSQL for caching?

**Answer:**
- **SQL**: no built-in caching (use Redis/Memcached alongside). Query cache exists but limited.
- **NoSQL**: some built-in caching (MongoDB WiredTiger cache, Cassandra row cache)
- **Redis**: purpose-built cache with TTL, eviction policies

Most production apps use Redis for caching regardless of primary database.

**💡 Tip:** Neither SQL nor NoSQL replaces a proper cache. Use Redis for caching layer. SQL and NoSQL = source of truth. Redis = fast access layer.

---

### Q36. What is a database index in SQL vs NoSQL?

**Answer:**
- **SQL**: B-tree (default), hash, GIN, GiST. CREATE INDEX statement. Indexes on table columns.
- **NoSQL**: MongoDB has B-tree, GIN-like, text, geospatial. No explicit CREATE INDEX in some (Cassandra has materialized views).

Both use similar data structures. SQL has more mature indexing options.

**💡 Tip:** Indexing concept is **the same** across SQL and NoSQL. B-tree = common default. NoSQL may have fewer index types but similar principles.

---

### Q37. What is a database backup strategy?

**Answer:**
- **SQL**: `pg_dump`, `mysqldump`, full + incremental backups, WAL archiving for PITR
- **NoSQL**: `mongodump`, snapshots, oplog for point-in-time
- **Both**: schedule regular backups, test restoration, store off-site

3-2-1 rule: 3 copies, 2 different media, 1 off-site.

**💡 Tip:** Backup = **"insurance you hope to never use."** Full backup weekly, incremental daily. TEST restoration regularly. Untested backup = no backup.

---

### Q38. What is a query execution plan?

**Answer:** Database's strategy for executing a query:
- **SQL**: `EXPLAIN ANALYZE` shows: scan type, join strategy, estimated cost
- **NoSQL**: `explain()` shows: index used, documents scanned, execution time

Use to find: missing indexes, inefficient joins, full table scans.

**💡 Tip:** Execution plan = **"X-ray of your query."** Always EXPLAIN before optimizing. Don't guess — measure. Look for full scans and missing indexes.

---

### Q39. What is the difference between SQL and NoSQL for reporting?

**Answer:**
- **SQL**: excellent for reporting — complex JOINs, window functions, aggregations, GROUP BY
- **NoSQL**: aggregation pipeline (MongoDB), but limited compared to SQL

For heavy reporting: use SQL data warehouse (dedicated reporting database).

**💡 Tip:** SQL = **reporting champion** (JOINs, aggregations, window functions). NoSQL = **operational champion** (fast reads/writes). Use SQL for reports, NoSQL for operations.

---

### Q40. What is database monitoring?

**Answer:**
Track: query performance, connection counts, cache hit ratio, disk usage, replication lag, lock waits.
- **SQL**: pg_stat_statements, pg_stat_activity, slow query log
- **NoSQL**: Atlas monitoring, `serverStatus`, Profiler

Tools: Prometheus, Grafana, Datadog, New Relic.

**💡 Tip:** Monitoring = **"dashboard for database health."** Track slow queries, connection limits, replication lag. Alert before users notice problems.

---

## Advanced Questions (Q41–Q50)

### Q41. What is the PACELC theorem?

**Answer:** Extension of CAP theorem that also considers latency during normal operation:
- **P**artition: during network partition → choose **A**vailability or **C**onsistency
- **E**lse: during normal operation → choose **L**atency or **C**onsistency

Example: MongoDB = PA/EL (available during partition, prefers latency normally).

**💡 Tip:** PACELC = **"CAP, but also think about latency."** Not just during failures — what do you trade during normal operations? Latency vs consistency.

---

### Q42. What is multi-model database?

**Answer:** Database supporting multiple data models in one system:
- **ArangoDB**: document + graph + key-value
- **PostgreSQL**: relational + JSONB + full-text + geospatial
- **FaunaDB**: relational + document + graph

Reduce need for polyglot persistence. One database, multiple paradigms.

**💡 Tip:** Multi-model = **"one database, many data shapes."** PostgreSQL is surprisingly multi-model (JSONB, arrays, geospatial). Consider before adding another database.

---

### Q43. What is CQRS (Command Query Responsibility Segregation)?

**Answer:** Separate read and write models:
- **Write side**: normalized SQL database (optimized for writes)
- **Read side**: denormalized NoSQL/read replicas (optimized for reads)
- Events synchronize data between sides

Common in event-driven architectures and microservices.

**💡 Tip:** CQRS = **"different databases for reading and writing."** Write to SQL, read from NoSQL/cache. Complex but powerful for high-scale systems.

---

### Q44. What is event sourcing?

**Answer:** Store every state change as an immutable event instead of current state:
```
Instead of: balance = 500
Store: [Deposited 1000, Withdrew 200, Deposited 50, Withdrew 350]
Current state = replay all events = 500
```

Often paired with CQRS. Events are the source of truth.

**💡 Tip:** Event sourcing = **"keep the receipt, not the balance."** Audit trail + time travel. Replay events to reconstruct any point in time. Complex but powerful.

---

### Q45. What is the saga pattern?

**Answer:** Manage distributed transactions across multiple services without 2-phase commit:
- **Choreography**: each service emits events, next service reacts
- **Orchestration**: central coordinator tells each service what to do

Each step has a compensating action (rollback). Used when ACID isn't possible across services.

**💡 Tip:** Saga = **"distributed transaction with undo."** Each step has a compensation (undo) action. No distributed locks. Used in microservices.

---

### Q46. What is change data capture (CDC)?

**Answer:** Capture row-level changes from database transaction logs:
- **SQL**: Debezium (reads WAL/binlog)
- **NoSQL**: MongoDB Change Streams, Kafka Connect

Stream changes to: data warehouses, search indexes, caches, analytics.

**💡 Tip:** CDC = **"listen to database changes in real-time."** Replicate data to other systems without polling. Debezium is the standard tool.

---

### Q47. What is vector database and why does it matter?

**Answer:** Database optimized for storing and querying vector embeddings:
- **Use cases**: semantic search, recommendation, RAG (AI), similarity search
- **Examples**: Pinecone, Weaviate, Milvus, pgvector (PostgreSQL extension)
- **Query**: "find items most similar to this vector" (cosine similarity, L2 distance)

Growing rapidly with AI/LLM applications.

**💡 Tip:** Vector database = **"find similar things by meaning, not exact match."** pgvector adds this to PostgreSQL. Essential for AI-powered search and recommendations.

---

### Q48. What is database federation?

**Answer:** Single interface querying multiple databases:
- **SQL**: Foreign Data Wrappers (PostgreSQL), Linked Servers (SQL Server)
- **Tools**: Trino, Presto, Apache Drill

Query across SQL, NoSQL, files, APIs as if they were one database.

**💡 Tip:** Federation = **"query many databases as one."** Useful for analytics across heterogeneous data. Has performance overhead for operational queries.

---

### Q49. What is the difference between SQL and NoSQL for full-text search?

**Answer:**
- **SQL**: PostgreSQL full-text search (tsvector, tsquery, GIN index). Good for most cases.
- **NoSQL**: MongoDB text indexes, Atlas Search (Lucene-powered).
- **Specialized**: Elasticsearch, Solr, Meilisearch (best search engines)

For basic search: use built-in features. For production search: use Elasticsearch.

**💡 Tip:** Both SQL and NoSQL have basic full-text search. For production (autocomplete, fuzzy, ranking): use Elasticsearch or Atlas Search. Don't build your own search engine.

---

### Q50. How to choose between SQL and NoSQL?

**Answer:**
| Factor | Choose SQL | Choose NoSQL |
|---|---|---|
| Data structure | Fixed, tabular | Flexible, hierarchical |
| Relationships | Many complex relations | Few or embeddable |
| Transactions | Critical (ACID) | Less critical |
| Scale | Moderate | Massive horizontal |
| Development | Stable requirements | Rapid iteration |
| Queries | Complex JOINs | Simple key lookups |
| Team expertise | SQL knowledge | Document data experience |

**Best practice**: Start with SQL (PostgreSQL). Add NoSQL only when you hit specific limits. Polyglot persistence for mature systems.

**💡 Tip:** Default = **PostgreSQL** (handles most use cases). Add Redis for caching. Add Elasticsearch for search. Add MongoDB for flexible documents. Don't start with 5 databases — add as needed.

---
