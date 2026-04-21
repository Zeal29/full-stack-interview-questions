# MongoDB Interview Questions — Top 50

> **Difficulty Split:** Basic (Q1–Q20) | Intermediate (Q21–Q40) | Advanced (Q41–Q50)

---

## Basic Questions (Q1–Q20)

### Q1. What is MongoDB?

**Answer:** MongoDB is a NoSQL document database that stores data in flexible, JSON-like documents (BSON). It's schema-less, horizontally scalable, and designed for high availability. Key features: flexible schema, rich queries, indexing, aggregation pipeline, and native scaling.

**💡 Tip:** MongoDB = **document database**. Stores JSON-like objects, not tables. Flexible schema, easy to start, scales horizontally. "Mongo" = "humongous" (designed for big data).

---

### Q2. What is BSON?

**Answer:** BSON (Binary JSON) is MongoDB's binary-encoded serialization format. Extends JSON with additional data types: Date, Binary, ObjectId, Int32, Int64, Decimal128, Regex. More efficient to traverse and store than plain JSON.

**💡 Tip:** BSON = **JSON on steroids** (binary + more types). JSON doesn't have Date type — BSON does. Faster to parse than text JSON.

---

### Q3. What is an ObjectId?

**Answer:** A 12-byte unique identifier for MongoDB documents:
- 4 bytes: timestamp (when created)
- 5 bytes: random value (machine/process)
- 3 bytes: counter

Generated automatically for `_id` field. Sortable by creation time (first 4 bytes = timestamp).

**💡 Tip:** ObjectId = **timestamp + machine + counter**. Contains creation time: `objectId.getTimestamp()`. No need for separate `created_at` field.

---

### Q4. What is the difference between a document and a collection?

**Answer:**
- **Document**: a single record (like a row in SQL), stored as BSON, max 16MB
- **Collection**: a group of documents (like a table in SQL), schema-less
- **Database**: group of collections

```javascript
// Document
{ _id: ObjectId("..."), name: "Ali", age: 25 }
// Collection: users (contains many such documents)
```

**💡 Tip:** Document = **row**. Collection = **table**. Database = **database**. Same concepts, different names. Documents can have different shapes in the same collection.

---

### Q5. What are the CRUD operations in MongoDB?

**Answer:**
- **Create**: `db.collection.insertOne()`, `insertMany()`
- **Read**: `db.collection.find()`, `findOne()`
- **Update**: `db.collection.updateOne()`, `updateMany()`, `replaceOne()`
- **Delete**: `db.collection.deleteOne()`, `deleteMany()`

```javascript
db.users.insertOne({ name: "Ali", age: 25 });
db.users.find({ age: { $gte: 18 } });
db.users.updateOne({ name: "Ali" }, { $set: { age: 26 } });
db.users.deleteOne({ name: "Ali" });
```

**💡 Tip:** CRUD = **insert, find, update, delete**. Always use `One` or `Many` suffix (no bare `update()` — deprecated).

---

### Q6. What is the difference between `find()` and `findOne()`?

**Answer:**
- `find(filter)`: returns a **cursor** to all matching documents (lazy evaluation)
- `findOne(filter)`: returns the **first matching document** (or null)

`find()` doesn't load all data at once — it returns a cursor you iterate over.

**💡 Tip:** `findOne` = **"give me one"** (actual document). `find` = **"give me a cursor"** (iterate through results). Use `findOne` when you need exactly one document.

---

### Q7. What are query operators in MongoDB?

**Answer:**
- **Comparison**: `$eq`, `$ne`, `$gt`, `$gte`, `$lt`, `$lte`, `$in`, `$nin`
- **Logical**: `$and`, `$or`, `$not`, `$nor`
- **Element**: `$exists`, `$type`
- **Array**: `$all`, `$elemMatch`, `$size`
- **Evaluation**: `$regex`, `$expr`

```javascript
db.users.find({ age: { $gte: 18, $lte: 65 }, role: { $in: ["admin", "mod"] } });
```

**💡 Tip:** Operators start with `$`. Comparison = **filter by value**. Logical = **combine conditions**. Array = **query arrays**.

---

### Q8. What are update operators?

**Answer:**
- `$set`: set field value
- `$unset`: remove field
- `$inc`: increment/decrement
- `$push` / `$pull`: add/remove from array
- `$addToSet`: add to array if not exists
- `$rename`: rename field

```javascript
db.users.updateOne({ _id: id }, {
  $set: { name: "Ahmed" },
  $inc: { loginCount: 1 },
  $push: { tags: "verified" }
});
```

**💡 Tip:** Always use operators with `$set`/`$inc` etc. Without operators, `updateOne` replaces the entire document! Common beginner mistake.

---

### Q9. What is indexing in MongoDB?

**Answer:** Indexes improve query speed by creating ordered data structures:
```javascript
db.users.createIndex({ email: 1 });           // ascending
db.users.createIndex({ email: 1 }, { unique: true }); // unique
db.users.createIndex({ name: 1, age: -1 });   // compound
db.users.createIndex({ location: "2dsphere" }); // geospatial
```

Default: `_id` index. Without indexes: **collection scan** (reads every document).

**💡 Tip:** Index = **lookup table**. Add indexes on fields used in queries/filters. Too many = slow writes. Use `explain()` to check index usage.

---

### Q10. What is the aggregation pipeline?

**Answer:** Data processing pipeline with stages:
```javascript
db.orders.aggregate([
  { $match: { status: "completed" } },         // filter
  { $group: { _id: "$customerId", total: { $sum: "$amount" } } }, // group
  { $sort: { total: -1 } },                    // sort
  { $limit: 10 }                               // limit
]);
```

Stages: `$match`, `$group`, `$sort`, `$project`, `$lookup`, `$unwind`, `$limit`, `$skip`, `$facet`.

**💡 Tip:** Aggregation = **pipeline of transformations**. Like a factory assembly line — each stage processes and passes to the next. Order matters for performance (filter early).

---

### Q11. What is the `$lookup` stage?

**Answer:** MongoDB's equivalent of a SQL LEFT JOIN:
```javascript
db.orders.aggregate([
  { $lookup: {
    from: "users",
    localField: "userId",
    foreignField: "_id",
    as: "user"
  }}
]);
```

Adds a `user` array field to each order document with matching user data.

**💡 Tip:** `$lookup` = **LEFT JOIN**. Results come as array (use `$unwind` to flatten). Use for related data. Prefer embedding over $lookup when possible.

---

### Q12. What is embedding vs referencing?

**Answer:**
- **Embedding**: store related data inside the document (nested objects/arrays)
- **Referencing**: store IDs pointing to other documents (like foreign keys)

Embed for: 1-to-few, frequently accessed together. Reference for: 1-to-many/many-to-many, large/growing data.

**💡 Tip:** Embed = **all in one place** (fast reads, no joins). Reference = **normalized** (smaller docs, flexible). Rule: embed what you read together, reference what grows independently.

---

### Q13. What is the 16MB document size limit?

**Answer:** Each MongoDB document has a maximum size of 16MB. This encourages proper data modeling — embed small related data, reference large/growing data. Use GridFS for files larger than 16MB.

**💡 Tip:** 16MB = **plenty for most documents**. If hitting the limit, your data model is wrong. Split into referenced documents.

---

### Q14. What are the data types in MongoDB?

**Answer:** BSON supports: String, Int32, Int64, Double, Boolean, Date, Null, Array, Object (document), Binary, ObjectId, Regex, Decimal128, Timestamp, MinKey, MaxKey.

**💡 Tip:** MongoDB has **more types than JSON**. `Date` (ISODate), `ObjectId`, `Int32` vs `Int64`, `Decimal128` for money. Always use correct types.

---

### Q15. What is a capped collection?

**Answer:** Fixed-size collection that overwrites oldest documents when full:
```javascript
db.createCollection("logs", { capped: true, size: 10000000, max: 10000 });
```

Maintains insertion order. High-performance writes. Use for: logs, metrics, caching. Cannot delete individual documents.

**💡 Tip:** Capped = **circular buffer**. Old data auto-deleted. Fast writes (no index overhead). Perfect for logs and real-time data.

---

### Q16. What is the `explain()` method?

**Answer:** Shows query execution plan:
```javascript
db.users.find({ email: "test@example.com" }).explain("executionStats");
// Shows: index used, documents scanned, execution time
```

`COLLSCAN` = bad (no index). `IXSCAN` = good (uses index). `FETCH` = reads full documents.

**💡 Tip:** `explain()` = **X-ray for MongoDB queries**. Look for: COLLSCAN (add index!), totalDocsExamined (should ≈ returned docs), executionTimeMillis.

---

### Q17. What is a replica set?

**Answer:** Group of MongoDB servers maintaining the same data:
- **Primary**: accepts all writes
- **Secondaries**: replicate data from primary, serve reads
- **Arbiter**: votes in elections, no data

Automatic failover: if primary dies, secondary is elected as new primary.

**💡 Tip:** Replica set = **one primary + N secondaries**. Primary = writer. Secondary = readers. Auto-failover = high availability. Always use replica sets in production.

---

### Q18. What is sharding in MongoDB?

**Answer:** Horizontal scaling by distributing data across multiple machines:
- **Shard**: holds a subset of data
- **Config server**: stores cluster metadata
- **mongos**: query router (directs queries to correct shard)

Shard key determines data distribution. Choose carefully — can't change easily.

**💡 Tip:** Sharding = **split data across servers**. Use when one server can't handle the load. Shard key = most important decision. Start with replica set, shard when needed.

---

### Q19. What is a shard key?

**Answer:** The field(s) used to distribute documents across shards:
```javascript
sh.shardCollection("mydb.users", { email: 1 }); // ranged shard key
sh.shardCollection("mydb.logs", { _id: "hashed" }); // hashed shard key
```

Cannot be changed after sharding. Choose a key with: high cardinality, low frequency, even distribution.

**💡 Tip:** Shard key = **how data is divided**. Bad key = hot shard (all data on one server). Use hashed for even distribution, ranged for range queries.

---

### Q20. What is the MongoDB connection string format?

**Answer:**
```
mongodb://user:password@host1:27017,host2:27017,host3:27017/database?options
mongodb+srv://user:password@cluster.example.com/database?retryWrites=true
```

`+srv` uses DNS seedlist. Common options: `retryWrites`, `w=majority`, `readPreference=secondary`.

**💡 Tip:** `mongodb+srv://` = **modern connection string** (DNS-based). Include multiple hosts for replica sets. Never hardcode — use environment variables.

---

## Intermediate Questions (Q21–Q40)

### Q21. What are ACID transactions in MongoDB?

**Answer:** MongoDB supports multi-document ACID transactions (since 4.0):
```javascript
const session = client.startSession();
session.startTransaction();
try {
  await accounts.updateOne({ id: 1 }, { $inc: { balance: -100 } }, { session });
  await accounts.updateOne({ id: 2 }, { $inc: { balance: 100 } }, { session });
  await session.commitTransaction();
} catch { await session.abortTransaction(); }
```

Requires replica set. Has performance overhead — prefer atomic operations when possible.

**💡 Tip:** MongoDB has ACID transactions, but **prefer single-document atomicity** (embed related data). Transactions add overhead. Use for true multi-document transfers.

---

### Q22. What is the `$unwind` stage?

**Answer:** Deconstructs an array field into multiple documents:
```javascript
// { tags: ["a", "b", "c"] } → becomes 3 documents
db.posts.aggregate([{ $unwind: "$tags" }]);
```

Use `preserveNullAndEmptyArrays: true` to keep documents with empty/missing arrays.

**💡 Tip:** `$unwind` = **"explode array into rows"**. Like SQL's UNNEST. Use before `$group` for array-based aggregations.

---

### Q23. What are change streams?

**Answer:** Real-time notifications about data changes:
```javascript
const changeStream = db.users.watch();
changeStream.on("change", (next) => {
  console.log(next.operationType); // "insert", "update", "delete"
  console.log(next.fullDocument);
});
```

Built on replica set oplog. Like triggers but in your application code.

**💡 Tip:** Change streams = **real-time data change notifications**. Like PostgreSQL LISTEN/NOTIFY. Use for: sync, audit, real-time dashboards. Requires replica set.

---

### Q24. What are text indexes?

**Answer:** Full-text search on string content:
```javascript
db.articles.createIndex({ title: "text", content: "text" });
db.articles.find({ $text: { $search: "mongodb performance" } });
// With score
db.articles.find({ $text: { $search: "mongodb" } }, { score: { $meta: "textScore" } })
  .sort({ score: { $meta: "textScore" } });
```

One text index per collection. Supports multiple languages.

**💡 Tip:** Text index = **built-in search**. Good for basic needs. For production search (facets, fuzzy, autocomplete), use Atlas Search or Elasticsearch.

---

### Q25. What is the TTL index?

**Answer:** Auto-deletes documents after a specified time:
```javascript
db.sessions.createIndex({ createdAt: 1 }, { expireAfterSeconds: 3600 }); // 1 hour
```

Documents expire and are removed automatically. Perfect for: sessions, tokens, logs, temporary data.

**💡 Tip:** TTL index = **self-destruct timer**. Set `expireAfterSeconds` on a Date field. Background thread removes expired docs every 60 seconds.

---

### Q26. What are partial and sparse indexes?

**Answer:**
- **Sparse**: only indexes documents that have the indexed field
- **Partial**: indexes documents matching a filter condition

```javascript
db.users.createIndex({ email: 1 }, { sparse: true });
db.orders.createIndex({ status: 1 }, { partialFilterExpression: { status: "active" } });
```

Reduces index size by excluding irrelevant documents.

**💡 Tip:** Sparse = **"only index what exists."** Partial = **"only index what matches."** Both save space and improve write performance.

---

### Q27. What is the `$facet` stage?

**Answer:** Runs multiple aggregation pipelines on the same data in one query:
```javascript
db.products.aggregate([{ $facet: {
  categories: [{ $group: { _id: "$category", count: { $sum: 1 } } }],
  priceRange: [{ $group: { _id: null, min: { $min: "$price" }, max: { $max: "$price" } } }],
  topRated: [{ $sort: { rating: -1 } }, { $limit: 5 }]
}}]);
```

Like running multiple queries in one. Each facet is independent.

**💡 Tip:** `$facet` = **parallel aggregations**. Get counts, ranges, and top results in one query. Perfect for e-commerce filter pages.

---

### Q28. What is the `$merge` stage?

**Answer:** Writes aggregation results to a collection (create materialized views):
```javascript
db.dailyStats.aggregate([
  { $group: { _id: "$date", total: { $sum: "$amount" } } },
  { $merge: { into: "dailySummary", whenMatched: "merge", whenNotMatched: "insert" } }
]);
```

Can insert new documents, update existing, or replace. Incremental updates.

**💡 Tip:** `$merge` = **"save aggregation results to a collection."** Create materialized views, pre-compute reports. `$out` = replace, `$merge` = upsert.

---

### Q29. What is the read concern and write concern?

**Answer:**
- **Read concern**: how "confirmed" data must be before reading (`local`, `majority`, `linearizable`)
- **Write concern**: how many nodes must acknowledge a write (`w: 1`, `w: "majority"`, `w: 3`)

```javascript
db.users.find().readConcern("majority"); // only read confirmed data
db.users.insertOne(doc, { writeConcern: { w: "majority" } }); // wait for majority
```

**💡 Tip:** `majority` = **safe** (no rollbacks). `local` = **fast** (may roll back). Use `majority` for financial/critical data, `local` for logs/analytics.

---

### Q30. What is the `$expr` operator?

**Answer:** Allows using aggregation expressions in queries:
```javascript
// Find documents where fieldA > fieldB (can't do with regular queries)
db.products.find({ $expr: { $gt: ["$price", "$comparePrice"] } });
// With let for referencing variables
db.orders.find({ $expr: { $gt: [{ $subtract: ["$total", "$discount"] }, 100] } });
```

**💡 Tip:** `$expr` = **"compare fields to each other"** (not just fields to values). Regular queries compare fields to constants. `$expr` compares fields to fields.

---

### Q31. What is a compound index and the ESR rule?

**Answer:** Compound index on multiple fields follows the **ESR rule** for optimal performance:
1. **E**quality matches first
2. **S**ort fields next
3. **R**ange queries last

```javascript
db.events.createIndex({ type: 1, date: -1, priority: 1 });
// E(type) + S(date) + R(priority)
```

**💡 Tip:** ESR = **Equality → Sort → Range**. Order your compound index fields accordingly. Wrong order = index can't be used efficiently.

---

### Q32. What is the `$bucket` stage?

**Answer:** Groups documents into buckets based on a field:
```javascript
db.users.aggregate([{ $bucket: {
  groupBy: "$age",
  boundaries: [0, 18, 30, 50, 100],
  default: "100+",
  output: { count: { $sum: 1 }, names: { $push: "$name" } }
}}]);
```

Creates histogram-like distributions.

**💡 Tip:** `$bucket` = **"group into ranges."** Age ranges, price ranges, score ranges. Like `CASE WHEN age BETWEEN 18 AND 30` in SQL.

---

### Q33. What is Atlas Search?

**Answer:** MongoDB Atlas's built-in full-text search powered by Apache Lucene:
- Fuzzy matching, autocomplete, faceted search
- Synonyms, custom analyzers
- Relevance scoring (BM25)
- No separate Elasticsearch cluster needed

**💡 Tip:** Atlas Search = **Elasticsearch inside MongoDB Atlas**. Use for production search features. No separate infrastructure to manage.

---

### Q34. How to handle schema validation in MongoDB?

**Answer:** JSON Schema validation on collections:
```javascript
db.createCollection("users", {
  validator: {
    $jsonSchema: {
      required: ["name", "email"],
      properties: {
        name: { bsonType: "string" },
        email: { bsonType: "string", pattern: "^.+@.+$" },
        age: { bsonType: "int", minimum: 0 }
      }
    }
  }
});
```

**💡 Tip:** MongoDB can be **schema-less or schema-validated**. Use validation for production apps — catch bad data at the database level. Best of both worlds.

---

### Q35. What is the `$graphLookup` stage?

**Answer:** Recursive search on graph/hierarchical data:
```javascript
db.employees.aggregate([{ $graphLookup: {
  from: "employees",
  startWith: "$managerId",
  connectFromField: "managerId",
  connectToField: "_id",
  as: "managementChain",
  maxDepth: 5
}}]);
```

Follows references recursively — org charts, friend networks, category trees.

**💡 Tip:** `$graphLookup` = **"follow the chain."** Org charts, category trees, social graphs. Performance-heavy — use for small graphs or pre-compute.

---

### Q36. What is the bulk write operation?

**Answer:** Batch multiple write operations for efficiency:
```javascript
db.users.bulkWrite([
  { insertOne: { document: { name: "Ali" } } },
  { updateOne: { filter: { name: "Bob" }, update: { $set: { age: 30 } } } },
  { deleteOne: { filter: { name: "Charlie" } } }
], { ordered: false }); // parallel execution
```

`ordered: false` = execute in parallel (faster). `ordered: true` = stop on first error.

**💡 Tip:** `bulkWrite` = **batch operations**. 1000 operations in one call instead of 1000 individual calls. 10-100x faster for bulk imports/updates.

---

### Q37. What is the `collation` option?

**Answer:** Language-specific string comparison:
```javascript
db.users.find({ name: "ali" }).collation({ locale: "en", strength: 2 }); // case-insensitive
db.users.find().sort({ name: 1 }).collation({ locale: "ur" }); // Urdu sorting
```

`strength: 1` = ignore case + diacritics. `strength: 2` = ignore case only.

**💡 Tip:** Collation = **"how to compare strings."** Case-insensitive search, locale-aware sorting. Essential for multi-language apps.

---

### Q38. What is Atlas App Services (Realm)?

**Answer:** Serverless backend for MongoDB Atlas:
- **Functions**: serverless JavaScript functions
- **Triggers**: run on database changes, schedules, auth events
- **GraphQL API**: auto-generated from schema
- **Data API**: REST API for MongoDB data
- **Sync**: offline-first mobile sync

**💡 Tip:** Atlas App Services = **backend without managing servers**. Like Firebase but for MongoDB. Good for mobile/web apps.

---

### Q39. What is the `$redact` stage?

**Answer:** Security-focused stage that controls what data is returned:
```javascript
db.users.aggregate([{ $redact: {
  $cond: {
    if: { $eq: ["$level", "public"] },
    then: "$$DESCEND",
    else: "$$PRUNE"
  }
}}]);
```

`$$DESCEND` = check nested docs. `$$PRUNE` = exclude. `$$KEEP` = include without checking.

**💡 Tip:** `$redact` = **row-level security in aggregation**. Filter sensitive fields based on user permissions. Like RLS in PostgreSQL.

---

### Q40. How to handle time series in MongoDB?

**Answer:** MongoDB 5.0+ has native time series collections:
```javascript
db.createCollection("metrics", {
  timeseries: { timeField: "timestamp", metaField: "sensorId", granularity: "minutes" }
});
db.metrics.insertOne({ timestamp: new Date(), sensorId: "s1", value: 23.5 });
```

Optimized storage and queries for time-based data. Automatic bucketing.

**💡 Tip:** Time series collections = **built-in IoT/metrics support**. Efficient storage, fast range queries, automatic bucketing. Replace manual bucket patterns.

---

## Advanced Questions (Q41–Q50)

### Q41. What is the WiredTiger storage engine?

**Answer:** Default storage engine since MongoDB 3.2:
- Document-level concurrency (not collection-level)
- Compression (Snappy by default, Zlib, Zstd)
- Checkpoint-based persistence
- B-tree index format
- Supports encryption at rest

**💡 Tip:** WiredTiger = **MongoDB's engine room**. Document-level locking (not global). Compresses data automatically. Checkpoints for crash recovery.

---

### Q42. What is the oplog?

**Answer:** Operations log — a capped collection that records all writes:
- Used for replication (secondaries read and apply oplog entries)
- Used for change streams
- Default size: 5% of free disk (configurable)

Located in `local.oplog.rs`. Secondaries tail the oplog like a queue.

**💡 Tip:** Oplog = **replication's backbone**. Every write → oplog entry → secondaries copy. Change streams read from oplog. Size matters — too small = replication breaks.

---

### Q43. What are read preferences?

**Answer:** Controls which replica set members serve reads:
- `primary` (default): only from primary (consistent)
- `primaryPreferred`: primary if available, else secondary
- `secondary`: only from secondaries (eventually consistent)
- `secondaryPreferred`: secondaries first, primary as fallback
- `nearest`: lowest latency member

**💡 Tip:** Read preference = **"where should I read from?"** `primary` = consistent. `secondary` = offload primary. Use `secondaryPreferred` for analytics.

---

### Q44. What is causal consistency?

**Answer:** Ensures reads reflect previous writes by the same session:
```javascript
const session = client.startSession({ causalConsistency: true });
await db.users.updateOne({}, { $set: { status: "active" } }, { session });
// This read WILL see the update, even if read from secondary
const result = await db.users.find({}).session(session).readPref("secondary");
```

Requires `readConcern: "majority"` and `writeConcern: "majority"`.

**💡 Tip:** Causal consistency = **"my reads see my writes."** Even if reading from a secondary. Like reading your own writes in a distributed system.

---

### Q45. What is the `$lookup` with pipeline?

**Answer:** Advanced join with sub-pipeline:
```javascript
db.orders.aggregate([{ $lookup: {
  from: "products",
  let: { productId: "$productId", qty: "$quantity" },
  pipeline: [
    { $match: { $expr: { $eq: ["$_id", "$$productId"] } } },
    { $project: { name: 1, revenue: { $multiply: ["$price", "$$qty"] } } }
  ],
  as: "productDetails"
}}]);
```

Can filter, transform, and compute in the lookup stage.

**💡 Tip:** `$lookup` with pipeline = **"smart join"** (filter + transform during join). Regular `$lookup` = fetch all matches. Pipeline = only what you need.

---

### Q46. What are wildcard indexes?

**Answer:** Index all fields matching a pattern:
```javascript
db.products.createIndex({ "attributes.$**": 1 }); // all fields under attributes
db.products.createIndex({ "$**": 1 }); // all fields (expensive)
```

Useful for: dynamic schemas, user-defined attributes, product catalogs.

**💡 Tip:** Wildcard indexes = **"index everything matching a pattern."** Good for dynamic fields. Don't use `$**` on entire collection — too expensive.

---

### Q47. What is on-demand materialized views with `$merge`?

**Answer:** Build materialized views that update incrementally:
```javascript
// Run periodically
db.rawEvents.aggregate([
  { $match: { processed: false } },
  { $group: { _id: "$type", count: { $sum: 1 } } },
  { $merge: { into: "summary", whenMatched: [{ $addFields: { count: { $add: ["$count", "$$new.count"] } } }] } }
]);
```

Only processes new data — incremental updates, not full recomputation.

**💡 Tip:** `$merge` for incremental materialized views = **"only compute what changed."** Faster than rebuilding entire views. Schedule with Atlas Triggers.

---

### Q48. What is hedged reads?

**Answer:** Issue the same read to multiple replica set members and use the fastest response:
```javascript
client = new MongoClient(uri, { readPreference: "nearest", hedgedReadsEnabled: true });
```

Reduces tail latency. Reads go to two members simultaneously, first response wins.

**💡 Tip:** Hedged reads = **"ask two servers, use the faster answer."** Reduces latency outliers. Use with `nearest` read preference.

---

### Q49. What is the `$densify` stage?

**Answer:** Fills gaps in time series data:
```javascript
db.metrics.aggregate([{ $densify: {
  field: "timestamp",
  range: { step: 1, unit: "hour", bounds: [startDate, endDate] }
}}]);
```

Creates documents for missing time intervals. Essential for complete time series charts.

**💡 Tip:** `$densify` = **"fill the gaps in time data."** Missing hours get empty documents. Perfect for charts that need continuous time axes.

---

### Q50. How to optimize MongoDB performance?

**Answer:**
- **Indexes**: add for query patterns, follow ESR rule, use covered queries
- **Projections**: only return needed fields (`{ name: 1, _id: 0 }`)
- **Aggregation**: `$match` early to reduce documents
- **Connection pooling**: configure pool size
- **Sharding**: distribute load when single server insufficient
- **Read preferences**: offload reads to secondaries
- **Monitoring**: use Atlas Performance Advisor, `explain()`
- **Schema design**: embed for reads, reference for writes

**💡 Tip:** Optimize = **index → project → aggregate smartly → scale horizontally**. Always start with `explain()` to find bottlenecks. Don't premature shard.

---
