# Node.js Interview Questions — Top 50

> **Difficulty Split:** Basic (Q1–Q20) | Intermediate (Q21–Q40) | Advanced (Q41–Q50)

---

## Basic Questions (Q1–Q20)

### Q1. What is Node.js?

**Answer:** Node.js is a JavaScript runtime built on Chrome's V8 engine that executes JavaScript outside the browser (server-side). It's single-threaded, uses non-blocking I/O, and is event-driven. Ideal for I/O-heavy apps (APIs, real-time, microservices).

**💡 Tip:** Node.js = **JavaScript on the server**. V8 engine + libuv (async I/O). Single-threaded but handles concurrency via event loop. Best for I/O, not CPU-heavy tasks.

---

### Q2. How is Node.js different from browser JavaScript?

**Answer:**
- **Browser JS**: DOM access, `window` object, no file system, limited to browser APIs
- **Node.js**: `global` object, file system, network, OS access, `require`/`import`, `process`, `Buffer`, no DOM

Same V8 engine, different environment and APIs.

**💡 Tip:** Browser = **UI/DOM**. Node = **server/files/network**. Same language, different playgrounds. `window` → `global`.

---

### Q3. What is the event loop in Node.js?

**Answer:** The event loop is the mechanism that allows Node.js to perform non-blocking I/O despite being single-threaded. Phases: timers → pending callbacks → idle/prepare → poll → check → close callbacks. Processes one callback at a time from the queue.

**💡 Tip:** Event loop = **waiter serving tables**. Take order → give to kitchen → serve others → deliver food when ready. Never blocks waiting for one task.

---

### Q4. What is the difference between `require` and `import`?

**Answer:**
- `require`: CommonJS (Node default), synchronous, loaded at runtime, can use anywhere
- `import`: ES Modules, asynchronous, statically analyzed at compile time, must be at top level

Use `"type": "module"` in package.json or `.mjs` extension for ES Modules.

**💡 Tip:** `require` = **CommonJS** (old, sync). `import` = **ESM** (modern, async, tree-shakeable). New projects should use ESM.

---

### Q5. What is the `package.json` file?

**Answer:** Manifest file for Node.js projects containing: name, version, scripts (`npm run`), dependencies (`dependencies` + `devDependencies`), engines, and project metadata. Created with `npm init`.

**💡 Tip:** `package.json` = **project ID card**. Dependencies = production. devDependencies = development only. Scripts = shortcuts for common commands.

---

### Q6. What is the difference between `dependencies` and `devDependencies`?

**Answer:**
- `dependencies`: required at runtime (Express, database drivers, lodash) — installed with `npm install`
- `devDependencies`: only needed during development (jest, typescript, eslint) — NOT installed with `npm install --production`

**💡 Tip:** If it **runs in production**, it's a dependency. If it's **only for building/testing**, it's a devDependency.

---

### Q7. What is `npm` and what is `npx`?

**Answer:**
- `npm` (Node Package Manager): installs packages, manages dependencies, runs scripts
- `npx`: executes npm packages without installing globally. Runs binaries from `node_modules/.bin` or downloads temporarily.

Example: `npx create-react-app my-app` — runs without global install.

**💡 Tip:** `npm` = **install and manage**. `npx` = **run once without installing**. Use npx for scaffolding tools and one-off commands.

---

### Q8. What is the `module.exports` and `exports`?

**Answer:**
- `module.exports = value`: the actual export mechanism. Replaces entire export.
- `exports.key = value`: shorthand, references `module.exports`. Only works for adding properties, not reassignment.

```javascript
module.exports = myFunction; // OK
exports = myFunction; // WRONG! breaks the reference
exports.fn = myFunction; // OK
```

**💡 Tip:** Use `module.exports` for single export. Use `exports.xxx` for multiple named exports. Never reassign `exports` directly.

---

### Q9. What are streams in Node.js?

**Answer:** Streams process data piece by piece without loading everything into memory:
- **Readable**: fs.createReadStream (data in)
- **Writable**: fs.createWriteStream (data out)
- **Duplex**: TCP socket (read + write)
- **Transform**: zlib (read, modify, write)

Use `.pipe()` to connect streams. Essential for large files and real-time data.

**💡 Tip:** Streams = **conveyor belt** (process piece by piece). Without streams = **truck everything at once** (memory explosion). Always use streams for large data.

---

### Q10. What is a Buffer in Node.js?

**Answer:** Buffer is a fixed-size chunk of memory for raw binary data. Used when dealing with: file system, network packets, images, encryption. Can't be resized after creation.

```javascript
const buf = Buffer.from('Hello');
buf.toString(); // 'Hello'
Buffer.alloc(1024); // 1KB zero-filled buffer
```

**💡 Tip:** Buffer = **raw bytes in memory**. Think of it as a fixed-size container for binary data. Used for file I/O, network, cryptography.

---

### Q11. What is the `process` object?

**Answer:** Global object providing info about the current Node.js process:
- `process.env`: environment variables
- `process.argv`: command-line arguments
- `process.cwd()`: current working directory
- `process.exit()`: terminate process
- `process.pid`: process ID
- `process.nextTick()`: schedule callback after current operation

**💡 Tip:** `process` = **"who am I and where am I running?"** `process.env` for config, `process.argv` for CLI args, `process.exit()` for shutdown.

---

### Q12. What is `process.nextTick()`?

**Answer:** Schedules a callback to execute immediately after the current operation, before the event loop continues. Higher priority than microtasks (Promise.then). Can starve the event loop if used recursively.

**💡 Tip:** `nextTick` = **"do this RIGHT NOW, before anything else."** Higher priority than Promises. Don't use recursively — it blocks the event loop.

---

### Q13. What is the `fs` module?

**Answer:** File system module for interacting with files:
- `fs.readFile(path, cb)` / `fs.readFileSync(path)` — read files
- `fs.writeFile(path, data, cb)` / `fs.writeFileSync(path, data)` — write files
- `fs.mkdir`, `fs.readdir`, `fs.stat`, `fs.unlink`, `fs.rename`
- Promises API: `fs.promises.readFile()` or `require('fs/promises')`

**💡 Tip:** Always prefer async versions (`fs.promises`) in production. Sync versions block the event loop — only use in startup scripts.

---

### Q14. What is the `path` module?

**Answer:** Utility for file path operations:
- `path.join('src', 'files', 'app.js')` → `'src/files/app.js'` (cross-platform)
- `path.resolve('src', 'app.js')` → absolute path
- `path.dirname`, `path.basename`, `path.extname`
- `path.parse` → { root, dir, base, ext, name }

**💡 Tip:** Always use `path.join()` — never concatenate paths with `+` or template literals. Handles Windows (`\`) vs Unix (`/`) differences.

---

### Q15. What is the `http` module?

**Answer:** Built-in module for creating HTTP servers:
```javascript
const http = require('http');
const server = http.createServer((req, res) => {
  res.writeHead(200, { 'Content-Type': 'text/plain' });
  res.end('Hello World');
});
server.listen(3000);
```

Low-level. Most projects use Express, Fastify, or NestJS instead.

**💡 Tip:** `http` module = **raw HTTP server**. Express builds on top of it. Know the basics, but use a framework for real apps.

---

### Q16. What is the ` EventEmitter` class?

**Answer:** Core pattern for event-driven architecture:
```javascript
const emitter = new EventEmitter();
emitter.on('data', (payload) => console.log(payload));
emitter.emit('data', { msg: 'hello' });
```

Many Node.js objects extend EventEmitter: streams, servers, processes. Max 10 listeners by default.

**💡 Tip:** EventEmitter = **pub/sub pattern**. `on` = subscribe. `emit` = publish. `once` = subscribe once. Core of Node.js event-driven design.

---

### Q17. What are global objects in Node.js?

**Answer:** Available everywhere without `require`:
- `global` — global namespace (like `window`)
- `process` — current process info
- `console` — logging
- `Buffer` — binary data
- `__dirname` — current directory path
- `__filename` — current file path
- `setTimeout`, `setInterval`, `setImmediate`
- `queueMicrotask`

**💡 Tip:** `__dirname` and `__filename` are NOT globals in ESM — use `import.meta.dirname` and `import.meta.filename` instead.

---

### Q18. What is the difference between `__dirname` and `./`?

**Answer:**
- `__dirname`: absolute path to the directory of the **current file** (always consistent)
- `./`: relative to the **current working directory** (where you ran `node` from — can change)

Always use `__dirname` (or `import.meta.dirname`) for reliable path resolution.

**💡 Tip:** `__dirname` = **where the file lives** (fixed). `./` = **where you ran the command** (variable). Always prefer `__dirname`.

---

### Q19. What is the `child_process` module?

**Answer:** Spawn subprocesses:
- `exec()`: runs shell command, buffers output, good for small output
- `spawn()`: streams output, good for long-running/large output
- `fork()`: spawns new Node.js process with IPC communication
- `execFile()`: runs executable without shell

```javascript
const { exec } = require('child_process');
exec('ls -la', (err, stdout, stderr) => console.log(stdout));
```

**💡 Tip:** `spawn` = **streaming** (large/long processes). `exec` = **buffered** (small, quick commands). `fork` = **Node.js to Node.js** communication.

---

### Q20. What is the `cluster` module?

**Answer:** Creates multiple Node.js processes (workers) to utilize all CPU cores:
```javascript
const cluster = require('cluster');
if (cluster.isPrimary) {
  const numCPUs = require('os').cpus().length;
  for (let i = 0; i < numCPUs; i++) cluster.fork();
} else {
  // Worker: start server
  require('./server');
}
```

Master process distributes connections to workers.

**💡 Tip:** Cluster = **clones of your app**, one per CPU core. Overcomes single-thread limitation. PM2 handles this automatically.

---

## Intermediate Questions (Q21–Q40)

### Q21. What is the libuv library?

**Answer:** libuv is the C library that provides Node.js's event loop and async I/O:
- Manages the thread pool (4 threads by default, configurable via `UV_THREADPOOL_SIZE`)
- Handles file system, DNS lookup, network operations
- Provides the event loop implementation

**💡 Tip:** libuv = **Node.js's engine room**. Thread pool for file I/O, DNS, compression. Event loop = libuv's scheduling mechanism.

---

### Q22. What are worker threads?

**Answer:** `worker_threads` module enables true parallelism for CPU-heavy tasks:
```javascript
const { Worker } = require('worker_threads');
const worker = new Worker('./heavy-task.js', { workerData: { n: 40 } });
worker.on('message', result => console.log(result));
```

Each thread has its own V8 instance and event loop. Shares memory via `SharedArrayBuffer`.

**💡 Tip:** Worker threads = **parallelism for CPU work** (image processing, encryption, math). NOT for I/O (event loop handles that). Use for heavy computation.

---

### Q23. What is the thread pool in Node.js?

**Answer:** libuv maintains a thread pool (default 4 threads) for operations that can't be done asynchronously at the OS level: file system operations, DNS lookup, compression (zlib), crypto. Increase with `UV_THREADPOOL_SIZE=8 node app.js`.

**💡 Tip:** Thread pool = **helpers for blocking tasks**. File I/O, DNS, zlib, crypto use it. Default 4 — increase if all threads are busy (watch for delays).

---

### Q24. What is the difference between `setImmediate` and `setTimeout(fn, 0)`?

**Answer:**
- `setImmediate`: executes in the **check** phase (after I/O events)
- `setTimeout(fn, 0)`: executes in the **timers** phase (may be delayed by system clock, minimum 1ms)

In I/O cycles: `setImmediate` always executes before `setTimeout(fn, 0)`.

**💡 Tip:** `setImmediate` = **"after I/O"**. `setTimeout(0)` = **"as soon as timers allow"**. In I/O context, `setImmediate` is more predictable.

---

### Q25. What is backpressure in streams?

**Answer:** Backpressure occurs when a writable stream can't keep up with a readable stream. The readable stream is paused (or buffered) until the writable stream catches up. `.pipe()` handles this automatically.

Manual: check `writable.write()` return value — `false` means pause the readable.

**💡 Tip:** Backpressure = **"slow down, I can't keep up!"** `.pipe()` handles it. Without it, data piles up in memory → crash.

---

### Q26. What is the `net` module?

**Answer:** Creates TCP servers and clients for low-level network communication:
```javascript
const net = require('net');
const server = net.createServer((socket) => {
  socket.write('Hello\r\n');
  socket.on('data', (data) => console.log(data.toString()));
});
server.listen(8080);
```

Used for custom protocols, proxies, chat servers.

**💡 Tip:** `net` = **raw TCP** (below HTTP). Use for custom protocols, real-time systems. For HTTP, use Express/Fastify.

---

### Q27. What is the `crypto` module?

**Answer:** Built-in cryptography:
- Hashing: `crypto.createHash('sha256').update(data).digest('hex')`
- Encryption: `crypto.createCipheriv()` / `crypto.createDecipheriv()`
- HMAC: `crypto.createHmac()`
- Random: `crypto.randomBytes(32)`
- Signing: `crypto.createSign()` / `crypto.createVerify()`

**💡 Tip:** `crypto` = **Node.js security toolkit**. Always use `crypto.randomBytes()` for tokens (not `Math.random`). Use `scrypt` or `bcrypt` for passwords.

---

### Q28. What is the `os` module?

**Answer:** Operating system utilities:
- `os.cpus()` — CPU info (cores, model)
- `os.totalmem()` / `os.freemem()` — memory
- `os.hostname()` — machine name
- `os.platform()` — OS platform
- `os.uptime()` — system uptime
- `os.networkInterfaces()` — network info

**💡 Tip:** `os` = **system info**. `cpus().length` for cluster sizing. `freemem()` for health checks. `hostname()` for logging context.

---

### Q29. What are environment variables and how to use them?

**Answer:** Configuration values set outside the code:
```javascript
// Access
const port = process.env.PORT || 3000;
const dbUrl = process.env.DATABASE_URL;

// .env file + dotenv
require('dotenv').config();
```

Never hardcode secrets. Use `.env` locally, environment variables in production.

**💡 Tip:** Environment variables = **config outside code**. `.env` for local. `process.env.KEY` to access. **Never commit `.env` files to git.**

---

### Q30. What is `util.promisify`?

**Answer:** Converts callback-style functions to Promise-based:
```javascript
const fs = require('fs');
const { promisify } = require('util');
const readFile = promisify(fs.readFile);
// Now: const data = await readFile('file.txt', 'utf8');
```

Or use `fs/promises` which already provides Promise-based APIs.

**💡 Tip:** `promisify` = **callback → Promise converter**. Modern approach: use `fs/promises`, `util.parseArgs`, and other built-in promise APIs.

---

### Q31. What is the `events` module's `once` method?

**Answer:** `once` subscribes to an event for a single occurrence:
```javascript
emitter.once('connect', () => console.log('Connected!'));
// Fires once, then auto-removes
```

Also: `events.once(emitter, 'event')` returns a Promise that resolves on first event.

**💡 Tip:** `once` = **one-time listener** (auto-cleanup). Promise version: `await events.once(server, 'listening')` — wait for event in async code.

---

### Q32. What are Node.js middleware patterns?

**Answer:** Functions that have access to request/response and the `next` function:
```javascript
app.use((req, res, next) => {
  console.log(`${req.method} ${req.url}`);
  next(); // pass control to next middleware
});
```

Pattern used by Express, Koa, and custom frameworks. Chain of responsibility pattern.

**💡 Tip:** Middleware = **assembly line** — each function processes the request, then passes it on. `next()` = "I'm done, next person." No `next()` = response ends.

---

### Q33. What is `Error-first callback` pattern?

**Answer:** Node.js convention for callbacks: first argument is error (null if no error), second is data:
```javascript
fs.readFile('file.txt', (err, data) => {
  if (err) return console.error(err);
  console.log(data);
});
```

Always check error first. Modern alternative: Promises/async-await.

**💡 Tip:** Error-first = **"error gets VIP treatment"** (first parameter). `if (err)` check before using data. Legacy pattern — use Promises now.

---

### Q34. What is the `url` module?

**Answer:** URL parsing and construction:
```javascript
const url = new URL('https://example.com/path?q=hello#section');
url.hostname; // 'example.com'
url.pathname; // '/path'
url.searchParams.get('q'); // 'hello'
```

Use `new URL()` (Web API) instead of legacy `url.parse()`. Works in both Node and browser.

**💡 Tip:** `new URL()` = **modern URL parser**. `searchParams.get()` for query strings. Don't use deprecated `url.parse()`.

---

### Q35. What is the `querystring` module?

**Answer:** Parse/stringify URL query strings (deprecated in favor of `URLSearchParams`):
```javascript
const params = new URLSearchParams('name=Ali&age=25');
params.get('name'); // 'Ali'
params.toString(); // 'name=Ali&age=25'
```

**💡 Tip:** Use `URLSearchParams` (built-in, modern). Don't use the deprecated `querystring` module.

---

### Q36. How does Node.js handle uncaught exceptions?

**Answer:**
```javascript
process.on('uncaughtException', (err) => {
  console.error('Uncaught:', err);
  process.exit(1); // MUST exit — app is in unknown state
});
```

In Node.js, uncaught exceptions leave the app in an inconsistent state. Best practice: log the error, exit, and let the process manager (PM2, Docker) restart.

**💡 Tip:** `uncaughtException` = **emergency exit**. Log, crash, restart (PM2). Don't try to continue running — state may be corrupted.

---

### Q37. What is the `assert` module?

**Answer:** Built-in assertion for testing and validation:
```javascript
assert.strictEqual(1 + 1, 2);
assert.deepEqual(obj1, obj2);
assert.throws(() => fn(), /error message/);
assert.ok(value, 'Value should be truthy');
```

Lightweight testing. For full test suites, use Jest, Vitest, or Mocha.

**💡 Tip:** `assert` = **quick validation**. Good for internal invariants and simple tests. Use Jest/Vitest for comprehensive testing.

---

### Q38. What are source maps?

**Answer:** Source maps map compiled/bundled/minified code back to original source code. Essential for debugging TypeScript, bundled, or transpiled code:
```javascript
// Enable in Node.js
node --enable-source-maps dist/app.js
```

Shows original file names and line numbers in stack traces.

**💡 Tip:** Source maps = **decoder ring for compiled code**. Always enable in development. Don't expose in production (security risk).

---

### Q39. What is the `zlib` module?

**Answer:** Built-in compression/decompression:
```javascript
const zlib = require('zlib');
// Compress
zlib.gzipSync(buffer); // gzip compression
zlib.deflateSync(buffer); // deflate
zlib.brotliCompressSync(buffer); // brotli (best ratio)
```

Used in HTTP compression middleware (Express `compression()`), file compression, streaming.

**💡 Tip:** `zlib` = **make data smaller**. Gzip for general use. Brotli for web (better ratio). Use streaming versions (`createGzip()`) for large data.

---

### Q40. What is the `dns` module?

**Answer:** DNS lookup and resolution:
```javascript
const dns = require('dns');
dns.lookup('example.com', (err, address) => console.log(address));
dns.resolve4('example.com', (err, addresses) => console.log(addresses));
dns.reverse('93.184.216.34', (err, hostnames) => console.log(hostnames));
```

`lookup` uses OS resolver (sync internally). `resolve` queries DNS server directly (async).

**💡 Tip:** `dns.lookup` = **OS cache** (libuv thread pool). `dns.resolve` = **direct DNS query** (truly async). Prefer `resolve` for multiple lookups.

---

## Advanced Questions (Q41–Q50)

### Q41. What is the V8 engine's role in Node.js?

**Answer:** V8 is Google's JavaScript engine that:
- Compiles JS to machine code (JIT compilation)
- Manages memory (heap, garbage collection)
- Provides the event loop integration point
- Constantly optimized (TurboFan, Maglev compilers)

Node.js embeds V8 and adds APIs (fs, net, http) via C++ bindings.

**💡 Tip:** V8 = **JavaScript executor**. Compiles → optimizes → runs. Node.js wraps V8 with server APIs. V8 updates = Node.js performance improvements.

---

### Q42. What is the Node.js event loop's microtask queue?

**Answer:** Microtasks run between each phase of the event loop:
1. `process.nextTick` callbacks (highest priority)
2. Promise callbacks (`.then`, `.catch`, `.finally`)
3. `queueMicrotask` callbacks

ALL microtasks complete before the event loop moves to the next phase.

**💡 Tip:** Priority: `nextTick` > Promises > everything else. Microtasks can **starve** the event loop if they keep generating more microtasks.

---

### Q43. What is the Node.js performance hook (`perf_hooks`)?

**Answer:** High-resolution performance measurement:
```javascript
const { performance, PerformanceObserver } = require('perf_hooks');
const start = performance.now();
// ... operation
const duration = performance.now() - start;
```

`PerformanceObserver` monitors marks, measures, GC, and HTTP timing.

**💡 Tip:** `perf_hooks` = **precision stopwatch**. `performance.now()` for microbenchmarks. `PerformanceObserver` for automated monitoring.

---

### Q44. What is the `async_hooks` module?

**Answer:** Tracks async resources through their lifecycle:
```javascript
const asyncHooks = require('async_hooks');
const hook = asyncHooks.createHook({
  init(asyncId, type, triggerAsyncId) { /* resource created */ },
  before(asyncId) { /* callback starts */ },
  after(asyncId) { /* callback completes */ },
  destroy(asyncId) { /* resource destroyed */ },
});
hook.enable();
```

Used for: APM tools, request tracing, context propagation.

**💡 Tip:** `async_hooks` = **"track every async operation."** Used by APM tools (New Relic, Datadog). Application-level Request Context (ALS) is built on this.

---

### Q45. What is `AsyncLocalStorage`?

**Answer:** Store context that persists across async operations (like thread-local storage):
```javascript
const { AsyncLocalStorage } = require('async_hooks');
const requestContext = new AsyncLocalStorage();
// Middleware
app.use((req, res, next) => {
  requestContext.run({ traceId: uuid(), userId: req.user?.id }, next);
});
// Anywhere in the call chain
const ctx = requestContext.getStore(); // { traceId, userId }
```

No need to pass request context through every function.

**💡 Tip:** `AsyncLocalStorage` = **"magic backpack"** that follows your request through all async operations. No prop drilling for request context.

---

### Q46. What is graceful shutdown in Node.js?

**Answer:** Properly closing connections when the process is terminating:
```javascript
process.on('SIGTERM', async () => {
  console.log('Shutting down...');
  server.close(); // stop accepting new connections
  await db.disconnect(); // close database
  await redis.quit(); // close redis
  process.exit(0);
});
```

SIGTERM = graceful. SIGINT = Ctrl+C. Always handle both.

**💡 Tip:** Graceful shutdown = **"clean up before leaving."** Close server → drain connections → close DB → exit. PM2/Docker send SIGTERM before SIGKILL.

---

### Q47. What are Node.js native addons?

**Answer:** C/C++ modules that run directly in the Node.js process for performance-critical operations:
- Written in C++ using N-API (stable ABI)
- Compiled with `node-gyp` or `cmake-js`
- Examples: `bcrypt`, `sharp`, `node-sass`

Use when JavaScript is too slow (image processing, cryptography, native integration).

**💡 Tip:** Native addons = **C++ power for Node.js**. Used for performance-critical operations. `sharp` (images), `bcrypt` (passwords) are common examples.

---

### Q48. What is the Node.js `diagnostics_channel` module?

**Answer:** Publish/subscribe channel for diagnostic events without modifying core code:
```javascript
const diagnosticsChannel = require('diagnostics_channel');
const channel = diagnosticsChannel.channel('my-app.request');
channel.subscribe((data) => { /* log/trace */ });
channel.publish({ url, method, duration });
```

APM tools use this to instrument without code changes.

**💡 Tip:** `diagnostics_channel` = **spy on internal events** without modifying code. APM tools (OpenTelemetry) use this for auto-instrumentation.

---

### Q49. What is `node:sea` (Single Executable Applications)?

**Answer:** Bundle Node.js + your app into a single executable:
```bash
node --experimental-sea-config sea-config.json
npx postject my-app NODE_SEA_BLOB sea-prep.blob
```

No Node.js installation needed on the target machine. Uses V8 snapshot.

**💡 Tip:** SEA = **ship one file, no runtime needed**. Like Go binaries. Still experimental. Good for CLI tools and simple deployments.

---

### Q50. What is the `node:watch` (File Watcher)?

**Answer:** Built-in file watching (replacement for `nodemon`):
```bash
node --watch app.js
```

Restarts on file changes. Built into Node.js 19+. Use `--watch-path` for specific directories.

**💡 Tip:** `node --watch` = **built-in nodemon**. No dependency needed. Good for development. Use alongside `--enable-source-maps` for debugging.

---
