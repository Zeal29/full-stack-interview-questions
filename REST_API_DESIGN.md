# REST API Design Interview Questions — Top 50

> **Difficulty Split:** Basic (Q1–Q20) | Intermediate (Q21–Q40) | Advanced (Q41–Q50)

---

## Basic Questions (Q1–Q20)

### Q1. What is REST?

**Answer:** REST (Representational State Transfer) is an architectural style for designing networked applications. It uses HTTP methods to operate on resources identified by URLs. Key constraints: stateless, client-server, cacheable, uniform interface, layered system.

**💡 Tip:** REST = **6 constraints** for web services. Stateless, URLs = resources, HTTP methods = actions. Not a protocol — a style. Most common API pattern.

---

### Q2. What are the HTTP methods in REST?

**Answer:**
- **GET**: retrieve resource (safe, idempotent)
- **POST**: create resource (not idempotent)
- **PUT**: replace entire resource (idempotent)
- **PATCH**: partial update (not guaranteed idempotent)
- **DELETE**: remove resource (idempotent)
- **OPTIONS**: describe communication options (CORS preflight)

**💡 Tip:** **CRUD → HTTP**: Create=POST, Read=GET, Update=PUT/PATCH, Delete=DELETE. GET and DELETE are idempotent (same result no matter how many times called).

---

### Q3. What is idempotency?

**Answer:** An operation is idempotent if calling it multiple times produces the same result as calling it once:
- **Idempotent**: GET, PUT, DELETE (safe to retry)
- **Not idempotent**: POST (each call may create a new resource)

Critical for retry logic in distributed systems.

**💡 Tip:** Idempotent = **"do it once or 100 times, same result."** GET (read same data), PUT (set same value), DELETE (already gone). POST creates NEW each time.

---

### Q4. What are HTTP status codes?

**Answer:**
- **2xx Success**: 200 OK, 201 Created, 204 No Content
- **3xx Redirection**: 301 Moved Permanently, 302 Found, 304 Not Modified
- **4xx Client Error**: 400 Bad Request, 401 Unauthorized, 403 Forbidden, 404 Not Found, 409 Conflict, 422 Unprocessable, 429 Too Many Requests
- **5xx Server Error**: 500 Internal, 502 Bad Gateway, 503 Unavailable

**💡 Tip:** 2xx = **success**, 4xx = **client's fault**, 5xx = **server's fault**. Know these by heart — 200, 201, 400, 401, 403, 404, 409, 500 are most common.

---

### Q5. What is a resource in REST?

**Answer:** A resource is any information that can be named and represented: users, orders, products. Resources are identified by URIs: `/users/123`. Use **nouns** for resources (not verbs): `/users` not `/getUsers`.

**💡 Tip:** Resource = **noun** (things, not actions). `/users`, `/orders`, `/products`. Never use verbs in URLs: ❌ `/createUser`, ✅ `POST /users`.

---

### Q6. What are the REST naming conventions?

**Answer:**
- Use **plural nouns**: `/users`, `/orders` (not `/user`, `/order`)
- Use **lowercase** with hyphens: `/user-profiles` (not `/userProfiles`)
- **Nested resources**: `/users/123/orders` (user 123's orders)
- **Query params** for filtering: `/users?status=active&role=admin`
- Avoid verbs: ❌ `/getUsers`, ✅ `GET /users`

**💡 Tip:** Plural nouns + lowercase + hyphens. `/users/:id/orders`. Filter with `?key=value`. No verbs in URLs. Be consistent.

---

### Q7. What is the difference between PUT and PATCH?

**Answer:**
- **PUT**: replaces the ENTIRE resource with the provided data. Missing fields = set to null/default.
- **PATCH**: updates ONLY the specified fields. Missing fields = unchanged.

```
PUT /users/1 { "name": "Ali" }  → email becomes null
PATCH /users/1 { "name": "Ali" } → email stays unchanged
```

**💡 Tip:** PUT = **full replacement** (send everything). PATCH = **partial update** (send only what changes). Use PATCH for edits, PUT for full replacements.

---

### Q8. What is a payload and request body?

**Answer:**
- **Request body**: data sent by client (POST/PUT/PATCH)
- **Response body**: data returned by server
- **Headers**: metadata (Content-Type, Authorization, etc.)

Common formats: JSON (`application/json`), form data (`application/x-www-form-urlencoded`), multipart (`multipart/form-data`).

**💡 Tip:** Body = **the actual data**. Headers = **metadata about the data**. Most APIs use JSON. Always set `Content-Type: application/json`.

---

### Q9. What is pagination in REST APIs?

**Answer:** Return large datasets in chunks:
- **Offset-based**: `GET /users?page=2&limit=20` (simple but slow for deep pages)
- **Cursor-based**: `GET /users?cursor=abc123&limit=20` (fast, consistent)
- **Keyset**: `GET /users?after_id=100&limit=20` (fast for ordered data)

Return pagination metadata: `{ data: [], total: 1000, page: 2, totalPages: 50 }`.

**💡 Tip:** Offset = **simple** (skip 20, take 20). Cursor = **scalable** (no skip, just "after this point"). Use cursor for large datasets.

---

### Q10. What is CORS?

**Answer:** Cross-Origin Resource Sharing controls which domains can access your API. Browser security feature:
```
Access-Control-Allow-Origin: https://myapp.com
Access-Control-Allow-Methods: GET, POST, PUT, DELETE
Access-Control-Allow-Headers: Content-Type, Authorization
```

Preflight: browser sends OPTIONS request first. Server must respond with allowed origins/methods.

**💡 Tip:** CORS = **browser's security guard**. Server says "these domains can call me." Without proper headers, browsers block cross-origin requests. Not a server-side security feature — configure it!

---

### Q11. What is rate limiting?

**Answer:** Restrict number of requests per client in a time window:
- **Fixed window**: 100 requests per hour (resets at hour boundary)
- **Sliding window**: 100 requests in any rolling 60-minute period
- **Token bucket**: bursty traffic with average rate limit

Response headers: `X-RateLimit-Limit`, `X-RateLimit-Remaining`, `X-RateLimit-Reset`. Status: `429 Too Many Requests`.

**💡 Tip:** Rate limiting = **"slow down, you're asking too much."** Protects against abuse and DDoS. Use `429` with `Retry-After` header. Stricter for auth routes.

---

### Q12. What is API versioning?

**Answer:**
- **URL versioning**: `/api/v1/users` (most common)
- **Header versioning**: `Accept: application/vnd.myapi.v2+json`
- **Query parameter**: `/api/users?version=2`

Never break existing clients. Maintain at least 2 versions. Deprecation timeline: announce → warn → remove.

**💡 Tip:** URL versioning = **simplest and clearest**. `/v1/` → `/v2/`. Plan for breaking changes. Keep old version alive during migration.

---

### Q13. What is HATEOAS?

**Answer:** Hypermedia as the Engine of Application State — include links in API responses so clients can discover actions:
```json
{
  "id": 1, "name": "Ali",
  "_links": {
    "self": "/api/users/1",
    "orders": "/api/users/1/orders",
    "deactivate": "/api/users/1/deactivate"
  }
}
```

Level 3 REST maturity. Rarely fully implemented in practice.

**💡 Tip:** HATEOAS = **"API tells you what you can do next."** Like a website with links. Nice in theory, rare in practice. Most APIs stop at Level 2.

---

### Q14. What is the difference between 401 and 403?

**Answer:**
- **401 Unauthorized**: not authenticated — who are you? (missing/invalid token)
- **403 Forbidden**: authenticated but not authorized — you can't do this (wrong role/permissions)

**💡 Tip:** 401 = **"I don't know you"** (log in). 403 = **"I know you, but you can't"** (wrong role). Always distinguish — clients need different handling.

---

### Q15. What is content negotiation?

**Answer:** Client and server agree on response format using headers:
```
Accept: application/json         → server returns JSON
Accept: application/xml          → server returns XML
Accept-Language: en-US           → server returns English content
```

Server responds with `Content-Type: application/json`. If unsupported: `406 Not Acceptable`.

**💡 Tip:** Content negotiation = **"what format do you want?"** Client asks via `Accept`, server responds with `Content-Type`. Most APIs just use JSON.

---

### Q16. What is idempotency key?

**Answer:** Client-generated UUID sent with requests to prevent duplicate processing:
```
POST /orders
Idempotency-Key: 550e8400-e29b-41d4-a716-446655440000
```

Server stores key → if same key arrives, returns previous response without reprocessing. Essential for payment APIs.

**💡 Tip:** Idempotency key = **"receipt for this request."** Server remembers: "I already processed this key, here's the cached result." Critical for payments.

---

### Q17. What are query parameters vs path parameters?

**Answer:**
- **Path params**: identify resources — `/users/123` (required, part of URL)
- **Query params**: filter/sort/paginate — `/users?role=admin&sort=name` (optional)

Path = **which resource?** Query = **how to filter/present it?**

**💡 Tip:** Path param = **"which one?"** (`/users/:id`). Query param = **"which subset/how?"** (`?status=active`). Path for identity, query for options.

---

### Q18. What is a REST API contract?

**Answer:** Agreement between client and server defining:
- Endpoints (URLs + methods)
- Request/response formats (JSON schema)
- Status codes
- Authentication requirements
- Rate limits

Documented with OpenAPI/Swagger specifications.

**💡 Tip:** API contract = **"menu of what the API offers."** Use OpenAPI/Swagger to define and share it. Contracts prevent surprises.

---

### Q19. What are API best practices?

**Answer:**
- Use nouns for resources, verbs in HTTP methods
- Return consistent error format: `{ error: string, code: string, details?: any }`
- Use plural resource names: `/users` not `/user`
- Version your API: `/api/v1/`
- Paginate list endpoints
- Use HTTPS always
- Document with OpenAPI/Swagger

**💡 Tip:** Consistency = **most important principle**. Same error format, same pagination style, same naming. Pick conventions and stick to them.

---

### Q20. What is the difference between REST and SOAP?

**Answer:**
| Feature | REST | SOAP |
|---|---|---|
| Format | JSON, XML, any | XML only |
| Protocol | Architectural style | Protocol (strict) |
| State | Stateless | Can be stateful |
| Performance | Lightweight | Heavy (XML envelope) |
| Use case | Web/mobile APIs | Enterprise, banking |

REST dominates modern APIs. SOAP used in legacy enterprise systems.

**💡 Tip:** REST = **modern, lightweight, JSON**. SOAP = **legacy, strict, XML**. Unless working with banks/enterprise legacy, you'll use REST.

---

## Intermediate Questions (Q21–Q40)

### Q21. How to handle errors in REST APIs?

**Answer:** Consistent error response format:
```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid email format",
    "details": [{ "field": "email", "message": "Must be valid email" }]
  }
}
```

Use appropriate status codes. Never return 200 with error body. Include request ID for debugging.

**💡 Tip:** Consistent errors = **lifesaver for debugging.** `code` for programmatic handling, `message` for humans, `details` for specific fields. Always include `requestId`.

---

### Q22. What is API throttling vs rate limiting?

**Answer:**
- **Rate limiting**: hard cutoff — "100 requests per hour, then 429"
- **Throttling**: gradual slowdown — "after 80 requests, add 100ms delay per request"

Throttling degrades gracefully. Rate limiting is binary. Combine both: throttle near limit, reject at limit.

**💡 Tip:** Rate limit = **"stop!"** (hard wall). Throttle = **"slow down!"** (speed bump). Use both: warn with throttling, block with rate limiting.

---

### Q23. What are API gateways?

**Answer:** Single entry point that handles cross-cutting concerns:
- Authentication/authorization
- Rate limiting
- Request routing
- Load balancing
- Logging/monitoring
- SSL termination
- Request/response transformation

Examples: Kong, AWS API Gateway, Nginx, Traefik.

**💡 Tip:** API Gateway = **receptionist** for your services. Handles auth, routing, rate limiting. Services focus on business logic. Essential for microservices.

---

### Q24. How to handle file uploads in REST?

**Answer:**
- **Multipart form data**: `Content-Type: multipart/form-data` for files + metadata
- **Base64 in JSON**: encode file in JSON body (inefficient, 33% size increase)
- **Presigned URL**: client uploads directly to S3 (best for large files)
- **Chunked upload**: split large files into chunks with retry

**💡 Tip:** Small files → multipart. Large files → presigned URL (upload directly to S3). Never base64 encode files — wastes bandwidth.

---

### Q25. What is the Richardson Maturity Model?

**Answer:** Levels of REST compliance:
- **Level 0**: single endpoint, SOAP-like (RPC)
- **Level 1**: resources (multiple URLs)
- **Level 2**: HTTP methods + status codes (most APIs)
- **Level 3**: HATEOAS (hypermedia controls)

Most production APIs are at Level 2.

**💡 Tip:** Level 0 = **one URL**. Level 1 = **multiple URLs**. Level 2 = **HTTP verbs + status codes**. Level 3 = **links in responses**. Aim for Level 2.

---

### Q26. How to handle long-running operations?

**Answer:**
- **202 Accepted**: return immediately with a status URL
- **Polling**: client checks `GET /tasks/123/status` periodically
- **Webhooks**: server calls client URL when done
- **WebSocket/SSE**: real-time status updates

```
POST /reports → 202 { "taskId": "abc", "status": "/tasks/abc" }
GET /tasks/abc → 200 { "status": "processing", "progress": 75 }
GET /tasks/abc → 200 { "status": "completed", "result": "/reports/456" }
```

**💡 Tip:** Long tasks → **202 + polling**. `POST` returns task ID, client polls for status. Webhooks for server-push. Never block the HTTP request.

---

### Q27. What is the difference between RPC and REST?

**Answer:**
- **REST**: resource-oriented, HTTP methods = verbs, URLs = nouns. `GET /users/123`
- **RPC**: action-oriented, function calls over HTTP. `POST /getUser { "id": 123 }`

gRPC = modern RPC (protobuf, HTTP/2, streaming). REST = resource-based. RPC = action-based.

**💡 Tip:** REST = **"operate on things."** RPC = **"do actions."** REST for CRUD. RPC/gRPC for complex operations (compute, transform). Both valid — choose by use case.

---

### Q28. What are API response caching strategies?

**Answer:**
- **ETag**: hash of response — client sends `If-None-Match`, server returns 304 if unchanged
- **Last-Modified**: timestamp — client sends `If-Modified-Since`
- **Cache-Control**: `max-age=3600` (cache for 1 hour), `no-cache`, `private`
- **Redis**: application-level caching for frequent queries

**💡 Tip:** ETag = **"has this changed?"** (hash comparison). Cache-Control = **"how long is this valid?"** Use both. Redis for heavy query caching.

---

### Q29. How to secure REST APIs?

**Answer:**
- **HTTPS**: encrypt all traffic (non-negotiable)
- **Authentication**: JWT, OAuth2, API keys
- **Authorization**: role-based access control (RBAC)
- **Input validation**: validate all inputs server-side
- **Rate limiting**: prevent abuse
- **CORS**: restrict cross-origin access
- **SQL injection**: parameterized queries
- **Helmet**: security headers (XSS, CSRF protection)
- **Request size limits**: prevent oversized payloads

**💡 Tip:** Security = **HTTPS + Auth + Validate + Rate Limit**. Never trust client input. Validate everything server-side. Use parameterized queries.

---

### Q30. What is GraphQL vs REST?

**Answer:**
| Feature | REST | GraphQL |
|---|---|---|
| Endpoints | Multiple (per resource) | Single (`/graphql`) |
| Data fetching | Fixed response shape | Client specifies fields |
| Over-fetching | Common | Eliminated |
| Under-fetching | Multiple requests | Single request |
| Versioning | URL versioning | Schema evolution |

**💡 Tip:** REST = **"here's what I give you."** GraphQL = **"tell me exactly what you want."** REST for simplicity. GraphQL for complex, varied data needs.

---

### Q31. What is API documentation and why does it matter?

**Answer:** Documents how to use the API:
- **OpenAPI/Swagger**: machine-readable specification
- **Tools**: Swagger UI, Redoc, Stoplight
- Include: endpoints, parameters, request/response examples, auth, error codes

Good documentation = faster integration, fewer support requests.

**💡 Tip:** Document as you build. OpenAPI spec → auto-generated interactive docs. Undocumented API = unusable API.

---

### Q32. What is request/response validation?

**Answer:**
- **Request validation**: verify incoming data (types, required fields, ranges)
- **Response validation**: ensure outgoing data matches schema

Use JSON Schema, Zod, Joi, or class-validator. Validate at the API boundary — don't let bad data reach your business logic.

**💡 Tip:** Validate at the **door** (API boundary). Reject bad requests early (400). Never let invalid data reach your services. Use middleware for validation.

---

### Q33. What are soft deletes in REST APIs?

**Answer:** Don't actually delete data — mark as deleted:
```
DELETE /users/123 → sets deleted_at = now()
GET /users → WHERE deleted_at IS NULL
```

Benefits: data recovery, audit trail, analytics. Add `?include_deleted=true` for admin access.

**💡 Tip:** Soft delete = **"hide, don't destroy."** Add `deleted_at` column. Filter by default. Admin can see deleted. Required for financial/legal compliance.

---

### Q34. What are the common API anti-patterns?

**Answer:**
- ❌ Verbs in URLs: `/getUser` → ✅ `GET /users/:id`
- ❌ Return 200 for errors: `{ status: 200, error: "..." }` → ✅ proper status codes
- ❌ No pagination: return all records → ✅ always paginate
- ❌ No versioning: breaking changes → ✅ `/api/v1/`
- ❌ Expose internal IDs (MongoDB ObjectId in URL) → ✅ use UUIDs/slugs
- ❌ No error format standard → ✅ consistent error object

**💡 Tip:** Anti-patterns = **"don't do what I see everyone doing wrong."** Proper methods, proper status codes, pagination, versioning. Consistency is king.

---

### Q35. How to handle concurrent updates (optimistic locking)?

**Answer:**
- **Optimistic**: include version/timestamp, reject if changed:
```json
PUT /users/1 { "name": "Ali", "version": 3 }
// 409 Conflict if current version ≠ 3
```
- **Pessimistic**: lock the row until update completes (database locks)

Optimistic = better for REST (stateless). Pessimistic = better for long transactions.

**💡 Tip:** Optimistic = **"hope no one changed it"** (check version). Pessimistic = **"lock it"** (nobody else can touch). REST uses optimistic (stateless).

---

### Q36. What is API mocking?

**Answer:** Simulate API responses for development/testing before real API is ready:
- **Tools**: Postman Mock Server, WireMock, Prism
- Use OpenAPI spec to generate mocks
- Frontend can develop against mocks while backend builds real API

**💡 Tip:** Mock = **"fake API for development."** Generate from OpenAPI spec. Unblocks frontend while backend is being built. Delete mocks when real API is ready.

---

### Q37. What are webhooks?

**Answer:** Server-to-server notifications via HTTP POST:
```
Client registers: POST /webhooks { "url": "https://myapp.com/callback", "events": ["order.created"] }
Server notifies: POST https://myapp.com/callback { "event": "order.created", "data": {...} }
```

Use for: payment confirmations, CI/CD notifications, third-party integrations.

**💡 Tip:** Webhooks = **"don't call us, we'll call you."** Reverse API — server calls client. Use for async notifications. Always verify webhook signatures.

---

### Q38. What is request tracing?

**Answer:** Track a request across multiple services:
- Generate unique `X-Request-ID` at entry point
- Pass through all services
- Log with request ID for debugging
- Distributed tracing: OpenTelemetry, Jaeger, Zipkin

**💡 Tip:** Request ID = **breadcrumb trail** through your services. Generate at gateway, pass everywhere. Essential for debugging microservices.

---

### Q39. What is API composition vs aggregation?

**Answer:**
- **Composition**: combine multiple API calls into one response (BFF — Backend for Frontend)
- **Aggregation**: merge data from multiple services into a single response

Example: `GET /dashboard` returns user info + recent orders + notifications — composed from 3 services.

**💡 Tip:** Composition = **"one call, many data sources."** Reduces client round trips. Use BFF pattern for frontend-specific data needs.

---

### Q40. How to test REST APIs?

**Answer:**
- **Unit tests**: test individual handlers/services
- **Integration tests**: test with real database (test containers)
- **E2E tests**: test full flow with Supertest/Postman
- **Contract tests**: verify API matches OpenAPI spec
- **Load tests**: performance with k6, Artillery, Locust
- **Security tests**: OWASP ZAP, Burp Suite

**💡 Tip:** Test pyramid: **many unit tests → some integration → few E2E**. Contract tests prevent breaking changes. Load test before launch.

---

## Advanced Questions (Q41–Q50)

### Q41. What is the Backend for Frontend (BFF) pattern?

**Answer:** Create specialized APIs for each client type:
- `/api/mobile/users` — optimized for mobile (less data, smaller images)
- `/api/web/users` — full data for web
- `/api/iot/users` — minimal data for IoT devices

Each BFF composes data from shared services. Avoid one-size-fits-all API.

**💡 Tip:** BFF = **"custom API for each client."** Mobile needs less data. Web needs more. IoT needs minimal. Don't make mobile download desktop-sized responses.

---

### Q42. What is API governance?

**Answer:** Organization-wide standards for API design:
- Naming conventions, error formats, versioning strategy
- Security requirements (auth, rate limiting)
- Documentation standards (OpenAPI required)
- Review process for new endpoints
- Deprecation policy

Ensures consistency across teams and services.

**💡 Tip:** Governance = **"API design rules everyone follows."** Without it, every team designs differently → chaos. Lint APIs with Spectral.

---

### Q43. How to handle distributed transactions in REST?

**Answer:** REST is stateless — can't use traditional 2-phase commit:
- **Saga pattern**: sequential local transactions with compensating actions
- **Outbox pattern**: store events in database, relay to message broker
- **Idempotency keys**: safe retries
- **Eventual consistency**: accept eventual state convergence

**💡 Tip:** No distributed transactions in REST. Use Saga + eventual consistency. Each service handles its own transaction. Compensate on failure.

---

### Q44. What is API observability?

**Answer:** Three pillars of API monitoring:
- **Metrics**: request count, latency, error rate (RED method)
- **Logs**: structured logs with request ID
- **Traces**: distributed tracing across services (OpenTelemetry)

Tools: Prometheus + Grafana, Datadog, New Relic, ELK stack.

**💡 Tip:** Observability = **"know what's happening."** RED: Rate (requests/s), Errors (%), Duration (latency). Alert on SLA breaches (p99 latency > 500ms).

---

### Q45. What is schema evolution in APIs?

**Answer:** Evolving API response without breaking clients:
- **Additive changes** (safe): add new fields, new endpoints
- **Breaking changes** (need version): remove fields, change types, rename fields
- Use `null` for optional new fields
- Deprecation headers: `Sunset: Sat, 1 Jan 2025 00:00:00 GMT`

**💡 Tip:** Add = **safe** (new fields). Remove = **breaking** (version bump). Deprecate with `Sunset` header. Give clients migration time.

---

### Q46. What is the difference between synchronous and asynchronous APIs?

**Answer:**
- **Synchronous**: client waits for response (REST, gRPC). Simple, immediate feedback.
- **Asynchronous**: client gets acknowledgment, result later (webhooks, message queues, SSE). Better for long operations.

Pattern: Sync for quick operations. Async for: report generation, video processing, batch jobs.

**💡 Tip:** Quick operations → **synchronous** (REST). Long operations → **asynchronous** (202 + webhook/polling). Don't block on operations that take >5 seconds.

---

### Q47. What is API fingerprinting and why avoid it?

**Answer:** Servers can identify clients by: User-Agent, TLS fingerprint, header order, IP. While useful for bot detection, it can be used for tracking. Minimize fingerprinting surface for user privacy.

**💡 Tip:** API fingerprinting = **"who are you based on behavior."** Reduce by: minimizing unique headers, standardizing TLS config. Privacy-conscious design.

---

### Q48. How to implement search in REST APIs?

**Answer:**
- **Simple search**: `GET /users?search=ali` (LIKE query)
- **Full-text search**: `GET /users?q=ali&fields=name,email` (Elasticsearch)
- **Filtering**: `GET /users?role=admin&status=active`
- **Sorting**: `GET /users?sort=-created_at,name` (desc by created_at, asc by name)
- **Field selection**: `GET /users?fields=id,name` (reduce payload)

**💡 Tip:** `q` for search, `?key=value` for filters, `sort=-field` for descending. Let clients choose fields. Use Elasticsearch for serious search.

---

### Q49. What is the outbox pattern?

**Answer:** Ensure API events are reliably published:
1. Write to database AND outbox table in same transaction
2. Separate process reads outbox and publishes to message broker
3. Guarantees at-least-once delivery

Solves: "what if database write succeeds but message publish fails?"

**💡 Tip:** Outbox = **"save the event, send it later."** Atomic write (data + event). Background worker publishes. No lost events. Essential for event-driven architectures.

---

### Q50. What makes a great REST API?

**Answer:**
1. **Consistent**: same patterns everywhere
2. **Well-documented**: OpenAPI spec, examples, error codes
3. **Predictable**: follows conventions clients expect
4. **Forgiving**: helpful error messages with guidance
5. **Fast**: proper caching, pagination, indexing
6. **Secure**: HTTPS, auth, validation, rate limiting
7. **Versioned**: plan for changes
8. **Observable**: logging, metrics, tracing
9. **Tested**: unit, integration, contract, load tests
10. **Evolving**: additive changes, deprecation strategy

**💡 Tip:** Great API = **"easy to learn, hard to break."** Consistency + documentation + proper errors + safety nets. Design for the developer consuming it, not just the system behind it.

---
