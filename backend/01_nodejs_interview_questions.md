# Node.js Interview Questions — Top 50 Most Probable Questions

> **Difficulty Split:** 20 Basic (Q1–Q20) | 22 Intermediate (Q21–Q42) | 8 Advanced (Q43–Q50)
> **Sources:** StackInterview, InterviewBit, GeeksforGeeks, Medium, Toptal, dev.to, easyinterview.me

---

## 🟢 BASIC (Q1–Q20) — Foundational Concepts

---

### Q1. What is Node.js and how does it differ from browser JavaScript?

**Answer:** Node.js is a server-side runtime environment built on Chrome's V8 engine that executes JavaScript outside the browser. Browser JS has access to `window`, `document`, and DOM APIs; Node.js has access to file system, network sockets, and OS-level APIs through built-in modules like `fs`, `http`, and `path`. The global object in Node is `global`, not `window`.

**Memory Tip:** 🖥️ **B**rowser has **B**OM/DOM, **N**ode has **N**ative OS modules. "Browser = Button clicks, Node = Backend calls."

---

### Q2. Explain the Node.js event loop — phases and order of execution.

**Answer:** The event loop is what allows Node.js to perform non-blocking I/O using a single thread. It processes callbacks in distinct phases in order: **timers** (setTimeout/setInterval) → **pending callbacks** (I/O errors) → **idle/prepare** (internal) → **poll** (fetch new I/O events) → **check** (setImmediate) → **close callbacks**. Between each phase, Node drains the `nextTick` queue and microtask (Promise) queue.

**Memory Tip:** 🔄 **T**imers → **P**ending → **I**dle → **P**oll → **C**heck → **C**lose. Remember: "**T**hree **P**igs **I**n **P**aris **C**an **C**ook."

---

### Q3. What is non-blocking I/O?

**Answer:** Non-blocking I/O means Node.js initiates an I/O operation (file read, DB query, network request) and immediately moves on instead of waiting. When the operation completes, the OS notifies libuv, which queues the callback for the poll phase of the event loop. This lets a single thread handle thousands of concurrent connections.

**Memory Tip:** 🍽️ Think of a restaurant — non-blocking is giving your order and getting a buzzer instead of standing at the counter.

---

### Q4. What is the difference between `process.nextTick()`, `setImmediate()`, and `setTimeout()`?

**Answer:** `process.nextTick()` fires before the next event loop phase (nextTick queue, highest priority). `Promise.then()` fires after nextTick but before next phase (microtask queue). `setImmediate()` fires in the check phase (after poll). `setTimeout(fn, 0)` fires in the timers phase (after minimum 1ms delay).

**Memory Tip:** ⚡ Priority order: **nextTick > Promises > setTimeout > setImmediate** (at top level). Inside I/O callback: **nextTick > setImmediate > setTimeout**.

---

### Q5. What are Streams in Node.js? Name the four types.

**Answer:** Streams let you read/write data piece by piece (chunks) rather than loading everything into memory. The four types: **Readable** (source, e.g., `fs.createReadStream`), **Writable** (destination, e.g., `fs.createWriteStream`), **Duplex** (both, e.g., `net.Socket`), and **Transform** (duplex that modifies data, e.g., `zlib.createGzip`).

**Memory Tip:** 🌊 **R**ead → **W**rite → **D**uplex (both) → **T**ransform (modify). "Read Write Duplex Transform" = **RWDT**.

---

### Q6. What is a Buffer? When would you use it?

**Answer:** A Buffer is a fixed-size chunk of memory allocated outside the V8 heap for handling raw binary data — file contents, network packets, image bytes. Strings are UTF-8 by default; Buffers handle any encoding (hex, base64, binary). Use `Buffer.alloc()` for safe allocation; `Buffer.allocUnsafe()` is faster but may contain old memory.

**Memory Tip:** 📦 Buffer = **Box** for **Binary** data. "alloc is safe, allocUnsafe is risky but fast."

---

### Q7. What is the difference between `require()` and ES Modules (`import/export`)?

**Answer:** `require()` is CommonJS — synchronous, dynamic, loads at runtime. `import/export` is ES Modules — static, asynchronous, analyzed at parse time (enabling tree-shaking). Node supports both: `.mjs` files use ESM; `.cjs` use CJS; `.js` depends on `"type"` in `package.json`. You can't `require()` an ESM file — use dynamic `import()`.

**Memory Tip:** 📦 **C**ommonJS = **C**opy at runtime (sync). **E**SM = **E**xamined at parse time (static, tree-shakeable).

---

### Q8. What is `module.exports` vs `exports`?

**Answer:** `module.exports` is the actual object returned by `require()`. `exports` is a shorthand reference to `module.exports`. Mutating `exports.foo = ...` works, but reassigning `exports = {...}` breaks the link — your module exports nothing. Always use `module.exports` for full reassignment.

**Memory Tip:** 📎 `exports` is an **alias**, not the real thing. Reassigning an alias breaks it. Stick to `module.exports` for safety.

---

### Q9. How do you read environment variables in Node.js?

**Answer:** Use `process.env` — an object containing all environment variables. For local development, load from a `.env` file using `dotenv` package. Never commit `.env` to git; use `.env.example` to document required variables.

```js
require('dotenv').config();
const port = process.env.PORT || 3000;
```

**Memory Tip:** 🔐 `process.env` = your app's **E**nvironment **V**ariables. Keep secrets out of code!

---

### Q10. What is the difference between `__dirname` and `__filename`?

**Answer:** `__dirname` is the absolute path of the directory containing the current file. `__filename` is the absolute path of the current file itself. In ES Modules, use `import.meta.url` with `fileURLToPath()` and `dirname()` instead.

**Memory Tip:** 📁 `__dirname` = **dir**ectory. `__filename` = **file** path. "dirname has no extension, filename does."

---

### Q11. What is `package-lock.json` and why is it important?

**Answer:** It locks exact versions of all dependencies (including transitive ones) so every team member and CI/CD installs identical versions. Use `npm ci` in CI (faster, enforces lockfile) instead of `npm install`. Always commit it to version control.

**Memory Tip:** 🔒 `package-lock.json` = **lock** the versions. Without it: "works on my machine" bugs.

---

### Q12. How do you create a simple HTTP server without frameworks?

**Answer:** Use the built-in `http` module: `http.createServer()` with a request handler, then `.listen(port)`.

```js
const http = require('http');
const server = http.createServer((req, res) => {
  res.writeHead(200, {'Content-Type': 'application/json'});
  res.end(JSON.stringify({message: 'Hello'}));
});
server.listen(3000);
```

---

### Q13. What is the difference between synchronous and asynchronous functions?

**Answer:** Synchronous functions block execution until complete. Asynchronous functions return immediately and use callbacks, Promises, or async/await for completion. Node.js is built for async I/O — blocking the event loop with sync calls kills performance.

**Memory Tip:** 🚗 Sync = **one lane road** (everyone waits). Async = **highway with lanes** (cars pass freely).

---

### Q14. What is the EventEmitter pattern?

**Answer:** EventEmitter lets objects emit named events and register listeners. It's the foundation of Node's event-driven architecture. Extend `events.EventEmitter`, emit with `.emit()`, listen with `.on()`.

```js
const EventEmitter = require('events');
class MyService extends EventEmitter {
  doWork() { this.emit('done', {result: 42}); }
}
const svc = new MyService();
svc.on('done', (data) => console.log(data));
```

**Memory Tip:** 📡 EventEmitter = **Radio station**. `.emit()` = broadcast. `.on()` = tune in.

---

### Q15. What is the purpose of `package.json` scripts?

**Answer:** Scripts automate common tasks defined in the `"scripts"` field: `npm start`, `npm test`, `npm run build`. `pre` and `post` hooks run before/after a script automatically.

**Memory Tip:** 📜 `package.json` scripts = your project's **command shortcuts**. `pretest` runs before `test`, `postbuild` runs after `build`.

---

### Q16. What is the difference between `fs.readFile()` and `fs.createReadStream()`?

**Answer:** `fs.readFile()` loads the entire file into memory before the callback fires. `fs.createReadStream()` reads in chunks, emitting `data` events. For files >100MB, streams are mandatory to avoid out-of-memory errors.

**Memory Tip:** 🪣 `readFile` = **bucket** (all at once). `createReadStream` = **hose** (continuous flow).

---

### Q17. How do you handle errors in async/await?

**Answer:** Wrap `await` calls in `try/catch`. For multiple parallel operations, use `Promise.allSettled()` to handle both successes and failures together (vs `Promise.all()` which rejects on first failure).

**Memory Tip:** 🛡️ **try/catch** = safety net. `allSettled` = get **all** results. `all` = fail **fast**.

---

### Q18. What is the REPL in Node.js?

**Answer:** REPL (Read-Eval-Print-Loop) is Node's interactive shell for running JavaScript one line at a time. Start it by typing `node` in terminal. Useful for quick testing and debugging.

**Memory Tip:** 💻 REPL = **R**ead **E**val **P**rint **L**oop. Like a JavaScript playground in your terminal.

---

### Q19. What are the built-in modules in Node.js?

**Answer:** Node ships with core modules: `fs` (file system), `http`/`https` (web server), `path` (file paths), `os` (operating system), `crypto` (encryption), `stream`, `events`, `buffer`, `child_process`, `cluster`, `util`, and `url`. No npm install needed — just `require()` them.

**Memory Tip:** 📚 Core modules = **free tools** in the box. Most used: **fs, http, path, os, crypto, stream**.

---

### Q20. How does Node.js handle concurrency despite being single-threaded?

**Answer:** Node uses a single thread for JavaScript execution but delegates I/O operations to libuv's thread pool (4 threads default) and OS async APIs. The event loop coordinates callbacks from these operations. This means I/O-bound tasks don't block the main thread.

**Memory Tip:** 🎭 Node is **single-threaded JS** but **multi-threaded I/O** via libuv. "One chef (JS), many kitchen helpers (libuv threads)."

---

## 🟡 INTERMEDIATE (Q21–Q42) — Production Patterns

---

### Q21. How do you implement JWT-based authentication?

**Answer:** Use `jsonwebtoken` to sign and verify tokens. On login, generate a JWT with user data; on protected routes, verify the token from the `Authorization` header. Store JWT in `httpOnly` cookies (not localStorage) to prevent XSS theft.

**Memory Tip:** 🔑 JWT flow: **Login → Sign token → Client stores → Send in header → Verify → Allow**. Use `httpOnly` cookies for security.

---

### Q22. What is CORS? How do you configure it?

**Answer:** CORS (Cross-Origin Resource Sharing) restricts web pages from making requests to different domains. Configure it with the `cors` middleware: whitelist specific origins in production, enable `credentials: true` for cookies.

```js
app.use(cors({ origin: ['https://myapp.com'], credentials: true }));
```

**Memory Tip:** 🌐 CORS = **bouncer** at the door. Without it: browser blocks cross-origin requests. With it: selective access.

---

### Q23. What is the difference between `PUT` and `PATCH` in REST APIs?

**Answer:** `PUT` replaces the entire resource (full update). `PATCH` applies a partial update — only included fields are modified. Always use `PUT` for full replacement, `PATCH` for partial edits.

**Memory Tip:** ✏️ **PUT** = **P**ut everything new. **PATCH** = **P**atch the hole (only what's needed).

---

### Q24. How do you validate request data in Express?

**Answer:** Use validation libraries like `joi` or `express-validator`. Define a schema, validate request body, and return 400 errors if validation fails. `express-validator` integrates directly as middleware.

**Memory Tip:** ✅ Validate at the **door** (middleware), not in every room (controller). "Don't trust the client."

---

### Q25. What is rate limiting and how do you implement it?

**Answer:** Rate limiting restricts requests per client in a time window (e.g., 100 req/15 min). Use `express-rate-limit` to prevent DoS attacks and API abuse. Apply to `/api` routes.

**Memory Tip:** 🚦 Rate limiting = **traffic light** for requests. Green = allowed, Red = "too many, slow down."

---

### Q26. How do you handle uncaught exceptions and unhandled promise rejections?

**Answer:** Register global handlers: `process.on('uncaughtException', ...)` and `process.on('unhandledRejection', ...)`. Log the error, clean up resources, and exit gracefully. Continuing after an uncaught exception is unsafe. Use process managers (PM2) to auto-restart.

**Memory Tip:** 🛟 These are **last-resort lifeguards**. The real fix: wrap async code in try/catch or use Express error middleware.

---

### Q27. What is clustering? How does it improve performance?

**Answer:** Clustering spawns multiple Node.js worker processes sharing the same port, utilizing all CPU cores. Use the `cluster` module to fork workers — each handles requests independently. In production, use PM2 for clustering, zero-downtime restarts, and log aggregation.

**Memory Tip:** 🏭 Cluster = **factory with multiple workers** on the same assembly line. More CPUs = more workers = more throughput.

---

### Q28. What are child processes? When would you use them?

**Answer:** Child processes spawn separate Node processes or shell commands. Use `child_process.spawn()` for streaming data, `.exec()` for shell commands, `.fork()` for other Node scripts with IPC messaging.

**Memory Tip:** 👶 **spawn** = stream (long output), **exec** = buffer (short commands), **fork** = Node-to-Node communication.

---

### Q29. How do you prevent SQL injection in Node.js?

**Answer:** Use parameterized queries (prepared statements). Never concatenate user input into SQL strings. ORMs like Sequelize or query builders like Knex handle this automatically.

```js
// SAFE - parameterized
const result = await db.query('SELECT * FROM users WHERE id = $1', [userId]);
```

**Memory Tip:** 🛡️ Never **concatenate** user input. Always use **parameterized queries** ($1, $2 placeholders).

---

### Q30. How do you implement logging in production Node.js?

**Answer:** Use structured logging with `pino` or `winston`. Log as JSON for log aggregators (ELK, Datadog). Include request IDs for tracing. Set log levels: error > warn > info > debug.

**Memory Tip:** 📋 **Pino** = **fastest** logger. Always use **JSON** format. Include **requestId** for tracing.

---

### Q31. What is the purpose of the `next()` function in middleware?

**Answer:** `next()` passes control to the next middleware in the stack. Without calling it, the request hangs. Calling `next(err)` skips to error-handling middleware (4 parameters: `err, req, res, next`).

**Memory Tip:** ➡️ `next()` = **pass the baton**. `next(err)` = **throw the baton to the medic** (error handler).

---

### Q32. How do you upload files in Node.js?

**Answer:** Use `multer` middleware for `multipart/form-data`. Configure storage (disk/memory), file filters (MIME type), and size limits.

**Memory Tip:** 📎 **Multer** = **M**ultipart **U**pload **L**ibrary for **E**xpress **R**outing (mnemonic). Set size limits to prevent abuse.

---

### Q33. What is the event emitter's `once()` vs `on()`?

**Answer:** `.on()` registers a listener that fires every time the event is emitted. `.once()` registers a listener that fires only the first time, then automatically removes itself.

**Memory Tip:** 🎫 `on()` = **season ticket** (every game). `once()` = **single ticket** (one game only).

---

### Q34. How do you secure sensitive data like API keys?

**Answer:** Store secrets in environment variables (`process.env`), never in code. Use `.env` locally with `dotenv`. In production, use secret managers (AWS Secrets Manager, HashiCorp Vault). Add `.env` to `.gitignore`.

**Memory Tip:** 🔐 **Never** hardcode secrets. `.env` for local, **Vault** for prod, `.gitignore` for safety.

---

### Q35. How do you implement pagination in a REST API?

**Answer:** Accept `page` and `limit` query parameters, calculate `skip = (page - 1) * limit`, and return results with metadata (total count, total pages, current page).

**Memory Tip:** 📄 Pagination formula: **skip = (page - 1) × limit**. Always return total count for UI.

---

### Q36. How do you debug a Node.js application?

**Answer:** Use `node --inspect` to attach Chrome DevTools. Add `debugger` statements for breakpoints. For production, use structured logging (`pino`) and APM tools (New Relic, Datadog, OpenTelemetry).

**Memory Tip:** 🐛 Dev: `--inspect` + Chrome DevTools. Prod: **structured logs + APM**.

---

### Q37. How do you connect to MongoDB from Node.js?

**Answer:** Use `mongoose` (ODM) or the native `mongodb` driver. Create a client, connect, and use connection pooling (default in both). Never create a new client per request.

**Memory Tip:** 🍃 **Mongoose** = schema + validation. **Native driver** = raw speed. Always use **connection pooling**.

---

### Q38. What are Promises and how do they differ from callbacks?

**Answer:** Promises represent a value that may be available now, in the future, or never. They avoid callback hell by allowing `.then()/.catch()` chaining and `async/await` syntax. Callbacks are error-prone with nesting; Promises are chainable and composable.

**Memory Tip:** 🔗 Callback = **pyramid of doom** (nested). Promise = **chain** (flat). async/await = **synchronous-looking** (cleanest).

---

### Q39. What is the difference between `Promise.all()` and `Promise.allSettled()`?

**Answer:** `Promise.all()` rejects immediately when any promise rejects (fail-fast). `Promise.allSettled()` waits for all promises to settle (resolve or reject) and returns an array of result objects with `status`, `value`, or `reason`.

**Memory Tip:** 🏁 `all` = **one fails, all fail**. `allSettled` = **everyone finishes** regardless of outcome.

---

### Q40. What are worker threads and when would you use them?

**Answer:** Worker threads (`worker_threads`) run JavaScript in parallel threads within a single process, sharing memory via `ArrayBuffer`. Use them for CPU-bound tasks (image processing, cryptography). Unlike clustering, they share memory — better for heavy computation within one app.

**Memory Tip:** 🧵 **Cluster** = separate **processes** (scale HTTP). **Worker threads** = shared-memory **threads** (heavy computation).

---

### Q41. What is a thread pool in Node.js? Which library handles it?

**Answer:** libuv provides a default thread pool of 4 threads that handles operations like file I/O, DNS lookups, and compression when the OS doesn't provide async alternatives. You can increase it via `UV_THREADPOOL_SIZE` environment variable (max 128).

**Memory Tip:** 🏊 **libuv** = the pool manager. Default = **4 threads**. Set `UV_THREADPOOL_SIZE` for more.

---

### Q42. How do you implement file streaming with backpressure handling?

**Answer:** Use `.pipe()` which handles backpressure automatically. Manually: check `writable.write()` return value — if `false`, pause the readable until `drain` event fires. Backpressure prevents memory overflow when data production outpaces consumption.

```js
readable.pipe(writable); // handles backpressure automatically
```

**Memory Tip:** 💧 `.pipe()` = **automatic water regulator**. Don't reinvent backpressure — use pipe.

---

## 🔴 ADVANCED (Q43–Q50) — System Design & Production Expertise

---

### Q43. How do you implement graceful shutdown in Node.js?

**Answer:** Listen for `SIGTERM`/`SIGINT` signals. Stop accepting new connections with `server.close()`, wait for in-flight requests to complete, close DB/Redis connections, then `process.exit(0)`. Set a force-exit timeout (10s) as a safety net.

**Memory Tip:** 🛑 Graceful shutdown = **"closing time at a restaurant"**: stop new customers, let current ones finish, lock up.

---

### Q44. How do you detect and fix memory leaks in Node.js?

**Answer:** Use `node --inspect` with Chrome DevTools to take heap snapshots. Compare snapshots over time to find objects not garbage collected. Common causes: global variables, uncleared intervals, event listeners not removed, closures holding references. Fix with LRU caches, weak references, and cleanup logic.

**Memory Tip:** 🕵️ **3 common leak culprits**: globals (never GC'd), timers (never cleared), listeners (never removed). Use heap snapshots to compare.

---

### Q45. What is the circuit breaker pattern? When would you use it?

**Answer:** A circuit breaker prevents cascading failures by stopping requests to a failing service. After N consecutive failures, it "opens" (stops calls), waits, then "half-opens" (tests with 1 request). Use `opossum` library. Apply when calling unreliable external APIs.

**Memory Tip:** ⚡ Circuit breaker = **electrical fuse**. Closed = normal. Open = tripped (no calls). Half-open = testing if safe.

---

### Q46. What is AsyncLocalStorage and when would you use it?

**Answer:** `AsyncLocalStorage` provides request-scoped context that propagates through async calls without passing variables. Use it for request IDs, user context, or tracing data — no prop drilling needed. It's built into `async_hooks`.

**Memory Tip:** 🎒 AsyncLocalStorage = **backpack** that follows you through async calls. Store requestId once, access anywhere.

---

### Q47. How do you implement distributed tracing?

**Answer:** Use OpenTelemetry to instrument your app. It injects trace IDs into requests and exports spans to backends (Jaeger, Datadog). Auto-instrument HTTP, Express, and database calls. Essential for debugging latency across microservices.

**Memory Tip:** 🔍 **OpenTelemetry** = GPS for your requests. Each service adds a "breadcrumb" to the trace.

---

### Q48. What is the N+1 query problem? How do you fix it?

**Answer:** N+1 happens when you fetch N items, then run a separate query for each item's related data. Fix with SQL JOINs, eager loading (ORM), or DataLoader for batching multiple lookups into single queries.

**Memory Tip:** 📊 N+1 = **1 query for list + N queries for details**. Fix: **JOIN** (SQL) or **DataLoader** (batch).

---

### Q49. How would you architect a high-throughput Node.js service handling 100k req/sec?

**Answer:** At 100k req/sec, the bottleneck is rarely Node.js itself — it's the database or network. Key moves: load balancer → cluster of workers → in-process LRU cache → Redis cache → DB connection pool → async job queue (BullMQ) → observability (OpenTelemetry + Prometheus). Node can handle 50-100k simple req/sec per core.

**Memory Tip:** 🏗️ **LB → Workers → LRU → Redis → DB Pool → Queue → Observe**. Cache everything possible, pool DB connections.

---

### Q50. How do you implement a custom Transform stream?

**Answer:** Extend the `Transform` class from `stream` module, implement `_transform(chunk, encoding, callback)` to process each chunk, and `_flush(callback)` for final cleanup. Use `objectMode: true` for non-Buffer data.

```js
class UpperTransform extends Transform {
  _transform(chunk, enc, cb) {
    this.push(chunk.toString().toUpperCase());
    cb();
  }
}
```

**Memory Tip:** 🔄 Transform stream = **middleman**. `_transform` = process each chunk. `_flush` = cleanup at the end.

---

*Sources: [StackInterview](https://stackinterview.abhijeetkushwaha.dev), [InterviewBit](https://www.interviewbit.com), [GeeksforGeeks](https://www.geeksforgeeks.org), [EasyInterview](https://easyinterview.me), [Medium](https://medium.com)*
