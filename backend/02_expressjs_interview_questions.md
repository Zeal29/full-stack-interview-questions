# Express.js Interview Questions — Top 50 Most Probable Questions

> **Difficulty Split:** 20 Basic (Q1–Q20) | 23 Intermediate (Q21–Q43) | 7 Advanced (Q44–Q50)
> **Sources:** GeeksforGeeks, devinterview.io, Simplilearn, InterviewPrep, FinalRoundAI

---

## 🟢 BASIC (Q1–Q20) — Foundational Concepts

---

### Q1. What is Express.js and how does it relate to Node.js?

**Answer:** Express.js is a minimal, flexible web application framework built on top of Node.js. It simplifies building web apps and APIs by providing routing, middleware support, and HTTP utility methods. Node.js is the runtime; Express is the framework that adds structure and convenience.

**Memory Tip:** 🏗️ **Node.js = engine**, **Express.js = steering wheel**. You can drive without it, but it's much harder.

---

### Q2. What is middleware in Express.js?

**Answer:** Middleware functions have access to `req`, `res`, and `next`. They execute code, modify request/response objects, end the cycle, or call the next middleware. They execute sequentially in the order registered via `app.use()`.

**Memory Tip:** 🔗 Middleware = **assembly line**. Each station (middleware) does its job and passes the product (request) to the next.

---

### Q3. What is the role of `next()` in middleware?

**Answer:** `next()` passes control to the next middleware in the stack. Without calling it, the request hangs. If called with an argument `next(err)`, Express skips to error-handling middleware.

**Memory Tip:** ➡️ `next()` = **"pass it on"**. `next(err)` = **"call the manager"** (error handler).

---

### Q4. What is routing in Express.js?

**Answer:** Routing defines how an application's endpoints (URIs) respond to client requests. Use methods like `app.get()`, `app.post()`, `app.put()`, `app.delete()` to bind HTTP methods to paths and handler functions.

```js
app.get('/users', (req, res) => res.json({users: []}));
```

---

### Q5. What is the difference between `app.use()` and `app.get()`?

**Answer:** `app.use()` mounts middleware functions that run for ALL HTTP methods on matching paths. `app.get()` handles only GET requests for a specific route. `app.use('/api', middleware)` runs middleware for any method under `/api`.

**Memory Tip:** 📌 `app.use()` = **catch-all** (any method). `app.get()` = **specific** (GET only).

---

### Q6. What are the types of middleware in Express?

**Answer:** Five types: **Application-level** (`app.use()`), **Router-level** (`router.use()`), **Error-handling** (4 params: `err, req, res, next`), **Built-in** (`express.json()`, `express.static()`), and **Third-party** (`cors`, `helmet`, `morgan`).

**Memory Tip:** 🖐️ **5 fingers, 5 types**: App, Router, Error, Built-in, Third-party = **A-R-E-B-T**.

---

### Q7. What are built-in middleware functions in Express?

**Answer:** Express ships with: `express.json()` (parse JSON bodies), `express.urlencoded()` (parse URL-encoded bodies), `express.static()` (serve static files), and `express.raw()` (parse raw body as Buffer).

**Memory Tip:** 📦 **J-U-S-R**: **J**son, **U**rlencoded, **S**tatic, **R**aw. The four built-in middlewares.

---

### Q8. What are some popular third-party middleware?

**Answer:** Common ones: `cors` (cross-origin), `helmet` (security headers), `morgan` (logging), `express-rate-limit` (rate limiting), `cookie-parser` (cookies), `express-session` (sessions), `multer` (file uploads), `compression` (gzip).

**Memory Tip:** 🛠️ **C-H-M-R-C-S-M-C** = **C**ors, **H**elmet, **M**organ, **R**ate-limit, **C**ookie, **S**ession, **M**ulter, **C**ompression.

---

### Q9. How do you serve static files in Express?

**Answer:** Use `express.static()` built-in middleware. Pass the directory name to serve files from.

```js
app.use(express.static('public'));
// Files in /public are now accessible at /
```

**Memory Tip:** 📁 `express.static('folder')` = **open the folder** to the web. Like a public file cabinet.

---

### Q10. What is `express.Router()`?

**Answer:** `express.Router()` creates modular, mountable route handlers — a "mini-application." Use it to organize routes into separate files and mount them with `app.use('/prefix', router)`.

```js
const router = express.Router();
router.get('/', (req, res) => res.send('Users'));
app.use('/users', router);
```

**Memory Tip:** 🧩 Router = **mini-Express app**. Group related routes, mount with a prefix.

---

### Q11. What is the difference between `req.query` and `req.params`?

**Answer:** `req.params` extracts values from URL path segments defined with `:` (e.g., `/users/:id` → `req.params.id`). `req.query` extracts query string parameters (e.g., `/users?sort=name` → `req.query.sort`).

**Memory Tip:** 🔍 `params` = **path** segments (`:id`). `query` = **question mark** params (`?sort=name`).

---

### Q12. What is `req.body` and how do you access it?

**Answer:** `req.body` contains the parsed request body data. You must use body-parsing middleware (`express.json()` or `express.urlencoded()`) before accessing it, otherwise it's `undefined`.

**Memory Tip:** 📬 `req.body` = **envelope contents**. Without `express.json()`, the envelope is sealed (undefined).

---

### Q13. How do you handle errors in Express?

**Answer:** Define error-handling middleware with 4 parameters: `(err, req, res, next)`. It must be registered **after** all other middleware and routes. Call `next(err)` from regular middleware/routes to trigger it.

```js
app.use((err, req, res, next) => {
  res.status(err.status || 500).json({ error: err.message });
});
```

**Memory Tip:** 🚨 Error middleware = **4 params** (err, req, res, next). Register it **last**. The `err` param is the key identifier.

---

### Q14. What is the default port for Express?

**Answer:** Express has no fixed default port. Convention is 3000, but you specify any port: `app.listen(process.env.PORT || 3000)`.

---

### Q15. What are route parameters in Express?

**Answer:** Route parameters are named URL segments captured with `:` prefix. Accessed via `req.params`.

```js
app.get('/users/:id', (req, res) => {
  res.send(`User ID: ${req.params.id}`);
});
```

**Memory Tip:** 🎯 `:id` in path = **capture** it. Access with `req.params.id`.

---

### Q16. What are the main features of Express.js?

**Answer:** Routing (map URLs to handlers), middleware (request pipeline), HTTP utility methods, template engine support, static file serving, error handling, and REST API creation.

**Memory Tip:** 🌟 Express = **R-M-H-T-S-E-R** = **R**outing, **M**iddleware, **H**TTP methods, **T**emplates, **S**tatic files, **E**rrors, **R**EST.

---

### Q17. How do you enable CORS in Express?

**Answer:** Install and use the `cors` middleware. For production, whitelist specific origins.

```js
const cors = require('cors');
app.use(cors({ origin: ['https://myapp.com'], credentials: true }));
```

---

### Q18. What are some alternatives to Express.js?

**Answer:** Fastify (faster, schema-based), Koa (lightweight, async middleware), Hapi (configuration-driven), NestJS (opinionated, enterprise), Hono (edge-first). Each has different tradeoffs in speed, features, and architecture.

**Memory Tip:** 🏎️ **Fastify** = speed. **Koa** = minimal. **NestJS** = enterprise. **Hono** = edge.

---

### Q19. How do you handle 404 errors in Express?

**Answer:** Add a catch-all middleware **after** all routes, before error handler:

```js
app.use((req, res, next) => {
  res.status(404).json({ error: 'Route not found' });
});
```

**Memory Tip:** 🚫 404 handler = **"lost and found"** desk. Register it **after all routes**, before error middleware.

---

### Q20. What is the request-response lifecycle in Express?

**Answer:** Incoming request → matched route → middleware stack executes sequentially → route handler → response sent. If middleware calls `next(err)`, jumps to error handler. If middleware calls `res.send()`, cycle ends.

**Memory Tip:** 🔄 **Request → Middleware Chain → Route Handler → Response**. Like a toll road: pay at each booth (middleware) until you reach your exit (handler).

---

## 🟡 INTERMEDIATE (Q21–Q43) — Production Patterns

---

### Q21. What is error-handling middleware and how is it different?

**Answer:** Error-handling middleware has exactly **4 parameters**: `(err, req, res, next)`. Express recognizes it by the 4-parameter signature. It must be registered **last**. Triggered when `next(err)` is called anywhere in the chain.

**Memory Tip:** 🚑 **4 params = error handler**. `err` first is the signal. Register **dead last**.

---

### Q22. How do you implement API versioning in Express?

**Answer:** Common approaches: URL-based (`/v1/users`), header-based (`Accept: application/vnd.myapi.v1+json`), or route prefixing with Express Router.

```js
app.use('/v1', v1Router);
app.use('/v2', v2Router);
```

**Memory Tip:** 📊 URL versioning = **most visible**. Header = **cleanest URLs**. Route prefix = **easiest**.

---

### Q23. What is Helmet.js and why should you use it?

**Answer:** Helmet sets security-related HTTP headers to protect your app: Content-Security-Policy, X-Frame-Options, X-Content-Type-Options, Strict-Transport-Security, etc. Just `app.use(helmet())` adds 15+ security headers.

**Memory Tip:** 🪖 **Helmet** = **head protection** for your app. One line = 15+ security headers.

---

### Q24. How do you implement rate limiting in Express?

**Answer:** Use `express-rate-limit` middleware. Configure a window (time period) and max requests. Apply globally or to specific routes.

```js
const rateLimit = require('express-rate-limit');
app.use('/api', rateLimit({ windowMs: 15*60*1000, max: 100 }));
```

**Memory Tip:** 🚦 Rate limiting = **speed bump**. `windowMs` = time window. `max` = request cap.

---

### Q25. How do you implement file uploads in Express?

**Answer:** Use `multer` middleware for `multipart/form-data`. Configure storage destination, filename, file size limits, and MIME type filters.

```js
const upload = multer({ dest: 'uploads/', limits: { fileSize: 5*1024*1024 } });
app.post('/upload', upload.single('avatar'), (req, res) => {...});
```

**Memory Tip:** 📎 **Multer** handles `multipart/form-data`. `.single()`, `.array()`, `.fields()` for different upload types.

---

### Q26. What is the difference between application-level and router-level middleware?

**Answer:** Application-level is bound to `app` instance (`app.use()`), affects all routes. Router-level is bound to `express.Router()` instance, affects only routes on that router. Router-level is better for modular code.

**Memory Tip:** 🏢 App-level = **building-wide** policy. Router-level = **department** policy.

---

### Q27. How do you handle async errors in Express route handlers?

**Answer:** Express 4 doesn't catch async/await errors automatically. Wrap handlers in try/catch, or use a wrapper function that catches and forwards to `next(err)`. Express 5 handles this natively.

```js
// Wrapper pattern
const asyncHandler = (fn) => (req, res, next) => Promise.resolve(fn(req, res, next)).catch(next);
```

**Memory Tip:** ⚠️ Express 4: async errors **don't propagate** automatically. Use wrapper or try/catch. Express 5 fixes this.

---

### Q28. How do you implement authentication middleware in Express?

**Answer:** Create a middleware that checks for a valid token (JWT) in the `Authorization` header. If valid, attach user data to `req` and call `next()`. If invalid, return 401.

```js
function authMiddleware(req, res, next) {
  const token = req.headers.authorization?.split(' ')[1];
  if (!token) return res.status(401).json({error: 'No token'});
  try {
    req.user = jwt.verify(token, SECRET);
    next();
  } catch { res.status(401).json({error: 'Invalid token'}); }
}
```

---

### Q29. What is the difference between `res.json()` and `res.send()`?

**Answer:** `res.send()` can send various types (string, Buffer, object, Array) and auto-sets Content-Type. `res.json()` specifically sends JSON, sets `Content-Type: application/json`, and uses `JSON.stringify()` internally with replacer/spaces support.

**Memory Tip:** 📤 `res.send()` = **flexible** (any type). `res.json()` = **JSON specific** (always sets JSON header).

---

### Q30. How do you handle JSON body parsing in Express?

**Answer:** Use built-in `express.json()` middleware before your routes: `app.use(express.json())`. This populates `req.body` with parsed JSON. For URL-encoded data, use `express.urlencoded({ extended: true })`.

---

### Q31. What is a route handler? Can you have multiple handlers per route?

**Answer:** A route handler processes a specific route. Yes, Express supports multiple handlers per route using an array or multiple arguments: `app.get('/path', middleware1, middleware2, finalHandler)`. Each must call `next()` to proceed.

**Memory Tip:** 🎯 Multiple handlers = **relay race**. Each runner (handler) passes the baton (`next()`).

---

### Q32. How do you structure an Express application for scalability?

**Answer:** Use MVC or layered pattern: `routes/` → `controllers/` → `services/` → `models/`. Use `express.Router()` for modular routes. Extract middleware into `middleware/`. Use config files for environment settings. Separation of concerns is key.

**Memory Tip:** 📁 **R-C-S-M** pattern: **R**outes → **C**ontrollers → **S**ervices → **M**odels. Each layer has one job.

---

### Q33. What is template engines support in Express?

**Answer:** Express supports template engines like EJS, Pug, Handlebars via `app.set('view engine', 'ejs')`. Use `res.render('template', data)` to render HTML with dynamic data.

**Memory Tip:** 🎨 `view engine` = template language. `res.render()` = render page with data. Like server-side React.

---

### Q34. How do you implement session management in Express?

**Answer:** Use `express-session` middleware with a store (MemoryStore for dev, Redis for production). Configure secret, cookie settings, and TTL. Never use default MemoryStore in production.

```js
const session = require('express-session');
app.use(session({ secret: 'key', resave: false, saveUninitialized: false, store: RedisStore }));
```

**Memory Tip:** 🔐 **Never use MemoryStore in production** (memory leak). Use **Redis** or **connect-redis**.

---

### Q35. How do you prevent XSS attacks in Express?

**Answer:** Use `helmet()` for security headers. Sanitize user input (never trust client data). Set Content-Security-Policy headers. Use `express-validator` to strip HTML tags. Store JWT in `httpOnly` cookies.

**Memory Tip:** 🛡️ Anti-XSS: **Helmet + input sanitization + CSP headers + httpOnly cookies**.

---

### Q36. How do you implement request timeout handling?

**Answer:** Use `connect-timeout` middleware: `app.use(timeout('5s'))`. Check `req.timedout` in handlers. Alternatively, use `res.setTimeout()` or implement in reverse proxy (nginx).

---

### Q37. What is content negotiation in Express?

**Answer:** Content negotiation lets the server return different formats based on the client's `Accept` header. Use `res.format()` to handle multiple content types.

```js
res.format({
  'text/html': () => res.render('page'),
  'application/json': () => res.json(data),
  'default': () => res.status(406).send('Not Acceptable')
});
```

---

### Q38. What is the difference between `res.redirect()` and `res.render()`?

**Answer:** `res.redirect()` sends an HTTP redirect (3xx) to a different URL. `res.render()` compiles a server-side template with data and sends the resulting HTML to the client.

**Memory Tip:** 🔀 `redirect` = **go there instead**. `render` = **draw this page here**.

---

### Q39. How do you secure Express.js applications?

**Answer:** Use Helmet (security headers), CORS (whitelist origins), rate limiting, input validation/sanitization, HTTPS, httpOnly cookies, CSRF protection, parameterized queries, and keep dependencies updated.

**Memory Tip:** 🏰 **H-C-R-V-H-C-C-P-U** = **H**elmet, **C**ORS, **R**ate-limit, **V**alidation, **H**TTPS, **C**ookies(httpOnly), **C**SRF, **P**arameterized queries, **U**pdate deps.

---

### Q40. How do you implement cookie handling in Express?

**Answer:** Use `cookie-parser` middleware: `app.use(require('cookie-parser')())`. Set cookies with `res.cookie('name', 'value', options)`. Read with `req.cookies.name`. Options include `httpOnly`, `secure`, `maxAge`, `sameSite`.

---

### Q41. What is the difference between `res.status()` and `res.sendStatus()`?

**Answer:** `res.status(200).json(data)` sets status code and sends body. `res.sendStatus(200)` sets status and sends the status text as body ("OK"). Use `status()` when you need a custom body; `sendStatus()` for status-only responses.

**Memory Tip:** 📡 `status()` = **custom body**. `sendStatus()` = **just the code** + default text.

---

### Q42. What is the difference between `app.all()` and `app.use()`?

**Answer:** `app.all('/path', handler)` handles ALL HTTP methods but for a specific path pattern only. `app.use('/path', middleware)` also handles all methods but matches any path starting with `/path` (prefix matching). `app.all` requires exact path match.

**Memory Tip:** 🎯 `app.all('/users')` = exact `/users`. `app.use('/users')` = `/users` AND `/users/anything`.

---

### Q43. How do you test Express applications?

**Answer:** Use `supertest` for HTTP assertions and `jest` for test framework. Test route handlers, middleware, and error scenarios.

```js
const request = require('supertest');
const app = require('./app');
test('GET /users returns 200', async () => {
  const res = await request(app).get('/users');
  expect(res.status).toBe(200);
});
```

**Memory Tip:** 🧪 **supertest** = test Express without starting a real server. **jest** = run the tests.

---

## 🔴 ADVANCED (Q44–Q50) — System Design & Performance

---

### Q44. How do you implement a health check endpoint?

**Answer:** Create `/health` that checks critical dependencies (DB, Redis, external APIs). Return 200 if healthy, 503 if degraded. Include uptime, version, and dependency status.

```js
app.get('/health', async (req, res) => {
  const checks = { db: await pingDB(), redis: await pingRedis() };
  const healthy = Object.values(checks).every(v => v === 'ok');
  res.status(healthy ? 200 : 503).json(checks);
});
```

**Memory Tip:** 💚 Health check = **"are you alive?"** endpoint. K8s/ELB use it. Check DB + Redis + critical deps.

---

### Q45. How do you optimize Express.js for production?

**Answer:** Enable gzip compression (`compression`), cache static files, use clustering (PM2), implement connection pooling, enable keep-alive, use reverse proxy (nginx) for static content and SSL termination, minimize middleware on hot paths.

**Memory Tip:** ⚡ Prod optimization = **C-C-C-C-K** = **C**ompression, **C**ache, **C**luster, **C**onnection pool, **K**eep-alive.

---

### Q46. How do you prevent blocking the event loop in Express?

**Answer:** Never run CPU-intensive synchronous work in route handlers. Offload heavy computation to worker threads, use streaming for large data, break up long loops with `setImmediate()`, and use async I/O everywhere.

**Memory Tip:** 🚫 Block event loop = **entire server freezes**. Solution: **worker threads + streaming + async everything**.

---

### Q47. How do you implement streaming responses in Express?

**Answer:** Use Node streams with `res.write()` and `res.end()`, or pipe a Readable stream directly to `res`. Set appropriate headers (`Transfer-Encoding: chunked`). Great for large files, CSV exports, and real-time data.

```js
app.get('/export', (req, res) => {
  res.setHeader('Content-Type', 'text/csv');
  fs.createReadStream('data.csv').pipe(res);
});
```

---

### Q48. What is middleware composition and how does it work?

**Answer:** Middleware composition is chaining multiple middleware functions in a specific order. Express processes them as a stack — each middleware decides to continue (`next()`), respond (`res.send()`), or error (`next(err)`). The order of registration is critical.

**Memory Tip:** 🔗 Middleware order = **matter of life and death**. Auth before business logic. Logging before everything.

---

### Q49. How do you implement WebSocket support alongside Express?

**Answer:** Use `ws` or `socket.io` library. Attach to the same HTTP server instance Express uses. Socket.io integrates seamlessly with Express sessions.

```js
const server = require('http').createServer(app);
const io = require('socket.io')(server);
io.on('connection', (socket) => { socket.on('message', ...); });
server.listen(3000);
```

**Memory Tip:** 🔌 Share the **same HTTP server** for Express + WebSockets. Don't create separate servers.

---

### Q50. How would you migrate an Express app from v4 to v5?

**Answer:** Express 5 key changes: async route handler errors are caught automatically (no wrapper needed), removed deprecated methods (`app.del`, `app.param(fn)`), path matching is more strict, `res.redirect()` no longer falls back. Review the changelog, update middleware, and test thoroughly.

**Memory Tip:** 📦 Express 5 = **native async error handling**. Biggest win: no more `asyncHandler` wrapper needed.

---

*Sources: [GeeksforGeeks](https://www.geeksforgeeks.org/node-js/top-50-express-js-interview-questions-and-answers/), [devinterview.io](https://devinterview.io/blog/express-interview-questions), [Simplilearn](https://www.simplilearn.com/express-js-interview-questions-article), [InterviewPrep](https://interviewprep.org/express-js-interview-questions/)*
