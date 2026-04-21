# Database Normalization & Indexing Interview Questions — Top 50

> **Difficulty Split:** Basic (Q1–Q20) | Intermediate (Q21–Q40) | Advanced (Q41–Q50)

---

## Basic Questions (Q1–Q20)

### Q1. What is database normalization?

**Answer:** Normalization is the process of organizing data to reduce redundancy and dependency by dividing tables into smaller, related tables. Goals: eliminate data anomalies (insert, update, delete), ensure data integrity, and minimize storage.

**💡 Tip:** Normalization = **"don't repeat yourself"** for databases. Split big messy tables into small, focused ones linked by relationships.

---

### Q2. What are the normal forms?

**Answer:**
- **1NF**: atomic values, no repeating groups, unique rows
- **2NF**: 1NF + no partial dependencies (every non-key column depends on the whole key)
- **3NF**: 2NF + no transitive dependencies (non-key columns don't depend on other non-key columns)
- **BCNF**: stricter 3NF (every determinant is a candidate key)
- **4NF**: no multi-valued dependencies
- **5NF**: no join dependencies

Most practical applications normalize to **3NF**.

**💡 Tip:** 1NF = **atomic**. 2NF = **whole key**. 3NF = **only the key**. Classic mnemonic: "Every non-key attribute must provide a fact about the key, the whole key, and nothing but the key, so help me Codd."

---

### Q3. What is First Normal Form (1NF)?

**Answer:** Rules: (1) Each cell contains a single (atomic) value, (2) Each row is unique (has a primary key), (3) No repeating groups or arrays.

**Bad**: `{ id: 1, name: "Ali", phones: "0321, 0345" }` — multiple values in one cell
**Good**: Two rows: `{ id: 1, name: "Ali", phone: "0321" }` and `{ id: 1, name: "Ali", phone: "0345" }`

**💡 Tip:** 1NF = **one value per cell, ever**. No comma-separated lists, no arrays. Each piece of data gets its own cell.

---

### Q4. What is Second Normal Form (2NF)?

**Answer:** 1NF + no **partial dependencies** (non-key columns depend on the ENTIRE primary key, not just part of it). Only applies to tables with composite primary keys.

**Bad**: `{ student_id, course_id, student_name, course_name }` — `student_name` depends only on `student_id` (partial)
**Good**: Split into `students(student_id, name)`, `courses(course_id, name)`, `enrollments(student_id, course_id)`

**💡 Tip:** 2NF = **"non-key columns depend on the WHOLE key."** If a composite key exists, no column should depend on just one part.

---

### Q5. What is Third Normal Form (3NF)?

**Answer:** 2NF + no **transitive dependencies** (non-key columns don't depend on other non-key columns).

**Bad**: `{ id, name, dept_id, dept_name }` — `dept_name` depends on `dept_id` (not directly on `id`)
**Good**: Split into `employees(id, name, dept_id)` and `departments(dept_id, dept_name)`

**💡 Tip:** 3NF = **"non-key depends on key, nothing else."** `A → B → C` (transitive) is bad. Split so everything depends directly on the key.

---

### Q6. What is denormalization?

**Answer:** Intentionally adding redundancy to improve read performance. Trade read speed for write complexity and storage. Common in: data warehouses, reporting databases, read-heavy workloads.

Example: storing `order_total` in the orders table (can be computed from order_items).

**💡 Tip:** Denormalize = **"repeat yourself for speed."** Normalize for OLTP (writes), denormalize for OLAP (reads/analytical). Most apps: normalize for storage, denormalize for caching.

---

### Q7. What is a database index?

**Answer:** A data structure that improves query speed by providing quick lookup. Like a book index — find the topic without reading every page. Trades: faster reads for slower writes and more storage.

**💡 Tip:** Index = **book index**. Without it, read every page (sequential scan). With it, jump to the right page (index scan). Add on frequently queried columns.

---

### Q8. What types of indexes exist?

**Answer:**
- **B-tree** (default): balanced tree, good for equality + range (`=`, `>`, `<`, `BETWEEN`)
- **Hash**: equality only (`=`), rare
- **Bitmap**: low-cardinality columns (gender, status)
- **GIN**: full-text search, JSONB, arrays (PostgreSQL)
- **GiST**: geometric, range types (PostgreSQL)
- **Clustered**: data physically sorted by index (SQL Server, MySQL InnoDB PK)
- **Non-clustered**: separate structure pointing to data

**💡 Tip:** B-tree = **default, covers 95%**. GIN = **JSON/arrays**. Clustered = **data IS the index**. Know which your database uses by default.

---

### Q9. What is a clustered vs non-clustered index?

**Answer:**
- **Clustered**: determines physical storage order of data. One per table. The table IS the index. (MySQL InnoDB PK, SQL Server clustered index)
- **Non-clustered**: separate structure with pointers to data rows. Multiple per table.

PostgreSQL doesn't have true clustered indexes — uses heap storage with non-clustered indexes.

**💡 Tip:** Clustered = **data sorted by index** (one per table). Non-clustered = **pointer map to data** (many per table). PostgreSQL always uses non-clustered.

---

### Q10. What is a composite index?

**Answer:** Index on multiple columns:
```sql
CREATE INDEX idx_user_status ON orders(user_id, status);
```

**Leftmost prefix rule**: can use for `(user_id)`, `(user_id, status)` but NOT for `(status)` alone. Column order matters!

**💡 Tip:** Composite index = **multi-column index**. Order matters! Query must use leftmost columns first. `INDEX(a, b, c)` helps `(a)`, `(a,b)`, `(a,b,c)` but NOT `(b)` or `(c)`.

---

### Q11. What is a unique index?

**Answer:** Ensures no duplicate values in the indexed column(s):
```sql
CREATE UNIQUE INDEX idx_users_email ON users(email);
```

Automatically created for PRIMARY KEY and UNIQUE constraints. Prevents duplicate data at the database level.

**💡 Tip:** Unique index = **"no duplicates allowed"** + fast lookup. Use for emails, usernames, slugs. Always enforce uniqueness at the DB level, not just in app code.

---

### Q12. What is a covering index?

**Answer:** An index that contains ALL columns needed by a query — no need to read the actual table:
```sql
CREATE INDEX idx_covering ON orders(user_id, status, total);
-- Query: SELECT status, total FROM orders WHERE user_id = 1;
-- Index-only scan — never reads the table!
```

**💡 Tip:** Covering index = **"query answered entirely by the index."** Fastest possible query. Include all SELECT columns in the index. Look for "Index Only Scan" in EXPLAIN.

---

### Q13. What is an index scan vs table scan?

**Answer:**
- **Index scan**: uses index to find rows (fast, O(log n))
- **Table/Sequential scan**: reads every row (slow, O(n))

Database chooses based on: estimated rows, index availability, statistics. Sometimes sequential scan is faster for small tables or queries returning most rows.

**💡 Tip:** Index scan = **use the index** (fast for selective queries). Table scan = **read everything** (faster when returning >5-10% of rows). Not always bad!

---

### Q14. What is the cost of indexing?

**Answer:** Indexes aren't free:
- **Slower writes**: INSERT/UPDATE/DELETE must update all indexes
- **Storage**: indexes take disk space (can be 50%+ of table size)
- **Maintenance**: REINDEX, ANALYZE, VACUUM needed
- **Memory**: indexes cached in RAM (competing with data)

**💡 Tip:** Indexes = **read speed tax on writes**. Each index = slower INSERT/UPDATE. Only index columns used in WHERE, JOIN, ORDER BY. Remove unused indexes.

---

### Q15. What is a foreign key constraint?

**Answer:** Ensures referential integrity between tables:
```sql
CREATE TABLE orders (
  id SERIAL PRIMARY KEY,
  user_id INTEGER REFERENCES users(id) ON DELETE CASCADE
);
```

Options: `CASCADE` (delete children), `SET NULL`, `RESTRICT` (prevent delete), `NO ACTION`.

**💡 Tip:** Foreign key = **"this value must exist in the other table."** `ON DELETE CASCADE` = delete parent → children auto-deleted. Always use FKs for relationships.

---

### Q16. What is a candidate key vs primary key?

**Answer:**
- **Candidate key**: any column (or set) that could uniquely identify rows
- **Primary key**: the chosen candidate key (only one per table)
- **Alternate key**: candidate keys not chosen as primary

Example: `email` and `id` are both candidate keys. Choose `id` as primary, `email` is alternate key.

**💡 Tip:** Candidate = **could be the key**. Primary = **chosen key**. A table has many candidates but only one primary. Pick the shortest, simplest one.

---

### Q17. What is a surrogate key vs natural key?

**Answer:**
- **Surrogate key**: artificial, generated (SERIAL, UUID) — no business meaning
- **Natural key**: real-world identifier (email, SSN, ISBN)

Surrogate keys are recommended: stable, simple, never changes. Natural keys can change (people change emails).

**💡 Tip:** Surrogate = **made-up ID** (recommended). Natural = **real-world ID** (risky — can change). Always prefer surrogate keys (SERIAL, UUID).

---

### Q18. What are database anomalies?

**Answer:** Problems in unnormalized data:
- **Insert anomaly**: can't insert data without unrelated data (can't add dept without an employee)
- **Update anomaly**: updating one row requires updating multiple rows (inconsistent if missed)
- **Delete anomaly**: deleting one row unintentionally removes other data

**💡 Tip:** Anomalies = **data mess from bad design.** Insert = "can't save without full data." Update = "same fact in two places." Delete = "lose unrelated info." Normalization fixes these.

---

### Q19. What is a partial dependency?

**Answer:** When a non-key column depends on only PART of a composite primary key:
```
Table: (student_id, course_id, student_name)
student_name depends only on student_id (not the full key)
```

This violates 2NF. Fix: split into separate tables.

**💡 Tip:** Partial dependency = **"this column only needs HALF the key."** Only happens with composite keys. Split the table to fix.

---

### Q20. What is a transitive dependency?

**Answer:** When a non-key column depends on another non-key column:
```
Table: (id, name, zip_code, city)
city depends on zip_code (not directly on id)
```

This violates 3NF. Fix: create a separate `zip_codes` table.

**💡 Tip:** Transitive dependency = **A → B → C** (chain). `zip → city`, not `id → city` directly. Break the chain with separate tables.

---

## Intermediate Questions (Q21–Q40)

### Q21. What is Boyce-Codd Normal Form (BCNF)?

**Answer:** Stricter version of 3NF where **every determinant must be a candidate key**. Catches edge cases where 3NF is satisfied but anomalies still exist.

Example: `{ student, subject, teacher }` where each subject has one teacher but a teacher can teach multiple subjects. `(student, subject) → teacher` and `teacher → subject`. BCNF violated because teacher is a determinant but not a candidate key.

**💡 Tip:** BCNF = **"3NF but stricter."** Every determinant = candidate key. Rare edge cases in practice. Most apps stop at 3NF.

---

### Q22. What is the difference between a dense and sparse index?

**Answer:**
- **Dense index**: has an entry for EVERY row in the table (B-tree, most common)
- **Sparse index**: has entries for only SOME rows (one per block/page)

Dense = faster lookup (direct pointer). Sparse = smaller size (one entry per data block).

**💡 Tip:** Dense = **every row has a pointer**. Sparse = **every block has a pointer**. B-tree indexes are dense. BRIN indexes are sparse.

---

### Q23. What is index cardinality?

**Answer:** Number of unique values in a column:
- **High cardinality**: email, UUID (nearly all unique) — index very effective
- **Low cardinality**: gender, boolean (few unique values) — index less effective
- **Medium cardinality**: city, category — moderate effectiveness

**💡 Tip:** Cardinality = **"how many unique values?"** High = index helps a lot. Low = index doesn't help much (consider bitmap or skip index). Rule: index high-cardinality columns.

---

### Q24. When should you NOT use an index?

**Answer:** Skip indexing when:
- Small tables (< 1000 rows) — sequential scan is fast enough
- Columns returning >5-10% of rows — index overhead > benefit
- Frequently updated columns — write cost too high
- Columns rarely used in queries — wasted storage
- Low cardinality + no range queries — minimal benefit

**💡 Tip:** Don't index = **"it hurts more than helps."** Small tables, high-return queries, write-heavy columns. Use EXPLAIN to verify index actually helps.

---

### Q25. What is a functional/expression index?

**Answer:** Index on a computed expression:
```sql
CREATE INDEX idx_lower_email ON users(LOWER(email));
-- Now this uses the index:
SELECT * FROM users WHERE LOWER(email) = 'ali@test.com';
```

Also: computed columns, function results. PostgreSQL supports expression indexes natively.

**💡 Tip:** Functional index = **"index the transformation, not just the column."** Case-insensitive search, date extraction, calculations. Essential for transformed queries.

---

### Q26. What is index fragmentation?

**Answer:** Index pages become scattered over time due to inserts, updates, deletes:
- **Logical fragmentation**: index pages out of order
- **Page splits**: new data doesn't fit, page splits in two

Fix: `REINDEX`, `VACUUM`, or index rebuild. Monitor with database-specific tools.

**💡 Tip:** Fragmentation = **index pages scattered on disk**. Slows reads (more disk seeks). Rebuild periodically (REINDEX in PostgreSQL, ALTER INDEX REBUILD in SQL Server).

---

### Q27. What is the difference between 4NF and 5NF?

**Answer:**
- **4NF**: no multi-valued dependencies. An entity shouldn't have two independent multi-valued attributes.

Example violation: `(student_id, sport, language)` — sports and languages are independent but stored together.

- **5NF**: no join dependencies. Table can't be reconstructed by joining smaller tables without gaining spurious rows.

**💡 Tip:** 4NF = **"don't mix independent lists."** 5NF = **"decompose fully."** Both rare in practice. Most apps stop at 3NF/BCNF.

---

### Q28. What is a star schema?

**Answer:** Data warehouse design with:
- **Fact table**: central table with metrics (sales amount, quantity)
- **Dimension tables**: surround fact table with descriptive data (time, product, customer)

Denormalized dimensions for fast analytical queries. Named for its star-like shape.

**💡 Tip:** Star schema = **one fact in the center, dimensions around it.** OLAP (analytics). Fact = numbers. Dimensions = descriptions. Denormalized for query speed.

---

### Q29. What is a snowflake schema?

**Answer:** Extension of star schema where dimension tables are normalized (split into sub-dimensions):
```
Fact → Product Dimension → Category Dimension → Department Dimension
```

More normalized than star, less redundant, but more JOINs (slower queries).

**💡 Tip:** Snowflake = **star with normalized dimensions** (branches like snowflake). Less redundancy but more JOINs. Star schema is preferred for most data warehouses.

---

### Q30. What is an inverted index?

**Answer:** Maps content to its location — used for full-text search:
```
"mongodb" → [doc1, doc3, doc7]
"database" → [doc1, doc2, doc5]
```

For each word, stores which documents contain it. Enables fast text search. Used by Elasticsearch, GIN indexes in PostgreSQL.

**💡 Tip:** Inverted index = **"word → where it appears."** Like a book index (topic → page numbers). Core technology behind search engines.

---

### Q31. What is a multi-valued dependency?

**Answer:** When two attributes are independent of each other but both depend on the same key:
```
Student 101: plays Cricket, Football AND speaks English, Urdu
→ 4 rows needed: (101, Cricket, English), (101, Cricket, Urdu), (101, Football, English), (101, Football, Urdu)
```

This is a multi-valued dependency. 4NF says: split into separate tables.

**💡 Tip:** Multi-valued = **"two independent lists for one entity."** Sports and languages don't relate to each other. Put them in separate tables.

---

### Q32. What are redundant indexes?

**Answer:** Indexes that duplicate the work of other indexes:
```sql
INDEX (a, b, c)     -- covers these queries
INDEX (a, b)         -- REDUNDANT — already covered by first index (leftmost prefix)
INDEX (a)            -- REDUNDANT — already covered by first index
```

Remove redundant indexes — they waste storage and slow writes without helping reads.

**💡 Tip:** `INDEX(a,b,c)` makes `INDEX(a,b)` and `INDEX(a)` redundant. Remove them! Saves write overhead and storage.

---

### Q33. What is the difference between DELETE and TRUNCATE regarding indexes?

**Answer:**
- `DELETE`: removes rows one by one, indexes updated per row. Can fragment indexes.
- `TRUNCATE`: deallocates entire data pages. Indexes are reset (not updated row by row). Much faster.

After heavy DELETEs, consider REINDEX to remove fragmentation.

**💡 Tip:** TRUNCATE = **fast reset** (indexes reset too). DELETE = **slow row-by-row** (indexes fragment). For clearing entire tables, always TRUNCATE.

---

### Q34. What is an index-only scan?

**Answer:** Query satisfied entirely from the index without reading the table:
```sql
CREATE INDEX idx_user_email_name ON users(email, name);
SELECT email, name FROM users WHERE email = 'ali@test.com'; -- index-only!
```

Requires: all queried columns in the index + visibility map check (PostgreSQL). Fastest read possible.

**💡 Tip:** Index-only scan = **"index has everything, skip the table."** Include all SELECT columns in the index. PostgreSQL needs VACUUM'd visibility map for this.

---

### Q35. What is page split in indexes?

**Answer:** When an index page is full and a new row needs to go in the middle, the page splits into two:
- Performance penalty (page reorganization)
- Causes fragmentation
- Common with random inserts on B-tree indexes

Mitigation: use sequential keys (SERIAL/IDENTITY) instead of random (UUID v4).

**💡 Tip:** Page split = **"page full, cut it in half."** Random inserts cause splits. Sequential inserts (SERIAL) append to the end. Use sequential keys for high-insert tables.

---

### Q36. What is the difference between UNIQUE constraint and UNIQUE index?

**Answer:**
- **UNIQUE constraint**: ANSI SQL standard, defines a rule. Automatically creates a unique index behind the scenes.
- **UNIQUE index**: implementation detail, same enforcement but more options (partial, expression)

Functionally identical. Constraint = standard SQL. Index = more flexible (conditions, expressions).

**💡 Tip:** UNIQUE constraint = **"I'm declaring a rule."** UNIQUE index = **"I'm creating a lookup + rule."** Use constraints for business rules, indexes for performance+rules.

---

### Q37. What is database partitioning?

**Answer:** Splitting large tables into smaller, manageable pieces:
- **Horizontal partitioning**: split by rows (by date, by region) — each partition has same columns
- **Vertical partitioning**: split by columns — separate rarely-used columns into another table

Improves query performance (scan fewer rows), maintenance (rebuild one partition), and archival.

**💡 Tip:** Horizontal = **row split** (by date/region). Vertical = **column split** (hot vs cold data). Most partitioning is horizontal. PostgreSQL: `PARTITION BY RANGE/LIST/HASH`.

---

### Q38. What are the trade-offs of normalization?

**Answer:**
| Normalized | Denormalized |
|---|---|
| Less redundancy | More redundancy |
| Data integrity | Potential inconsistency |
| Slower reads (more JOINs) | Faster reads (fewer JOINs) |
| Easier maintenance | Complex maintenance |
| Smaller storage | Larger storage |

OLTP → normalize. OLAP → denormalize.

**💡 Tip:** Normalize = **clean but slow reads**. Denormalize = **fast reads but messy**. OLTP (transactions) = normalize. OLAP (analytics) = denormalize.

---

### Q39. What is a full table scan and when is it OK?

**Answer:** Reading every row in a table. OK when:
- Table is small (< 1000 rows)
- Query returns >10-15% of rows (index overhead > scanning)
- No suitable index exists
- Analytical queries that need all data

Database query planner chooses automatically based on statistics.

**💡 Tip:** Full scan isn't always bad! For small tables or queries returning most rows, it's faster than index + lookup. Trust the query planner.

---

### Q40. What are database statistics and why do they matter for indexing?

**Answer:** Statistics = metadata about data distribution (row counts, value distribution, null percentage). Query planner uses statistics to decide: use index or table scan? Which join strategy?

```sql
ANALYZE users; -- update statistics
```

Outdated statistics = bad query plans = slow queries.

**💡 Tip:** Statistics = **"what does the data look like?"** Query planner uses this to choose good plans. Run ANALYZE after bulk imports/deletes. Auto-analyze handles routine updates.

---

## Advanced Questions (Q41–Q50)

### Q41. What is the difference between B-tree and B+tree?

**Answer:**
- **B-tree**: stores data in ALL nodes (internal + leaf). Keys appear once.
- **B+tree**: stores data ONLY in leaf nodes. Internal nodes have copies of keys for navigation. Leaf nodes linked for range scans.

Most databases use B+tree for indexes — better range query performance and higher fan-out.

**💡 Tip:** B+tree = **"data only at the bottom, navigation above."** Better for range queries (linked leaves). B-tree = data at every level. Databases use B+tree.

---

### Q42. What is the selectivity of an index?

**Answer:** Ratio of rows returned to total rows:
- **High selectivity**: returns few rows (email = 'specific@test.com') — index very effective
- **Low selectivity**: returns many rows (gender = 'male') — index less effective

Rule: index is useful when selectivity < 5-10%. Use `EXPLAIN` to verify.

**💡 Tip:** Selectivity = **"how selective is my filter?"** Email (unique) = highest. Gender (2 values) = lowest. Index helps most with high selectivity.

---

### Q43. What is adaptive indexing (database auto-tuning)?

**Answer:** Some databases automatically create/drop indexes based on query patterns:
- Database monitors query workload
- Identifies missing indexes
- Suggests or auto-creates indexes
- Drops unused indexes

PostgreSQL: `pg_qualstats` extension. SQL Server: Database Engine Tuning Advisor. MongoDB: Atlas Performance Advisor.

**💡 Tip:** Auto-tuning = **"database manages its own indexes."** Monitor suggestions, review before applying. Don't blindly trust auto-suggestions.

---

### Q44. What is a covering index include columns?

**Answer:** Include non-key columns in the index for covering queries without increasing index key size:
```sql
-- SQL Server
CREATE INDEX idx_users_email ON users(email) INCLUDE (name, avatar);
-- PostgreSQL (use composite)
CREATE INDEX idx_users_email ON users(email, name, avatar);
```

Included columns aren't used for sorting/searching — only stored for retrieval.

**💡 Tip:** INCLUDE = **"store but don't sort."** Key columns = searchable. Included columns = just stored. Saves index key size while providing covering queries.

---

### Q45. What is the difference between OLTP and OLAP normalization?

**Answer:**
| OLTP (Transactions) | OLAP (Analytics) |
|---|---|
| Normalized (3NF) | Denormalized (Star/Snowflake) |
| Many small transactions | Few large queries |
| Row-oriented | Column-oriented |
| Current data | Historical data |
| Examples: e-commerce, banking | Examples: reporting, BI |

**💡 Tip:** OLTP = **normalized, fast writes** (bank transactions). OLAP = **denormalized, fast reads** (reports). Different designs for different workloads.

---

### Q46. What is index intersection?

**Answer:** Using multiple indexes simultaneously for a single query:
```sql
-- Two separate indexes
CREATE INDEX idx_a ON users(age);
CREATE INDEX idx_b ON users(city);
-- Query can use BOTH:
SELECT * FROM users WHERE age > 25 AND city = 'Lahore';
```

Query planner combines results from both indexes. Less efficient than a single composite index.

**💡 Tip:** Index intersection = **"two indexes team up."** Happens but slower than one composite index. If `AND` queries are common, create a composite index.

---

### Q47. What is lock escalation and its impact on indexes?

**Answer:** Database escalates row locks to page/table locks to reduce overhead:
- Many row locks → one table lock (less memory, more blocking)
- Can happen during large UPDATE/DELETE operations

Impact: blocks other queries on the entire table. Mitigation: batch operations, use appropriate isolation levels.

**💡 Tip:** Lock escalation = **"too many small locks → one big lock."** Saves memory but blocks more queries. Batch large operations to avoid escalation.

---

### Q48. What is lazy vs eager index creation?

**Answer:**
- **Eager**: index created immediately with data (concurrent index build) — blocks writes
- **Lazy**: index built in background after data load — non-blocking, eventually consistent

PostgreSQL: `CREATE INDEX` (blocks writes), `CREATE INDEX CONCURRENTLY` (non-blocking).

**💡 Tip:** `CONCURRENTLY` = **"build index without locking."** Takes longer but doesn't block production. Always use in production for large tables.

---

### Q49. What is the difference between indexing and materialized views for performance?

**Answer:**
| Feature | Index | Materialized View |
|---|---|---|
| Storage | B-tree/B+tree | Actual table |
| Updates | Automatic | Manual/periodic refresh |
| Purpose | Speed up WHERE/JOIN | Pre-compute complex queries |
| Maintenance | Auto | Scheduled refresh |

Indexes for fast lookups. Materialized views for pre-computed aggregations.

**💡 Tip:** Index = **"find fast."** Materialized view = **"pre-compute the answer."** Use indexes for WHERE/JOIN speed. Use materialized views for heavy aggregations.

---

### Q50. How to design an optimal indexing strategy?

**Answer:**
1. **Profile queries**: find slow queries (EXPLAIN, slow query log)
2. **Index high-selectivity columns**: WHERE, JOIN, ORDER BY columns
3. **Follow ESR rule**: Equality → Sort → Range for composite indexes
4. **Use covering indexes**: include all SELECT columns
5. **Remove unused indexes**: monitor with pg_stat_user_indexes
6. **Avoid over-indexing**: each index costs on writes
7. **Reindex periodically**: fix fragmentation
8. **Update statistics**: ANALYZE after schema/data changes
9. **Test in production-like data**: cardinality matters

**💡 Tip:** Index strategy = **measure → index → verify → repeat.** Start with missing indexes on WHERE/JOIN. Add covering indexes for hot queries. Remove unused. Never stop monitoring.

---
