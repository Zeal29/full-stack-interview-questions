# NestJS Interview Questions — Top 50

> **Difficulty Split:** Basic (Q1–Q20) | Intermediate (Q21–Q40) | Advanced (Q41–Q50)

---

## Basic Questions (Q1–Q20)

### Q1. What is NestJS?

**Answer:** NestJS is a progressive Node.js framework for building scalable server-side applications using TypeScript. It's heavily inspired by Angular — uses decorators, dependency injection, modules, and follows SOLID principles. Built on top of Express (or Fastify).

**💡 Tip:** NestJS = **Angular for backend**. Decorators, modules, DI, TypeScript-first. Built on Express/Fastify.

---

### Q2. What is the architecture of NestJS?

**Answer:** NestJS follows a modular architecture with these core building blocks:
- **Modules**: organize related features
- **Controllers**: handle incoming requests and return responses
- **Providers/Services**: contain business logic
- **Middleware**: functions executed before route handlers
- **Guards**: authorization/authentication logic
- **Pipes**: data transformation and validation
- **Interceptors**: bind extra logic before/after method execution

**💡 Tip:** NestJS = **Modules + Controllers + Services**. Module = box. Controller = door (receives requests). Service = brain (business logic).

---

### Q3. What is a Module in NestJS?

**Answer:** A module is a class decorated with `@Module()` that organizes related components:
```typescript
@Module({
  imports: [OtherModule],       // other modules needed
  controllers: [UserController], // route handlers
  providers: [UserService],      // services/injections
  exports: [UserService],        // what's shared with other modules
})
export class UserModule {}
```

Every NestJS app has at least one root module (`AppModule`).

**💡 Tip:** Module = **feature folder**. `imports` = what I need. `exports` = what I share. Keep modules focused on one feature.

---

### Q4. What is a Controller in NestJS?

**Answer:** Controllers handle HTTP requests and define routes using decorators:
```typescript
@Controller('users')
export class UserController {
  @Get() findAll(): Promise<User[]> { return this.userService.findAll(); }
  @Get(':id') findOne(@Param('id') id: string): Promise<User> { ... }
  @Post() create(@Body() dto: CreateUserDto): Promise<User> { ... }
  @Delete(':id') remove(@Param('id') id: string): Promise<void> { ... }
}
```

**💡 Tip:** Controller = **traffic cop** — directs requests to the right service. Keep controllers thin; delegate logic to services.

---

### Q5. What is a Provider/Service in NestJS?

**Answer:** Providers are classes that can be injected as dependencies. Services are the most common type:
```typescript
@Injectable()
export class UserService {
  constructor(private readonly userRepo: UserRepository) {}
  async findAll(): Promise<User[]> { return this.userRepo.find(); }
}
```

Registered in module's `providers` array. Injected via constructor.

**💡 Tip:** `@Injectable()` = **"I can be injected."** Services = business logic. Controllers call services, services call repositories.

---

### Q6. What is Dependency Injection (DI) in NestJS?

**Answer:** NestJS has a built-in IoC (Inversion of Control) container. Instead of creating dependencies manually, you declare them in the constructor and NestJS resolves and injects them:
```typescript
constructor(
  private readonly userService: UserService,
  private readonly emailService: EmailService,
) {}
```

NestJS manages the lifecycle — singleton by default.

**💡 Tip:** DI = **"give me what I need, I won't create it myself."** Declare dependencies in constructor, NestJS resolves them. Singleton by default.

---

### Q7. What are DTOs (Data Transfer Objects)?

**Answer:** DTOs define the shape of data for request/response, used with validation:
```typescript
export class CreateUserDto {
  @IsString() @IsNotEmpty() name: string;
  @IsEmail() email: string;
  @MinLength(8) password: string;
}
```

Used with `ValidationPipe` to automatically validate incoming requests.

**💡 Tip:** DTO = **"here's what the data should look like."** Validation + documentation + type safety. Always use DTOs for request bodies.

---

### Q8. What are Pipes in NestJS?

**Answer:** Pipes transform or validate data before it reaches the route handler:
- **ValidationPipe**: validates DTOs using class-validator decorators
- **ParseIntPipe**: converts string param to number
- **Custom pipes**: implement `PipeTransform` interface

```typescript
@Get(':id')
findOne(@Param('id', ParseIntPipe) id: number) { ... }
```

**💡 Tip:** Pipes = **data quality control**. ValidationPipe for request validation. ParseIntPipe for type conversion. Can be global or per-route.

---

### Q9. What are Guards in NestJS?

**Answer:** Guards determine whether a request should be handled (authorization):
```typescript
@Injectable()
export class AuthGuard implements CanActivate {
  canActivate(context: ExecutionContext): boolean {
    const request = context.switchToHttp().getRequest();
    return validateToken(request.headers.authorization);
  }
}
```

Return `true` to allow, `false` to deny (throws 403). Can be async.

**💡 Tip:** Guards = **bouncers** — "can this request proceed?" Used for auth/role checks. Execute AFTER middleware, BEFORE pipes and interceptors.

---

### Q10. What are Interceptors in NestJS?

**Answer:** Interceptors bind extra logic before/after method execution:
```typescript
@Injectable()
export class LoggingInterceptor implements NestInterceptor {
  intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
    console.log('Before...');
    return next.handle().pipe(
      tap(() => console.log('After...')),
    );
  }
}
```

Use cases: logging, caching, response transformation, error mapping, timeout.

**💡 Tip:** Interceptors = **middleware on steroids** — can modify both request AND response. Wrap the handler with `next.handle()`. Think AOP (aspect-oriented programming).

---

### Q11. What is Middleware in NestJS?

**Answer:** Middleware runs before route handlers (similar to Express middleware):
```typescript
@Injectable()
export class LoggerMiddleware implements NestMiddleware {
  use(req: Request, res: Response, next: NextFunction) {
    console.log(`${req.method} ${req.url}`);
    next();
  }
}
```

Registered in module's `configure()` method. Has access to req/res/next.

**💡 Tip:** Middleware = **pre-processing**. Runs before guards and interceptors. Use for logging, CORS, rate limiting. Express-compatible.

---

### Q12. What are Custom Decorators in NestJS?

**Answer:** Create reusable parameter decorators:
```typescript
// Parameter decorator
export const User = createParamDecorator((data, ctx) => {
  const request = ctx.switchToHttp().getRequest();
  return request.user;
});
// Usage: @User() user: UserEntity

// Combined with guard
@Get('profile')
@UseGuards(AuthGuard)
getProfile(@User() user) { return user; }
```

**💡 Tip:** Custom decorators = **DRY for common parameter extraction.** Extract user from request, get specific headers, etc.

---

### Q13. What is Exception Handling in NestJS?

**Answer:** NestJS has a built-in exception layer. Built-in exceptions:
- `HttpException`, `NotFoundException`, `BadRequestException`, `UnauthorizedException`, `ForbiddenException`

```typescript
throw new NotFoundException('User not found');
```

Custom: `throw new HttpException('Custom error', HttpStatus.CONFLICT);`

**💡 Tip:** Always throw NestJS exceptions (not plain Error) — they auto-format to proper HTTP responses. `NotFoundException` = 404, `UnauthorizedException` = 401.

---

### Q14. What are Exception Filters?

**Answer:** Exception filters catch and format errors:
```typescript
@Catch(HttpException)
export class HttpExceptionFilter implements ExceptionFilter {
  catch(exception: HttpException, host: ArgumentHost) {
    const ctx = host.switchToHttp();
    const response = ctx.getResponse();
    response.status(exception.getStatus()).json({
      statusCode: exception.getStatus(),
      message: exception.message,
      timestamp: new Date().toISOString(),
    });
  }
}
```

Can be global, per-controller, or per-route.

**💡 Tip:** Exception filters = **custom error formatting**. Make all errors return consistent JSON. Register globally for uniform responses.

---

### Q15. What is a Custom Provider in NestJS?

**Answer:** When you need more control over DI:
- **Value**: `useValue` — provide a constant or mock
- **Factory**: `useFactory` — dynamic creation with dependencies
- **Class**: `useClass` — conditionally use different classes
- **Existing**: `useExisting` — alias for another provider

```typescript
{ provide: 'CONFIG', useFactory: (env: ConfigService) => env.get('db'), inject: [ConfigService] }
```

**💡 Tip:** Custom providers = **"I control how this is created."** Factory for dynamic config, value for mocks in tests, existing for aliases.

---

### Q16. What is the request lifecycle in NestJS?

**Answer:** Request flows through these stages in order:
1. **Middleware** — pre-processing
2. **Guard** — authorization check
3. **Interceptor (before)** — pre-processing
4. **Pipe** — validation/transformation
5. **Route Handler** — controller method
6. **Interceptor (after)** — post-processing
7. **Exception Filter** — error handling (if error thrown)

**💡 Tip:** Remember: **M-G-I-P-H-I-F** (Middleware → Guard → Interceptor before → Pipe → Handler → Interceptor after → Filter).

---

### Q17. How to handle async operations in NestJS?

**Answer:** Return Promises or Observables — NestJS handles both:
```typescript
// Promise
async findAll(): Promise<User[]> { return this.repo.find(); }
// Observable
findAll(): Observable<User[]> { return from(this.repo.find()); }
```

NestJS automatically resolves Promises and subscribes to Observables.

**💡 Tip:** `async/await` (Promises) = most common. Observables = powerful for RxJS operators (retry, debounce). NestJS handles both seamlessly.

---

### Q18. What is the `@Inject()` decorator?

**Answer:** `@Inject()` is needed for non-class-based injections (custom tokens):
```typescript
constructor(
  @Inject('CONFIG') private config: ConfigType,
  @Inject(REQUEST) private request: Request,
) {}
```

Not needed for class-based providers — TypeScript types are enough.

**💡 Tip:** `@Inject()` = **"inject by this token."** Needed for string tokens, symbols, and circular dependency forward refs. Not needed for regular services.

---

### Q19. What is a Dynamic Module?

**Answer:** Modules that accept configuration at registration:
```typescript
@Module({}) // static
DatabaseModule.forRoot({ host: 'localhost', port: 5432 }) // dynamic
DatabaseModule.forRootAsync({ // async dynamic
  inject: [ConfigService],
  useFactory: (config: ConfigService) => ({ host: config.get('DB_HOST') }),
})
```

Common in third-party modules (TypeORM, Mongoose, Config).

**💡 Tip:** `forRoot()` = **configure once globally** (database connection). `forFeature()` = **register per module** (specific models/repositories).

---

### Q20. What is the ConfigModule in NestJS?

**Answer:** Built-in module for managing configuration:
```typescript
ConfigModule.forRoot({ isGlobal: true, envFilePath: '.env' });
// Usage
constructor(private config: ConfigService) {}
const dbHost = this.config.get<string>('DATABASE_HOST');
```

Loads `.env` files, validates schema, provides typed access. `isGlobal: true` = available everywhere.

**💡 Tip:** `ConfigModule.forRoot({ isGlobal: true })` = **load .env once, use everywhere.** `ConfigService.get()` for typed access. Always validate schema.

---

## Intermediate Questions (Q21–Q40)

### Q21. How to implement authentication in NestJS?

**Answer:** Common approach:
1. **Local strategy**: email/password login with Passport.js
2. **JWT strategy**: issue token, validate on every request
3. **Guards**: `@UseGuards(JwtAuthGuard)` on protected routes
4. **Decorators**: `@CurrentUser()` to extract the authenticated user

Use `@nestjs/passport` with `passport-jwt` or `passport-local`.

**💡 Tip:** Auth flow: Login → validate credentials → issue JWT → client sends JWT in header → Guard validates on each request.

---

### Q22. How to connect NestJS to a database?

**Answer:** Common ORMs/ODMs with NestJS integration:
- **TypeORM**: `@nestjs/typeorm` — SQL databases
- **Prisma**: `prisma` — SQL databases (popular modern choice)
- **Mongoose**: `@nestjs/mongoose` — MongoDB
- **Drizzle**: `drizzle-orm` — lightweight, SQL databases

All follow the pattern: module with `forRoot()` (connection) + `forFeature()` (models/repositories).

**💡 Tip:** Prisma = **most popular modern choice** (type-safe, great DX). TypeORM = mature, decorator-heavy. Choose based on project needs.

---

### Q23. What is Circular Dependency and how to resolve it?

**Answer:** When two services depend on each other. Resolve with `forwardRef`:
```typescript
// Service A
constructor(@Inject(forwardRef(() => ServiceB)) private serviceB: ServiceB) {}
// Module
providers: [ServiceA, { provide: ServiceB, useExisting: forwardRef(() => ServiceB) }]
```

Better solution: extract shared logic to a third service.

**💡 Tip:** Circular dependency = **red flag**. `forwardRef` is the band-aid. The cure = refactor to remove the cycle. Extract shared logic.

---

### Q24. What is the Execution Context?

**Answer:** `ExecutionContext` provides details about the current request:
```typescript
const ctx = host.switchToHttp();
const request = ctx.getRequest<Request>();
const response = ctx.getResponse<Response>();
```

Extended with `RpcContext` and `WsContext` for microservices and WebSockets.

**💡 Tip:** `ExecutionContext` = **"tell me everything about this request."** HTTP, RPC, or WebSocket — same interface.

---

### Q25. How to handle file uploads in NestJS?

**Answer:** Use `@nestjs/platform-express` Multer integration:
```typescript
@Post('upload')
@UseInterceptors(FileInterceptor('file', { storage: diskStorage({ destination: './uploads' }) }))
uploadFile(@UploadedFile() file: Express.Multer.File) {
  return { url: file.path };
}
```

For multiple files: `FilesInterceptor()`, `FileFieldsInterceptor()`, `AnyFilesInterceptor()`.

**💡 Tip:** `FileInterceptor` = single file. `FilesInterceptor` = multiple files. Always validate file type/size with custom pipe.

---

### Q26. What are Event Emitters in NestJS?

**Answer:** Built-in pub/sub for decoupled communication:
```typescript
// Publisher
constructor(private eventEmitter: EventEmitter2) {}
this.eventEmitter.emit('user.created', user);

// Listener
@OnEvent('user.created')
handleUserCreated(payload: User) { /* send welcome email */ }
```

Decouples services — no direct method calls needed.

**💡 Tip:** Event emitters = **"shout it out, whoever cares will listen."** Decouple side effects (emails, notifications) from core logic.

---

### Q27. What is the Schedule module?

**Answer:** Built-in cron job support:
```typescript
@Cron('0 * * * *') // every hour
handleCron() { /* cleanup task */ }

@Interval(5000) // every 5 seconds
handleInterval() { /* check health */ }

@Timeout(10000) // once after 10 seconds
handleTimeout() { /* initial setup */ }
```

Enable with `ScheduleModule.forRoot()`.

**💡 Tip:** `@Cron` = **scheduled tasks**. `@Interval` = **periodic tasks**. `@Timeout` = **one-time delay**. Don't forget `ScheduleModule.forRoot()`.

---

### Q28. What is the CQRS pattern in NestJS?

**Answer:** Command Query Responsibility Segregation — separate read and write operations:
- **Commands**: change state (create, update, delete)
- **Queries**: read state (find, search)
- **Events**: notify about changes

NestJS provides `@nestjs/cqrs` with `CommandBus`, `QueryBus`, and `EventBus`.

**💡 Tip:** CQRS = **"reads and writes are different."** Scale them independently. Good for complex domains, overkill for simple CRUD.

---

### Q29. How to implement rate limiting?

**Answer:** Use `@nestjs/throttler`:
```typescript
// App module
ThrottlerModule.forRoot([{ ttl: 60000, limit: 10 }]),

// Per route
@SkipThrottle() // skip rate limiting
@Throttle({ default: { ttl: 60000, limit: 5 } }) // custom limit
```

Protects against brute force and DDoS attacks.

**💡 Tip:** `ThrottlerModule.forRoot()` = **global rate limit**. `@SkipThrottle()` = **bypass** (webhooks). Adjust per route as needed.

---

### Q30. What is the Health Check (Terminus) module?

**Answer:** Built-in health checks for monitoring:
```typescript
@Get('health')
@HealthCheck()
check() {
  return this.health.check([
    () => this.db.pingCheck('database'),
    () => this.dns.pingCheck('redis'),
  ]);
}
```

Integrates with Kubernetes liveness/readiness probes.

**💡 Tip:** Health checks = **"is my app alive and working?"** DB ping, Redis ping, disk space. Required for Kubernetes deployments.

---

### Q31. How to implement caching in NestJS?

**Answer:** Built-in cache with `CacheModule`:
```typescript
CacheModule.register({ ttl: 60, max: 100 }), // 60s TTL, 100 items
// In service
@CacheKey('users')
@CacheTTL(120)
async findAll() { return this.repo.find(); }
```

Default: in-memory. Use `cache-manager-redis-store` for Redis.

**💡 Tip:** `CacheModule` + Redis = **production caching**. `@CacheTTL()` = cache duration. Cache at the service level, not controller.

---

### Q32. What are Swagger/OpenAPI decorators?

**Answer:** Auto-generate API documentation:
```typescript
@ApiTags('users')
@Controller('users')
export class UserController {
  @ApiOperation({ summary: 'Get all users' })
  @ApiResponse({ status: 200, type: [User] })
  @Get() findAll() { ... }
}
```

Enable with `SwaggerModule.setup('api/docs', app, document)`.

**💡 Tip:** Swagger = **free API docs**. Add decorators → get interactive docs at `/api/docs`. Always document your APIs.

---

### Q33. How to use WebSockets in NestJS?

**Answer:** First-class WebSocket support:
```typescript
@WebSocketGateway()
export class ChatGateway {
  @SubscribeMessage('message')
  handleMessage(@MessageBody() data: string, @ConnectedSocket() client: Socket) {
    client.broadcast.emit('message', data);
  }
}
```

Supports Socket.IO and native WebSockets (ws).

**💡 Tip:** `@WebSocketGateway()` = **real-time communication**. `@SubscribeMessage` = listen for events. `broadcast.emit` = send to all except sender.

---

### Q34. What are Microservices in NestJS?

**Answer:** NestJS supports microservice architecture with different transports:
- **TCP**: default, simple
- **Redis**: pub/sub messaging
- **NATS**: lightweight messaging
- **RabbitMQ/Kafka**: enterprise messaging
- **gRPC**: high-performance RPC

```typescript
const app = await NestFactory.createMicroservice(AppModule, { transport: Transport.RMQ });
```

**💡 Tip:** Microservices in NestJS = **same patterns, different transport**. Same modules/controllers/services. Just change the transport layer.

---

### Q35. What is the Serializer/Class Transformer?

**Answer:** Control what data is returned in responses:
```typescript
@Exclude()
export class UserEntity {
  @Expose() id: number;
  @Expose() name: string;
  @Exclude() password: string; // never returned
}
// Controller: return instanceToPlain(user) or use interceptor
```

Prevents sensitive data (passwords, tokens) from leaking in responses.

**💡 Tip:** `@Exclude()` = **"don't show this."** `@Expose()` = **"show only this."** Always exclude passwords and tokens from responses.

---

### Q36. How to handle validation globally?

**Answer:** Register `ValidationPipe` globally:
```typescript
app.useGlobalPipes(
  new ValidationPipe({
    whitelist: true,        // strips unknown properties
    forbidNonWhitelisted: true, // throws error for unknown props
    transform: true,        // auto-transform types (string → number)
    disableErrorMessages: false,
  }),
);
```

**💡 Tip:** Always set `whitelist: true` (prevents extra fields) and `transform: true` (auto type conversion). Register globally in `main.ts`.

---

### Q37. What is the Logging module?

**Answer:** Built-in logger:
```typescript
constructor(private readonly logger = new Logger(MyService.name)) {}
this.logger.log('User created');
this.logger.warn('Rate limit approaching');
this.logger.error('Database connection failed', error.stack);
```

Custom logger: implement `LoggerService` interface for Winston, Pino, etc.

**💡 Tip:** Use NestJS Logger = **structured logging** with context (class name). Replace with Winston/Pino for production (file rotation, log levels).

---

### Q38. How to test NestJS applications?

**Answer:**
- **Unit tests**: test services in isolation with `jest.mock()`
- **E2E tests**: use `Test.createTestingModule()` to create a test app
- **TestingModule**: replaces real providers with mocks

```typescript
const module = await Test.createTestingModule({
  controllers: [UserController],
  providers: [UserService, { provide: UserRepository, useValue: mockRepo }],
}).compile();
```

**💡 Tip:** `Test.createTestingModule()` = **test sandbox**. Replace real deps with mocks. Test controllers and services independently.

---

### Q39. What is the `@SetMetadata` decorator?

**Answer:** Attaches custom metadata to routes (used by guards):
```typescript
@SetMetadata('roles', ['admin'])
@Get('admin')
getAdminData() { ... }

// In guard
const roles = this.reflector.get<string[]>('roles', context.getHandler());
```

Use custom decorator: `@Roles('admin')` wrapping `@SetMetadata`.

**💡 Tip:** `@SetMetadata` = **attach data to routes**. Guards read it with `Reflector`. Common pattern: `@Roles('admin')` for role-based access.

---

### Q40. How to handle CORS in NestJS?

**Answer:** Enable CORS in `main.ts`:
```typescript
const app = await NestFactory.create(AppModule);
app.enableCors({
  origin: ['https://myapp.com'],
  methods: ['GET', 'POST'],
  credentials: true,
});
```

Or per-route with middleware. Never use `origin: '*'` in production.

**💡 Tip:** Enable CORS in `main.ts`. Restrict `origin` to known domains. `credentials: true` needed for cookies/auth headers.

---

## Advanced Questions (Q41–Q50)

### Q41. What is Module Ref and Dynamic instantiation?

**Answer:** `ModuleRef` dynamically resolve providers at runtime:
```typescript
constructor(private moduleRef: ModuleRef) {}
// Request-scoped resolution
const service = this.moduleRef.get(MyService, { strict: false });
// Dynamic resolution
const service = await this.moduleRef.resolve(TransientService);
```

Use for: plugin systems, dynamic module loading, testing.

**💡 Tip:** `ModuleRef` = **"give me any provider at runtime."** `get()` = singleton, `resolve()` = new instance. Use for dynamic behavior.

---

### Q42. What are Request-scoped providers?

**Answer:** By default, providers are singletons. Request-scoped = new instance per request:
```typescript
@Injectable({ scope: Scope.REQUEST })
export class RequestLogger {
  constructor(@Inject(REQUEST) private request: Request) {}
}
```

Use sparingly — adds overhead (new instance per request). Default singleton is preferred.

**💡 Tip:** Singleton (default) = **shared** (fast). Request-scoped = **per request** (slower). Transient = **per injection** (new every time). Stick with singleton unless you truly need per-request state.

---

### Q43. What is the Lazy-loading modules pattern?

**Answer:** Load modules on demand to improve startup time:
```typescript
const module = await import('./admin.module');
const moduleRef = this.moduleRef.create(module.AdminModule);
```

Use `@nestjs/lazy-module` or manual dynamic import. Reduces initial memory footprint.

**💡 Tip:** Lazy loading = **load when needed**. Admin module doesn't load until someone visits `/admin`. Good for rarely-used features.

---

### Q44. How to implement custom transports?

**Answer:** Create a custom transport strategy by implementing `CustomTransportStrategy`:
```typescript
class CustomTransport extends Server implements CustomTransportStrategy {
  listen(callback: () => void) { /* start listening */ callback(); }
  close() { /* cleanup */ }
}
```

Register as a microservice with custom transporter. Useful for proprietary messaging systems.

**💡 Tip:** Custom transport = **"I have my own messaging system."** Implement the interface, plug into NestJS microservice architecture.

---

### Q45. What is the difference between Express and Fastify in NestJS?

**Answer:**
- **Express** (default): mature, huge ecosystem, more middleware
- **Fastify**: 2-3x faster, schema-based validation, lower overhead

Switch: `NestFactory.create<NestFastifyApplication>(AppModule, new FastifyAdapter())`

Fastify has slightly different middleware patterns.

**💡 Tip:** Express = **safe default, ecosystem**. Fastify = **raw performance**. Switch with one line. Fastify is recommended for high-throughput APIs.

---

### Q46. What is the REPL (Read-Eval-Print Loop) in NestJS?

**Answer:** Interactive debugging shell for NestJS:
```bash
nest repl
# In REPL:
await app.get(UserService).findAll()
```

Inspect services, call methods, debug DI container in real-time. Great for development.

**💡 Tip:** `nest repl` = **playground for your app**. Test services, query database, debug without writing test code.

---

### Q47. What is the GraphQL module in NestJS?

**Answer:** First-class GraphQL support with code-first or schema-first:
```typescript
@Resolver()
export class UserResolver {
  @Query(() => [User])
  users() { return this.userService.findAll(); }
  
  @Mutation(() => User)
  createUser(@Args('input') input: CreateUserInput) { ... }
}
```

Code-first: decorators generate schema. Schema-first: write schema, implement resolvers.

**💡 Tip:** Code-first = **write TypeScript, generate schema** (recommended). Schema-first = **write schema, implement resolvers**. Code-first = less boilerplate.

---

### Q48. How to implement real-time notifications in NestJS?

**Answer:** Combine WebSockets + Event Emitters:
1. WebSocket Gateway for client connections
2. Event Emitter for internal pub/sub
3. Service publishes event → Gateway pushes to client

Or use Server-Sent Events (SSE) for simpler one-way communication.

**💡 Tip:** Real-time = **WebSocket (bidirectional)** or **SSE (server → client only)**. Use WebSocket for chat, SSE for notifications.

---

### Q49. What is the custom server approach?

**Answer:** Use custom Express/Fastify instance:
```typescript
const server = express();
const app = await NestFactory.create(AppModule, new ExpressAdapter(server));
await app.init();
server.listen(3000);
```

Needed for: custom middleware order, integrating with existing Express apps, custom server config.

**💡 Tip:** Custom server = **full control over Express/Fastify**. Use when you need middleware that must run before NestJS.

---

### Q50. How to scale NestJS applications?

**Answer:**
- **Clustering**: use Node.js `cluster` or PM2 for multi-core utilization
- **Microservices**: split into services communicating via message brokers
- **Database**: connection pooling, read replicas
- **Caching**: Redis for response/query caching
- **Rate limiting**: `@nestjs/throttler` for API protection
- **Load balancing**: Nginx/HAProxy in front of multiple instances

**💡 Tip:** Scale = **horizontal first** (more instances), then optimize individual services. PM2 cluster mode = easiest scaling win.

---
