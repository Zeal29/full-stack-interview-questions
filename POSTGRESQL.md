# PostgreSQL Interview Questions — Top 50

> **Difficulty Split:** Basic (Q1–Q20) | Intermediate (Q21–Q40) | Advanced (Q41–Q50)

---

## Basic Questions (Q1–Q20)

### Q1. What is PostgreSQL?

**Answer:** PostgreSQL is a powerful, open-source, object-relational database system known for reliability, feature robustness, and performance. It supports SQL (relational) and JSON (non-relational) querying, complex queries, triggers, views, and transactional integrity.

**💡 Tip:** PostgreSQL = **"world's most advanced open-source database."** ACID-compliant, extensible, handles both structured and semi-structured data. Called "Postgres" for short.

---

### Q2. What are the common data types in PostgreSQL?

**Answer:**
- **Numeric**: `INTEGER`, `BIGINT`, `DECIMAL`, `NUMERIC`, `REAL`, `SERIAL`
- **String**: `VARCHAR(n)`, `CHAR(n)`, `TEXT`
- **Date/Time**: `DATE`, `TIMESTAMP`, `TIMESTAMPTZ`, `INTERVAL`
- **Boolean**: `BOOLEAN`
- **Special**: `UUID`, `JSONB`, `ARRAY`, `HSTORE`, `BYTEA`, `CIDR`, `GEOMETRY` (PostGIS)

**💡 Tip:** Postgres has **the richest data types** of any SQL database. `JSONB` = indexed JSON. `SERIAL` = auto-increment. `TIMESTAMPTZ` = timezone-aware timestamps.

---

### Q3. What is the difference between `VARCHAR` and `TEXT`?

**Answer:**
- `VARCHAR(n)`: variable-length with max limit
- `TEXT`: variable-length with no limit

In PostgreSQL, there's **no performance difference**. Both use the same storage mechanism. Use `VARCHAR(n)` for semantic validation, `TEXT` for flexibility.

**💡 Tip:** Unlike MySQL, Postgres `TEXT` has **no performance penalty**. Use `TEXT` for flexibility, `VARCHAR(n)` only if you need a length constraint.

---

### Q4. What are PRIMARY KEY and FOREIGN KEY?

**Answer:**
- **PRIMARY KEY**: uniquely identifies each row, automatically creates unique index, NOT NULL
- **FOREIGN KEY**: references a column in another table, enforces referential integrity

```sql
CREATE TABLE users (id SERIAL PRIMARY KEY, name TEXT);
CREATE TABLE posts (id SERIAL PRIMARY KEY, user_id INTEGER REFERENCES users(id));
```

**💡 Tip:** Primary key = **unique identifier** (like CNIC). Foreign key = **link to another table** (like parent's CNIC on your form).

---

### Q5. What are the different types of JOINs?

**Answer:**
- **INNER JOIN**: rows matching in BOTH tables
- **LEFT JOIN**: all rows from LEFT + matching from RIGHT (NULL if no match)
- **RIGHT JOIN**: all rows from RIGHT + matching from LEFT
- **FULL OUTER JOIN**: all rows from BOTH tables, NULL where no match
- **CROSS JOIN**: cartesian product (every row × every row)
- **SELF JOIN**: table joins itself

**💡 Tip:** INNER = **both have it**. LEFT = **left has it, right maybe**. FULL = **either has it**. 90% of the time you use INNER or LEFT JOIN.

---

### Q6. What are indexes and why use them?

**Answer:** Indexes speed up data retrieval at the cost of slower writes and storage space:
```sql
CREATE INDEX idx_users_email ON users(email);              -- B-tree (default)
CREATE UNIQUE INDEX idx_users_email ON users(email);        -- unique
CREATE INDEX idx_posts_title ON posts USING gin(to_tsvector('english', title)); -- GIN
```

Without indexes, PostgreSQL does a **sequential scan** (reads every row).

**💡 Tip:** Index = **book index** — find a topic without reading the whole book. Add indexes on columns used in WHERE, JOIN, ORDER BY. Too many = slow writes.

---

### Q7. What is the difference between `WHERE` and `HAVING`?

**Answer:**
- `WHERE`: filters rows **before** grouping (individual rows)
- `HAVING`: filters groups **after** GROUP BY (aggregated results)

```sql
SELECT city, COUNT(*) FROM users
WHERE age > 18        -- filter individuals first
GROUP BY city
HAVING COUNT(*) > 10; -- filter groups after aggregation
```

**💡 Tip:** WHERE = **filter rows** (before GROUP BY). HAVING = **filter groups** (after GROUP BY). Execution order: WHERE → GROUP BY → HAVING.

---

### Q8. What are aggregate functions?

**Answer:** Functions that compute a single result from a set of rows:
- `COUNT()` — number of rows
- `SUM()` — total
- `AVG()` — average
- `MAX()` / `MIN()` — extremes
- `GROUP_CONCAT` (not in PG, use `STRING_AGG()`)

Always used with `GROUP BY` or over the entire result set.

**💡 Tip:** Aggregates = **math over groups**. COUNT, SUM, AVG, MAX, MIN. Must use with GROUP BY for per-group results.

---

### Q9. What are transactions and ACID properties?

**Answer:** A transaction groups operations into an all-or-nothing unit:
- **Atomicity**: all or nothing
- **Consistency**: database remains valid
- **Isolation**: transactions don't interfere
- **Durability**: committed data survives crashes

```sql
BEGIN;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
COMMIT; -- or ROLLBACK on error
```

**💡 Tip:** ACID = **A**ll-or-nothing, **C**onsistent, **I**solated, **D**urable. Use `BEGIN/COMMIT/ROLLBACK` for multi-step operations.

---

### Q10. What is `SERIAL` vs `IDENTITY`?

**Answer:**
- `SERIAL`: older auto-increment, actually creates a sequence + sets default
- `GENERATED ALWAYS AS IDENTITY`: modern (PG 10+), SQL-standard, more secure

```sql
-- Old
id SERIAL PRIMARY KEY
-- New (recommended)
id INTEGER GENERATED ALWAYS AS IDENTITY PRIMARY KEY
```

**💡 Tip:** Use `IDENTITY` in new projects (SQL standard, can't accidentally override). `SERIAL` is legacy but still common.

---

### Q11. What is the `COALESCE` function?

**Answer:** Returns the first non-NULL value:
```sql
SELECT COALESCE(phone, email, 'N/A') AS contact FROM users;
```

Common for handling NULLs in queries and reports.

**💡 Tip:** `COALESCE` = **"give me the first real value."** Perfect for fallback values. `COALESCE(null, null, 'default')` → `'default'`.

---

### Q12. What are `NULL` values and how to handle them?

**Answer:** NULL = missing/unknown value (not zero, not empty string):
- `NULL = NULL` → NULL (not TRUE!) — use `IS NULL`
- `NULL + 1` → NULL (any operation with NULL = NULL)
- Use `COALESCE()`, `IS NULL`, `IS NOT NULL`
- `ISNULL` / `IFNULL` (not standard — use `COALESCE`)

**💡 Tip:** NULL = **unknown**, not zero or empty. Comparisons with NULL always return NULL (not true/false). Always use `IS NULL` / `IS NOT NULL`.

---

### Q13. What is a VIEW?

**Answer:** A named query stored in the database (virtual table):
```sql
CREATE VIEW active_users AS
  SELECT id, name, email FROM users WHERE active = true;
-- Use like a table
SELECT * FROM active_users;
```

Simplifies complex queries, provides abstraction, can restrict access.

**💡 Tip:** View = **saved query that acts like a table**. Simplifies complexity, adds security layer. Doesn't store data — runs the query each time.

---

### Q14. What are `LIMIT` and `OFFSET`?

**Answer:** Pagination in queries:
```sql
SELECT * FROM users ORDER BY created_at DESC LIMIT 10 OFFSET 20; -- page 3, 10 per page
```

`LIMIT` = max rows returned. `OFFSET` = rows to skip. For large datasets, use keyset pagination instead (`WHERE id > last_id LIMIT 10`).

**💡 Tip:** LIMIT + OFFSET = **simple but slow** for deep pages (offset 100000 scans 100000 rows). Keyset pagination (`WHERE id > last_id`) is faster for large datasets.

---

### Q15. What are constraints in PostgreSQL?

**Answer:** Rules enforced on columns:
- `NOT NULL` — value required
- `UNIQUE` — no duplicates
- `PRIMARY KEY` — unique + not null
- `FOREIGN KEY` — references another table
- `CHECK` — custom condition (`age >= 0`)
- `DEFAULT` — default value

**💡 Tip:** Constraints = **database-level validation**. Don't rely only on app validation — constraints catch bad data from any source.

---

### Q16. What is `JSONB` in PostgreSQL?

**Answer:** Binary JSON storage with indexing support:
```sql
CREATE TABLE products (id SERIAL, data JSONB);
INSERT INTO products VALUES (1, '{"name": "Laptop", "price": 999}');
SELECT data->>'name' FROM products WHERE data @> '{"name": "Laptop"}';
CREATE INDEX idx_data ON products USING gin(data);
```

`JSON` = text storage (no indexing). `JSONB` = binary (indexable, faster).

**💡 Tip:** `JSONB` = **best of both worlds** (SQL + NoSQL). Use when you have flexible schema data. Create GIN indexes for JSONB queries.

---

### Q17. What are triggers?

**Answer:** Functions that automatically execute on table events:
```sql
CREATE FUNCTION update_timestamp() RETURNS TRIGGER AS $$
BEGIN NEW.updated_at = NOW(); RETURN NEW; END; $$ LANGUAGE plpgsql;
CREATE TRIGGER set_timestamp BEFORE UPDATE ON users
  FOR EACH ROW EXECUTE FUNCTION update_timestamp();
```

Events: BEFORE/AFTER INSERT/UPDATE/DELETE.

**💡 Tip:** Triggers = **automatic actions on data changes**. Use for timestamps, audit logs, data validation. Don't overuse — hard to debug.

---

### Q18. What are stored procedures and functions?

**Answer:**
- **Functions**: return values, used in queries (`SELECT my_func()`)
- **Procedures**: perform actions, can manage transactions (`CALL my_proc()`)

```sql
CREATE FUNCTION get_user_count() RETURNS INTEGER AS $$
BEGIN RETURN (SELECT COUNT(*) FROM users); END;
$$ LANGUAGE plpgsql;
```

**💡 Tip:** Functions = **compute and return** (in SELECT). Procedures = **do something** (with CALL). Keep logic in the app — use DB functions for performance-critical operations only.

---

### Q19. What is the `EXPLAIN` command?

**Answer:** Shows the query execution plan:
```sql
EXPLAIN ANALYZE SELECT * FROM users WHERE email = 'ali@test.com';
-- Shows: scan type, index usage, estimated cost, actual time
```

Seq Scan = reads all rows (slow). Index Scan = uses index (fast). Use to optimize slow queries.

**💡 Tip:** `EXPLAIN ANALYZE` = **X-ray for queries**. Look for: Seq Scan (bad on large tables), Index Scan (good), high cost numbers. Always analyze slow queries.

---

### Q20. What is the `psql` command-line tool?

**Answer:** PostgreSQL's interactive terminal:
- `\l` — list databases
- `\dt` — list tables
- `\d table_name` — describe table
- `\c dbname` — connect to database
- `\q` — quit

**💡 Tip:** `psql` = **PostgreSQL command line**. `\d table` = most useful (shows columns, types, indexes, constraints).

---

## Intermediate Questions (Q21–Q40)

### Q21. What are window functions?

**Answer:** Perform calculations across related rows without grouping:
```sql
SELECT name, salary,
  RANK() OVER (ORDER BY salary DESC) as rank,
  SUM(salary) OVER (PARTITION BY dept) as dept_total,
  LAG(salary, 1) OVER (ORDER BY salary) as prev_salary
FROM employees;
```

Functions: `ROW_NUMBER`, `RANK`, `DENSE_RANK`, `LEAD`, `LAG`, `SUM`, `AVG` over windows.

**💡 Tip:** Window functions = **aggregation without collapsing rows**. `OVER (PARTITION BY x ORDER BY y)` defines the window. Doesn't reduce row count like GROUP BY.

---

### Q22. What are Common Table Expressions (CTEs)?

**Answer:** Named temporary result sets:
```sql
WITH active_users AS (
  SELECT * FROM users WHERE last_login > NOW() - INTERVAL '30 days'
),
user_stats AS (
  SELECT user_id, COUNT(*) as post_count FROM posts GROUP BY user_id
)
SELECT au.name, us.post_count FROM active_users au
LEFT JOIN user_stats us ON au.id = us.user_id;
```

Recursive CTEs can traverse hierarchical data (org charts, trees).

**💡 Tip:** CTEs = **named subqueries** for readability. Recursive CTEs = **tree traversal**. Use for complex queries — makes them readable.

---

### Q23. What is the difference between `DELETE` and `TRUNCATE`?

**Answer:**
- `DELETE`: row-by-row, can use WHERE, triggers fire, can be rolled back, slower
- `TRUNCATE`: deallocates entire table, no WHERE, no triggers (by default), much faster, resets sequences

```sql
DELETE FROM logs WHERE created_at < '2024-01-01'; -- specific rows
TRUNCATE TABLE logs;                              -- all rows, fast
```

**💡 Tip:** DELETE = **surgical removal** (specific rows). TRUNCATE = **nuke the table** (all rows, instant). Use TRUNCATE for clearing staging tables.

---

### Q24. What are database locks and deadlocks?

**Answer:**
- **Row locks**: acquired on rows being modified (UPDATE, DELETE, SELECT FOR UPDATE)
- **Table locks**: acquired by ALTER, DROP, VACUUM
- **Deadlock**: two transactions each waiting for the other's lock

PostgreSQL auto-detects deadlocks and aborts one transaction. Use short transactions and consistent lock ordering to prevent.

**💡 Tip:** Deadlock = **two people holding doors, each waiting for the other**. PostgreSQL kills one after 1 second. Prevent with: short transactions, consistent access order.

---

### Q25. What are isolation levels in PostgreSQL?

**Answer:**
- **Read Uncommitted**: not actually supported (acts like Read Committed)
- **Read Committed** (default): sees only committed data, each statement gets fresh snapshot
- **Repeatable Read**: same snapshot for entire transaction, detects serialization failures
- **Serializable**: strictest, full serialization, can fail with concurrent modifications

**💡 Tip:** Default = **Read Committed** (good for most cases). Use **Repeatable Read** for reports needing consistency. Serializable = strictest but highest failure rate.

---

### Q26. What is `VACUUM` in PostgreSQL?

**Answer:** Reclaims storage from dead rows (UPDATE/DELETE don't immediately free space):
- **Auto-vacuum**: runs automatically (default on)
- `VACUUM`: marks space reusable, doesn't shrink file
- `VACUUM FULL`: rebuilds table (shrinks file, locks table)

Dead rows exist because of MVCC (Multi-Version Concurrency Control).

**💡 Tip:** VACUUM = **garbage collection for tables**. Auto-vacuum handles it. `VACUUM FULL` = expensive (locks table). Monitor dead tuples in production.

---

### Q27. What is MVCC?

**Answer:** Multi-Version Concurrency Control — PostgreSQL's approach to concurrency. Instead of overwriting rows, it creates new versions:
- Writers don't block readers
- Readers don't block writers
- Each transaction sees a consistent snapshot

Old row versions are cleaned up by VACUUM.

**💡 Tip:** MVCC = **"everyone sees their own version of reality."** Writers create new versions, readers see old versions. No locks between readers and writers.

---

### Q28. What are B-tree, Hash, GIN, and GiST indexes?

**Answer:**
- **B-tree** (default): general purpose, equality + range queries (`>`, `<`, `BETWEEN`)
- **Hash**: equality only (rarely used, B-tree is usually better)
- **GIN**: full-text search, JSONB, arrays (multiple elements per row)
- **GiST**: geometric data, full-text search, range types
- **BRIN**: for naturally ordered data (time-series), very small index

**💡 Tip:** Default = **B-tree** (covers 95%). GIN = **JSONB/arrays/full-text**. BRIN = **time-series**. Pick based on your query patterns.

---

### Q29. What are materialized views?

**Answer:** Cached query results stored on disk:
```sql
CREATE MATERIALIZED VIEW user_stats AS
  SELECT user_id, COUNT(*), SUM(amount) FROM orders GROUP BY user_id;
-- Refresh manually
REFRESH MATERIALIZED VIEW CONCURRENTLY user_stats;
```

Unlike regular views, stores actual data. Great for expensive analytical queries.

**💡 Tip:** Materialized view = **cached view**. Stores data (fast reads, stale until refreshed). Refresh periodically or after data changes. Use for dashboards/reports.

---

### Q30. What are connection pooling and PgBouncer?

**Answer:** PostgreSQL connections are expensive (forks a process). PgBouncer maintains a pool of reusable connections:
- **Session pooling**: one connection per client session
- **Transaction pooling**: connection returned to pool after each transaction (most efficient)
- **Statement pooling**: returned after each statement

**💡 Tip:** PgBouncer = **connection multiplexer**. Reduces connections from 1000 clients to 20 real DB connections. Always use in production.

---

### Q31. What is the `UPSERT` (INSERT ON CONFLICT)?

**Answer:** Insert or update if conflict:
```sql
INSERT INTO users (email, name) VALUES ('ali@test.com', 'Ali')
ON CONFLICT (email) DO UPDATE SET name = EXCLUDED.name;
```

`EXCLUDED` refers to the row that was proposed for insertion.

**💡 Tip:** `ON CONFLICT DO UPDATE` = **insert or update** (UPSERT). `ON CONFLICT DO NOTHING` = **insert or skip**. Based on unique constraint/primary key.

---

### Q32. What are advisory locks?

**Answer:** Application-level locks not tied to table rows:
```sql
SELECT pg_advisory_lock(12345);  -- acquire
SELECT pg_advisory_unlock(12345); -- release
```

Useful for: distributed locking, preventing concurrent batch jobs, leader election.

**💡 Tip:** Advisory locks = **custom locks for your app logic**. Not tied to data. Use for: one-at-a-time cron jobs, distributed coordination.

---

### Q33. What is full-text search in PostgreSQL?

**Answer:** Built-in text search with `tsvector` and `tsquery`:
```sql
CREATE INDEX idx_docs_search ON documents USING gin(to_tsvector('english', content));
SELECT * FROM documents WHERE to_tsvector('english', content) @@ to_tsquery('english', 'database & performance');
```

`tsvector` = preprocessed text. `tsquery` = search query. GIN index for speed.

**💡 Tip:** Full-text search = **Google-lite inside Postgres**. Create GIN index on `tsvector`. `@@` = match operator. Good for most apps — only use Elasticsearch for massive scale.

---

### Q34. What is the `pg_stat_statements` extension?

**Answer:** Tracks execution statistics for all SQL statements:
```sql
CREATE EXTENSION pg_stat_statements;
SELECT query, calls, total_exec_time, mean_exec_time, rows
FROM pg_stat_statements ORDER BY total_exec_time DESC LIMIT 10;
```

Shows: most expensive queries, most frequent queries, slowest queries.

**💡 Tip:** `pg_stat_statements` = **query performance dashboard**. Find slow queries, most-run queries, and optimization targets. Must enable in config.

---

### Q35. What are partitioned tables?

**Answer:** Split large tables into smaller, manageable pieces:
```sql
CREATE TABLE logs (id SERIAL, created_at TIMESTAMPTZ, message TEXT)
  PARTITION BY RANGE (created_at);
CREATE TABLE logs_2024_q1 PARTITION OF logs FOR VALUES FROM ('2024-01-01') TO ('2024-04-01');
```

Types: RANGE (dates, numbers), LIST (categories), HASH (even distribution). Queries scan only relevant partitions.

**💡 Tip:** Partitioning = **split big table into smaller tables**. Range = by date/number. List = by category. Queries skip irrelevant partitions = faster.

---

### Q36. What is the `LATERAL` join?

**Answer:** Subquery that can reference columns from preceding tables:
```sql
SELECT u.name, recent_posts.*
FROM users u
CROSS JOIN LATERAL (
  SELECT * FROM posts WHERE user_id = u.id ORDER BY created_at DESC LIMIT 3
) recent_posts;
```

Like a for-each loop — runs subquery for each row.

**💡 Tip:** `LATERAL` = **"for each row, run this subquery."** Get top-N per group, apply function per row. Think of it as a SQL loop.

---

### Q37. What is a cursor?

**Answer:** Allows processing query results one row (or batch) at a time:
```sql
BEGIN;
DECLARE user_cursor CURSOR FOR SELECT * FROM users;
FETCH 100 FROM user_cursor; -- get 100 rows
FETCH 100 FROM user_cursor; -- next 100 rows
CLOSE user_cursor;
COMMIT;
```

Useful for processing large datasets without loading everything into memory.

**💡 Tip:** Cursor = **row-by-row iterator**. For large datasets, process in batches instead of loading all at once. Drivers have built-in cursor support.

---

### Q38. What are foreign data wrappers (FDW)?

**Answer:** Query external data sources as if they were local tables:
```sql
CREATE EXTENSION postgres_fdw;
CREATE SERVER foreign_db FOREIGN DATA WRAPPER postgres_fdw OPTIONS (host 'remote-host', dbname 'other_db');
CREATE FOREIGN TABLE remote_users (id INT, name TEXT) SERVER foreign_db;
SELECT * FROM remote_users; -- query remote database
```

Connect to: other PostgreSQL, MySQL, MongoDB, CSV, REST APIs.

**💡 Tip:** FDW = **query other databases as local tables**. Read from legacy systems, migrate data, federate queries. Has performance overhead.

---

### Q39. What is the `RETURNING` clause?

**Answer:** Returns data from modified rows:
```sql
INSERT INTO users (name) VALUES ('Ali') RETURNING id, name;
UPDATE users SET name = 'Ahmed' WHERE id = 1 RETURNING *;
DELETE FROM users WHERE id = 1 RETURNING id;
```

Saves a round trip — no need for a separate SELECT after INSERT/UPDATE/DELETE.

**💡 Tip:** `RETURNING` = **"tell me what you just did."** Get auto-generated IDs, confirm changes, audit deletes — all in one query.

---

### Q40. What are sequences in PostgreSQL?

**Answer:** Auto-incrementing number generators:
```sql
CREATE SEQUENCE user_id_seq START 1;
SELECT nextval('user_id_seq'); -- 1
SELECT currval('user_id_seq'); -- 1 (current session)
SELECT setval('user_id_seq', 100); -- set to 100
```

Used internally by `SERIAL` and `IDENTITY` columns.

**💡 Tip:** Sequence = **counter in the database**. Thread-safe, gap-tolerant (numbers may skip). Gaps are normal — don't rely on contiguous IDs.

---

## Advanced Questions (Q41–Q50)

### Q41. What are logical replication and physical replication?

**Answer:**
- **Physical replication**: replicates block-level changes (WAL shipping). Replica is exact copy, read-only.
- **Logical replication**: replicates row-level changes (INSERT/UPDATE/DELETE). Can replicate specific tables, transform data.

Logical = flexible (partial replication, different versions). Physical = simpler, lower overhead.

**💡 Tip:** Physical = **byte-for-byte copy** (simple, all or nothing). Logical = **row-by-row** (flexible, specific tables). Use logical for selective sync.

---

### Q42. What is the Write-Ahead Log (WAL)?

**Answer:** PostgreSQL writes changes to WAL **before** writing to data files. On crash, replays WAL to recover. Also used for replication. Ensures durability (the D in ACID).

**💡 Tip:** WAL = **journal before writing**. Crash → replay journal → recover. Like saving game before a boss fight. Enables replication and point-in-time recovery.

---

### Q43. What is point-in-time recovery (PITR)?

**Answer:** Restore database to any point in time using WAL archives:
1. Restore base backup
2. Replay WAL up to target time
3. Database state = exactly that moment

Essential for: recovering from accidental DROP, data corruption, compliance.

**💡 Tip:** PITR = **time travel for your database**. Need WAL archiving enabled. Recover to `2024-03-15 14:30:00` — exactly that moment.

---

### Q44. What are tablespaces?

**Answer:** Storage locations for database objects:
```sql
CREATE TABLESPACE fast_storage LOCATION '/ssd/disk';
CREATE TABLE hot_data (id INT, data TEXT) TABLESPACE fast_storage;
```

Move busy tables to fast SSD, archive tables to slow HDD. Most deployments use the default tablespace.

**💡 Tip:** Tablespaces = **control where data lives physically**. Put hot data on SSD, cold data on HDD. Most apps don't need custom tablespaces.

---

### Q45. How does PostgreSQL handle concurrency without read locks?

**Answer:** MVCC creates row versions with visibility rules based on transaction IDs (xmin/xmax):
- Each row has `xmin` (creating transaction) and `xmax` (deleting transaction)
- A transaction sees rows where `xmin` ≤ my ID and (`xmax` is null or > my ID)
- No reader locks needed — readers never block writers

**💡 Tip:** Each row has "born on" (xmin) and "died on" (xmax) timestamps. Transactions only see rows alive during their snapshot. No read locks!

---

### Q46. What is the `pg_dump` and `pg_restore` tool?

**Answer:** Backup and restore:
```bash
pg_dump -U postgres -d mydb -F c -f backup.dump  # custom format backup
pg_restore -U postgres -d newdb backup.dump        # restore
pg_dump -U postgres mydb > backup.sql              # plain SQL backup
```

For live replicas, use streaming replication. `pg_dump` is for point-in-time snapshots.

**💡 Tip:** `pg_dump` = **snapshot backup**. Use `-F c` (custom format) for parallel restore. Schedule daily backups. For zero-downtime, use streaming replication.

---

### Q47. What is the `event_trigger`?

**Answer:** Triggers on DDL events (CREATE, ALTER, DROP):
```sql
CREATE FUNCTION prevent_drop() RETURNS event_trigger AS $$
BEGIN
  RAISE EXCEPTION 'Dropping tables is not allowed';
END; $$ LANGUAGE plpgsql;
CREATE EVENT TRIGGER no_drop ON ddl_command_end
  WHEN TAG IN ('DROP TABLE') EXECUTE FUNCTION prevent_drop();
```

**💡 Tip:** Event triggers = **guardrails for schema changes**. Prevent accidental drops, audit all DDL, enforce naming conventions.

---

### Q48. What is the `EXCLUDE` constraint?

**Answer:** Prevents overlapping ranges (like double-booking):
```sql
CREATE TABLE bookings (
  room INT, during TSTZRANGE,
  EXCLUDE USING gist (room WITH =, during WITH &&)
);
-- Prevents: same room, overlapping time ranges
```

Uses GiST index. `&&` = range overlap operator.

**💡 Tip:** EXCLUDE constraint = **"no overlaps"** constraint. Perfect for booking systems — prevents double-booking rooms, appointments.

---

### Q49. What are generated columns?

**Answer:** Columns computed from other columns (virtual or stored):
```sql
CREATE TABLE products (
  price DECIMAL,
  tax_rate DECIMAL DEFAULT 0.1,
  total DECIMAL GENERATED ALWAYS AS (price * (1 + tax_rate)) STORED
);
```

`STORED` = physically stored. Virtual (not stored) coming in future PG versions.

**💡 Tip:** Generated columns = **calculated fields**. Auto-updated when source columns change. Like Excel formulas in your database.

---

### Q50. How to optimize PostgreSQL performance?

**Answer:**
- **Indexes**: B-tree for equality/range, GIN for JSONB/arrays, BRIN for time-series
- **EXPLAIN ANALYZE**: find slow query plans
- **Connection pooling**: PgBouncer
- **VACUUM**: auto-vacuum tuning
- **Partitioning**: split large tables
- **Materialized views**: cache expensive queries
- **Configuration**: `shared_buffers`, `work_mem`, `effective_cache_size`
- **Query optimization**: avoid SELECT *, use LIMIT, batch INSERTs

**💡 Tip:** Optimization = **measure first, optimize second.** EXPLAIN ANALYZE → index → config → partition. Don't guess — measure.

---
