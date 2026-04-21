# Express.js Interview Questions — Top 50

> **Difficulty Split:** Basic (Q1–Q20) | Intermediate (Q21–Q40) | Advanced (Q41–Q50)

---

## Basic Questions (Q1–Q20)

### Q1. What is Express.js?

**Answer:** Express is a minimal, unopinionated web framework for Node.js that provides robust features for web and mobile applications: routing, middleware, HTTP helpers, and view system. It's the most popular Node.js framework, often called the "de facto standard."

**💡 Tip:** Express = **minimal web framework**. It gives you routing + middleware + HTTP helpers. Everything else (DB, auth, validation) you add as middleware.

---

### Q2. What is middleware in Express?

**Answer:** Middleware functions have access to `req`, `res`, and `next`. They can: execute code, modify request/response, end the cycle, or call `next()` to pass control:
```javascript
app.use((req, res, next) => {
  console.log(`${req.method} ${req.url}`);
  next();
});
```

Express is basically a stack of middleware functions.

**💡 Tip:** Middleware = **assembly line worker**. Receives → processes → passes to next. No `next()` = line stops (response sent). Express IS middleware.

---

### Q3. What is the difference between `app.use()` and `app.METHOD()`?

**Answer:**
- `app.use(path, middleware)`: matches paths that **start with** the given path (prefix matching). Used for middleware.
- `app.get/post/put/delete(path, handler)`: matches **exact** paths and HTTP methods. Used for route handlers.

`app.use('/api', router)` matches `/api`, `/api/users`, `/api/users/1`.

**💡 Tip:** `app.use` = **catch-all middleware** (prefix match). `app.get/post` = **exact route** (method + path). Use `use` for middleware, `get/post` for handlers.

---

### Q4. What is routing in Express?

**Answer:** Routing defines how an app responds to client requests:
```javascript
app.get('/users', (req, res) => res.json(users));
app.post('/users', (req, res) => res.status(201).json(newUser));
app.put('/users/:id', (req, res) => res.json(updatedUser));
app.delete('/users/:id', (req, res) => res.status(204).send());
```

Methods: `get`, `post`, `put`, `patch`, `delete`, `all`.

**💡 Tip:** Routing = **URL → function mapping**. `app.METHOD(path, handler)`. `:id` = dynamic parameter via `req.params.id`.

---

### Q5. How do you handle route parameters?

**Answer:**
- **Route params**: `req.params.id` — from URL path `/users/:id`
- **Query params**: `req.query.search` — from URL `?search=hello`
- **Request body**: `req.body.name` — from POST/PUT body (needs `express.json()`)
- **Headers**: `req.headers.authorization`

**💡 Tip:** `params` = URL path (`:id`). `query` = after `?`. `body` = POST data. `headers` = request headers. Use the right one for the right data.

---

### Q6. What is `express.json()`?

**Answer:** Built-in middleware that parses incoming JSON request bodies:
```javascript
app.use(express.json()); // parses application/json
app.use(express.urlencoded({ extended: true })); // parses form data
```

Without it, `req.body` is `undefined`. Must be placed before routes that need body parsing.

**💡 Tip:** `express.json()` = **"read the body as JSON."** Without it, `req.body` = undefined. Place it BEFORE your routes.

---

### Q7. How do you serve static files in Express?

**Answer:** `express.static` serves files from a directory:
```javascript
app.use(express.static('public')); // serve files from /public
app.use('/uploads', express.static('uploads')); // serve from /uploads at /uploads URL
```

Accessed at `http://localhost:3000/image.jpg` (no `/public` in URL).

**💡 Tip:** `express.static('folder')` = **"serve files from this folder."** Put CSS, JS, images in `public/`. Path is relative to project root.

---

### Q8. What is the difference between `res.json()` and `res.send()`?

**Answer:**
- `res.send(body)`: sends response — auto-detects type (string → HTML, object → JSON, Buffer → binary)
- `res.json(obj)`: explicitly converts to JSON and sets `Content-Type: application/json`

`res.json()` is preferred for API responses. `res.send()` for flexibility.

**💡 Tip:** `res.json()` = **"this is JSON, period."** `res.send()` = **"figure out the type."** For APIs, always use `res.json()` — explicit is better.

---

### Q9. What is Express Router?

**Answer:** `express.Router()` creates modular, mountable route handlers:
```javascript
const router = express.Router();
router.get('/', getUsers);
router.post('/', createUser);
router.get('/:id', getUser);

// In main app
app.use('/api/users', userRouter);
```

Keeps routes organized in separate files. Each router is a mini Express app.

**💡 Tip:** Router = **mini Express app**. Group related routes in separate files. Mount with `app.use('/path', router)`. Essential for large apps.

---

### Q10. How do you handle errors in Express?

**Answer:** Error-handling middleware has 4 parameters `(err, req, res, next)`:
```javascript
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(err.status || 500).json({ error: err.message });
});
```

Must be defined LAST, after all other middleware/routes. Pass errors with `next(err)`.

**💡 Tip:** Error middleware = **4 parameters** (err, req, res, next). Express detects it by the 4th param. Must be last. Call `next(err)` to trigger it.

---

### Q11. What is the `next()` function?

**Answer:** `next()` passes control to the next middleware/handler in the stack:
- `next()` — pass to next middleware
- `next('route')` — skip remaining handlers for this route
- `next(err)` — jump to error-handling middleware

Not calling `next()` = request hangs (unless response sent).

**💡 Tip:** `next()` = **"I'm done, next!"** `next(err)` = **"something's wrong!"** No `next()` and no response = request hangs forever.

---

### Q12. How do you structure an Express application?

**Answer:** Common MVC-like structure:
```
src/
  controllers/    # route handlers
  routes/         # route definitions
  models/         # data models
  middleware/     # custom middleware
  services/       # business logic
  config/         # configuration
  utils/          # helpers
app.js / index.js # entry point
```

**💡 Tip:** Structure = **separate concerns**. Routes define URLs, controllers handle logic, services contain business rules. Don't put everything in one file.

---

### Q13. What is CORS and how to enable it?

**Answer:** CORS (Cross-Origin Resource Sharing) allows or restricts cross-origin requests:
```javascript
const cors = require('cors');
app.use(cors({ origin: 'https://myapp.com', credentials: true }));
```

Without CORS, browsers block requests from different domains. Use the `cors` package.

**💡 Tip:** CORS = **browser security feature**. `cors` package enables it. Restrict `origin` in production — never use `*` with credentials.

---

### Q14. What is body-parser vs express.json()?

**Answer:** `body-parser` was a separate package for parsing request bodies. Since Express 4.16+, `express.json()` and `express.urlencoded()` are built-in (same functionality). No need for the separate `body-parser` package.

**💡 Tip:** Express 4.16+ = **built-in body parsing**. `express.json()` replaces `bodyParser.json()`. No need for the separate package.

---

### Q15. How do you handle file uploads in Express?

**Answer:** Use `multer` middleware:
```javascript
const multer = require('multer');
const upload = multer({ dest: 'uploads/', limits: { fileSize: 5 * 1024 * 1024 } });
app.post('/upload', upload.single('file'), (req, res) => {
  res.json({ file: req.file });
});
```

`single()`, `array()`, `fields()` for different upload scenarios.

**💡 Tip:** Multer = **file upload handler**. Always set `limits` (fileSize). Validate file types with `fileFilter`. Never trust client filenames.

---

### Q16. What is the request-response cycle in Express?

**Answer:** Incoming request → middleware stack (in order) → route handler → response sent:
1. `express.json()` — parse body
2. Custom middleware — logging, auth
3. Route handler — business logic
4. Error middleware — catch errors

Each middleware either responds or calls `next()`.

**💡 Tip:** Request → **middleware chain** → handler → response. Like a waterfall — each step adds something. Order matters!

---

### Q17. What are template engines in Express?

**Answer:** Template engines render dynamic HTML: EJS, Pug, Handlebars:
```javascript
app.set('view engine', 'ejs');
app.get('/', (req, res) => {
  res.render('index', { title: 'My App', users });
});
```

Most backend APIs skip templates (return JSON instead). Used for server-rendered pages.

**💡 Tip:** Template engines = **dynamic HTML**. EJS is closest to HTML. Pug uses indentation. For APIs, skip templates entirely — return JSON.

---

### Q18. How do you handle 404 errors in Express?

**Answer:** Add a catch-all middleware after all routes:
```javascript
// After all routes
app.use((req, res) => {
  res.status(404).json({ error: 'Route not found' });
});
```

Must be defined after all valid routes, before error handler.

**💡 Tip:** 404 catch-all = **last middleware before error handler.** If no route matched, this runs. Order matters — put it AFTER all routes.

---

### Q19. What is `express-session`?

**Answer:** Server-side session management:
```javascript
const session = require('express-session');
app.use(session({
  secret: 'keyboard cat',
  resave: false,
  saveUninitialized: false,
  cookie: { secure: true, httpOnly: true, maxAge: 86400000 }
}));
// Access: req.session.userId = user.id;
```

Stores session data server-side, sends session ID via cookie.

**💡 Tip:** `express-session` = **server-side sessions**. Default: in-memory (not for production). Use Redis/MongoDB store for production.

---

### Q20. What is helmet middleware?

**Answer:** `helmet` sets security-related HTTP headers:
```javascript
const helmet = require('helmet');
app.use(helmet()); // enables all 15 security headers
```

Sets: Content-Security-Policy, X-Frame-Options, X-Content-Type-Options, Strict-Transport-Security, etc.

**💡 Tip:** `helmet` = **security guard for headers**. Always use it. One line = 15 security headers. Zero downside.

---

## Intermediate Questions (Q21–Q40)

### Q21. What is the difference between `res.redirect()` and `res.render()`?

**Answer:**
- `res.redirect('/login')` — sends HTTP redirect (302/301) to a different URL
- `res.render('template', data)` — renders a template with data and sends HTML

Redirect = change URL. Render = generate HTML from template.

**💡 Tip:** Redirect = **"go to this URL."** Render = **"here's the HTML."** Use redirect after form submissions, render for displaying pages.

---

### Q22. What is rate limiting in Express?

**Answer:** Restrict number of requests per time window:
```javascript
const rateLimit = require('express-rate-limit');
app.use(rateLimit({ windowMs: 15 * 60 * 1000, max: 100 })); // 100 req/15min
```

Prevents brute force, DDoS, and abuse. Different limits per route type (stricter for auth).

**💡 Tip:** Rate limiting = **"slow down, too many requests."** `express-rate-limit` is the standard. Stricter limits for auth routes (5 req/min).

---

### Q23. How do you implement authentication middleware?

**Answer:**
```javascript
const auth = (req, res, next) => {
  const token = req.headers.authorization?.split(' ')[1];
  if (!token) return res.status(401).json({ error: 'No token' });
  try {
    req.user = jwt.verify(token, process.env.JWT_SECRET);
    next();
  } catch { res.status(401).json({ error: 'Invalid token' }); }
};
app.get('/profile', auth, (req, res) => res.json(req.user));
```

**💡 Tip:** Auth middleware = **check token → attach user → next()**. Use as second arg in routes: `app.get('/private', auth, handler)`.

---

### Q24. What is `express-winston` or logging in Express?

**Answer:** HTTP request logging:
```javascript
const morgan = require('morgan');
app.use(morgan('combined')); // Apache-style logs
// Or winston for structured logging
const winston = require('winston');
const logger = winston.createLogger({ transports: [new winston.transports.File({ filename: 'app.log' })] });
```

Morgan for HTTP logs, Winston for application logs.

**💡 Tip:** Morgan = **HTTP request logging** (dev/combined format). Winston = **application logging** (errors, info, files). Use both in production.

---

### Q25. What is the `compression` middleware?

**Answer:** Compresses HTTP responses:
```javascript
const compression = require('compression');
app.use(compression()); // gzip compression for all responses
```

Reduces response size by 60-80%. Free performance win. Always use in production.

**💡 Tip:** `compression()` = **free 60-80% bandwidth savings**. One line. Always enable in production. Browsers decompress automatically.

---

### Q26. How do you handle async errors in Express?

**Answer:** Express 4 doesn't catch async errors automatically. Solutions:
```javascript
// Option 1: try/catch
app.get('/users', async (req, res, next) => {
  try { res.json(await getUsers()); }
  catch (err) { next(err); }
});
// Option 2: wrapper function
const asyncHandler = (fn) => (req, res, next) => Promise.resolve(fn(req, res, next)).catch(next);
app.get('/users', asyncHandler(async (req, res) => res.json(await getUsers())));
```

Express 5 catches async errors automatically.

**💡 Tip:** Express 4 = **wrap async routes**. Express 5 = **automatic async error catching**. Use `express-async-errors` package for Express 4.

---

### Q27. What is the `morgan` middleware?

**Answer:** HTTP request logger middleware:
```javascript
const morgan = require('morgan');
app.use(morgan('dev'));     // colored, concise (development)
app.use(morgan('combined')); // Apache standard (production)
app.use(morgan('tiny'));     // minimal
```

Logs method, URL, status, response time, content length.

**💡 Tip:** Morgan = **access log for Express**. `dev` for development, `combined` for production. Stream to file in production.

---

### Q28. How do you handle validation in Express?

**Answer:** Use validation libraries:
```javascript
const { body, validationResult } = require('express-validator');
app.post('/users',
  body('email').isEmail(),
  body('password').isLength({ min: 8 }),
  (req, res) => {
    const errors = validationResult(req);
    if (!errors.isEmpty()) return res.status(400).json({ errors: errors.array() });
    // process
  }
);
```

**💡 Tip:** `express-validator` = **request validation middleware**. Chain validators. Always check `validationResult()` before processing.

---

### Q29. What is `cookie-parser`?

**Answer:** Parses cookies from the `Cookie` header:
```javascript
const cookieParser = require('cookie-parser');
app.use(cookieParser('secret')); // signed cookies
req.cookies.sessionId; // read cookie
res.cookie('token', jwt, { httpOnly: true, secure: true, maxAge: 86400000 });
```

**💡 Tip:** `cookie-parser` = **read cookies**. Use `httpOnly` (no JS access), `secure` (HTTPS only), `sameSite: 'strict'` for security.

---

### Q30. How to implement JWT authentication in Express?

**Answer:**
```javascript
// Login → issue token
const token = jwt.sign({ userId: user.id }, process.env.JWT_SECRET, { expiresIn: '7d' });
res.json({ token });

// Middleware → verify token
const auth = (req, res, next) => {
  const token = req.headers.authorization?.split(' ')[1];
  req.user = jwt.verify(token, JWT_SECRET);
  next();
};
```

Store token in httpOnly cookies for better security.

**💡 Tip:** JWT auth = **issue on login, verify on every request**. `Authorization: Bearer <token>` header. Use httpOnly cookies, not localStorage, for web apps.

---

### Q31. What is the difference between Express and Koa?

**Answer:**
- **Express**: callback-based middleware (`req, res, next`), larger ecosystem, more middleware
- **Koa**: `async/await` middleware (`ctx, next`), no built-in routing, lighter, uses `context` object

Koa is made by Express team — more modern but less ecosystem.

**💡 Tip:** Express = **battle-tested, huge ecosystem**. Koa = **cleaner async model, smaller ecosystem**. Most companies use Express.

---

### Q32. How do you handle pagination in Express APIs?

**Answer:**
```javascript
app.get('/users', async (req, res) => {
  const page = parseInt(req.query.page) || 1;
  const limit = parseInt(req.query.limit) || 10;
  const skip = (page - 1) * limit;
  const [users, total] = await Promise.all([
    User.find().skip(skip).limit(limit),
    User.countDocuments()
  ]);
  res.json({ data: users, page, totalPages: Math.ceil(total / limit), total });
});
```

**💡 Tip:** Pagination = `?page=1&limit=10`. `skip = (page - 1) * limit`. Always return total count for UI pagination controls.

---

### Q33. What is the `method-override` middleware?

**Answer:** Allows using HTTP verbs (PUT, DELETE) where the client doesn't support them:
```javascript
const methodOverride = require('method-override');
app.use(methodOverride('_method')); // ?_method=DELETE
// HTML form: <form method="POST" action="/resource?_method=DELETE">
```

Useful for HTML forms that only support GET/POST.

**💡 Tip:** `method-override` = **fake HTTP methods in forms**. HTML forms only support GET/POST. This adds PUT/DELETE/PATCH support.

---

### Q34. What is the `http-errors` package?

**Answer:** Creates HTTP errors with status codes:
```javascript
const createError = require('http-errors');
app.get('/user/:id', (req, res, next) => {
  const user = findUser(req.params.id);
  if (!user) return next(createError(404, 'User not found'));
  res.json(user);
});
```

Cleaner than `new Error()` + manual status code.

**💡 Tip:** `http-errors` = **typed errors with status codes**. `createError(404, 'Not found')` → error handler picks up status + message automatically.

---

### Q35. How to make Express TypeScript-friendly?

**Answer:**
```typescript
import { Request, Response, NextFunction } from 'express';
interface AuthRequest extends Request {
  user?: { id: string; role: string };
}
app.get('/profile', (req: AuthRequest, res: Response, next: NextFunction) => {
  res.json(req.user);
});
```

Or use `express-async-errors` + proper typing.

**💡 Tip:** Extend `Request` interface for custom properties (`user`, `traceId`). Type routes as `(req: Request, res: Response) => void`.

---

### Q36. What is the `express-async-errors` package?

**Answer:** Patches Express to automatically catch async errors:
```javascript
require('express-async-errors'); // import before app
app.get('/users', async (req, res) => {
  const users = await User.find(); // if error, auto-passed to error middleware
  res.json(users);
});
```

No need for try/catch or wrapper functions in every route.

**💡 Tip:** `express-async-errors` = **one import, no more try/catch in routes**. Import once at app entry point. Error middleware catches everything.

---

### Q37. What is request pooling in Express?

**Answer:** Use `agentkeepalive` for outgoing HTTP requests:
```javascript
const Agent = require('agentkeepalive');
const agent = new Agent({ maxSockets: 100, keepAlive: true });
http.get({ hostname: 'api.example.com', agent }, callback);
```

Reuses TCP connections for better performance when making many outgoing requests.

**💡 Tip:** Connection pooling = **reuse connections** instead of creating new ones. Reduces latency and resource usage for high-throughput APIs.

---

### Q38. How to handle websockets in Express?

**Answer:** Use `socket.io` with Express:
```javascript
const server = http.createServer(app);
const io = new Server(server);
io.on('connection', (socket) => {
  socket.on('message', (data) => io.emit('message', data));
});
server.listen(3000); // use server, not app
```

**💡 Tip:** Create HTTP server from Express app, attach Socket.IO to it. Listen on the server, not the Express app directly.

---

### Q39. What is the `trust proxy` setting?

**Answer:** Tells Express the app is behind a proxy (Nginx, Load Balancer):
```javascript
app.set('trust proxy', 1); // trust first proxy
req.ip; // real client IP (not proxy IP)
req.protocol; // 'https' (original protocol)
```

Required for correct IP addresses and secure cookies behind load balancers.

**💡 Tip:** `trust proxy` = **"I'm behind a load balancer, trust the forwarded headers."** Without it, `req.ip` = load balancer IP, not client IP.

---

### Q40. How to test Express applications?

**Answer:** Use `supertest` for HTTP testing:
```javascript
const request = require('supertest');
const app = require('../app');
test('GET /users returns 200', async () => {
  const res = await request(app).get('/users');
  expect(res.status).toBe(200);
  expect(Array.isArray(res.body)).toBe(true);
});
```

No need to start the server — supertest handles it.

**💡 Tip:** `supertest` = **test Express without starting server**. Pass `app` directly. Chain `.get()`, `.post()`, `.set()`, `.send()`.

---

## Advanced Questions (Q41–Q50)

### Q41. How does Express middleware stack work internally?

**Answer:** Express maintains a stack of middleware functions. When a request arrives, it iterates through the stack:
1. Checks if the middleware matches the path/method
2. If match: execute the function
3. If `next()` called: move to next middleware
4. If response sent or error thrown: stop iterating

The stack is processed sequentially in registration order.

**💡 Tip:** Middleware = **ordered array of functions**. Registration order = execution order. Path matching is prefix for `use`, exact for routes.

---

### Q42. What is the difference between Express 4 and Express 5?

**Answer:** Key changes in Express 5:
- Async errors are automatically caught (no wrapper needed)
- Path matching uses `path-to-regexp` v8 (stricter syntax)
- Removed deprecated methods (`app.del`, `req.param`)
- `res.redirect()` uses 302 instead of 301 by default
- Better TypeScript support

**💡 Tip:** Express 5 = **async-safe** (biggest change). Auto-catches async errors. Stricter routing. Currently in alpha/release candidate.

---

### Q43. How to implement API versioning in Express?

**Answer:**
```javascript
// URL-based
app.use('/v1', v1Router);
app.use('/v2', v2Router);

// Header-based
app.use((req, res, next) => {
  const version = req.headers['api-version'] || 'v1';
  req.version = version;
  next();
});

// Custom router that selects handler based on version
```

**💡 Tip:** URL versioning = **simplest** (`/v1/users`). Header versioning = **cleaner URLs**. Start with URL versioning.

---

### Q44. What is the `content-negotiation` in Express?

**Answer:** Respond with different formats based on `Accept` header:
```javascript
app.get('/users', (req, res) => {
  res.format({
    'application/json': () => res.json(users),
    'text/xml': () => res.type('xml').send(usersToXml(users)),
    'text/html': () => res.render('users', { users }),
    'default': () => res.status(406).send('Not Acceptable'),
  });
});
```

**💡 Tip:** `res.format()` = **respond in the format the client wants**. Check `Accept` header automatically. Useful for APIs that serve multiple formats.

---

### Q45. How to implement rate limiting per user/IP?

**Answer:**
```javascript
const rateLimit = require('express-rate-limit');
const limiter = rateLimit({
  windowMs: 15 * 60 * 1000,
  max: 100,
  keyGenerator: (req) => req.user?.id || req.ip,
  handler: (req, res) => res.status(429).json({ error: 'Too many requests' }),
});
app.use('/api', limiter);
```

Use user ID for authenticated routes, IP for public routes.

**💡 Tip:** Rate limit by **user ID** (authenticated) or **IP** (anonymous). `429 Too Many Requests` with `Retry-After` header.

---

### Q46. What is the `sub-app` pattern in Express?

**Answer:** Express apps can be nested:
```javascript
const adminApp = express();
adminApp.get('/dashboard', adminDashboard);
app.use('/admin', adminApp); // mount sub-app
```

Sub-apps have their own middleware stack, settings, and routing. Like mini applications inside the main app.

**💡 Tip:** Sub-app = **Express inside Express**. Has its own middleware, settings, routes. Use for multi-tenant apps or admin panels.

---

### Q47. How to handle long-running requests in Express?

**Answer:** Options:
- **Streaming**: `res.write()` + `res.end()` for progressive responses
- **Server-Sent Events**: one-way real-time updates
- **Background jobs**: respond immediately, process in background (Bull, Agenda)
- **WebSockets**: for bidirectional real-time communication
- **Timeout**: `req.setTimeout(30000)` for longer timeouts

**💡 Tip:** Long-running ≠ blocking. Stream responses, use background jobs, or WebSockets. Never block the event loop.

---

### Q48. What is the `x-powered-by` header and why disable it?

**Answer:** Express sends `X-Powered-By: Express` by default — reveals server technology to attackers. Disable it:
```javascript
app.disable('x-powered-by');
// Or use helmet which disables it automatically
app.use(helmet());
```

**💡 Tip:** `x-powered-by` = **"Hi attacker, I use Express!"** Always disable it. `helmet()` does this automatically.

---

### Q49. How to implement health checks in Express?

**Answer:**
```javascript
app.get('/health', (req, res) => {
  res.json({ status: 'ok', uptime: process.uptime(), timestamp: new Date().toISOString() });
});
app.get('/ready', async (req, res) => {
  try {
    await db.raw('SELECT 1');
    await redis.ping();
    res.json({ status: 'ready' });
  } catch { res.status(503).json({ status: 'not ready' }); }
});
```

`/health` = liveness (is process running?). `/ready` = readiness (can it serve traffic?).

**💡 Tip:** `/health` = **alive?** (liveness probe). `/ready` = **ready?** (check DB, Redis). Kubernetes uses both probes.

---

### Q50. How to scale Express applications?

**Answer:**
- **Cluster mode**: use all CPU cores (`cluster` module or PM2)
- **Reverse proxy**: Nginx for load balancing, SSL, static files
- **Horizontal scaling**: multiple instances behind a load balancer
- **Stateless design**: store sessions in Redis, not memory
- **Caching**: Redis for response/query caching
- **Connection pooling**: for database connections
- **Rate limiting**: protect against abuse

**💡 Tip:** Scale = **stateless + multiple instances**. PM2 cluster mode = easiest win. Redis for sessions/cache. Nginx as reverse proxy.

---
