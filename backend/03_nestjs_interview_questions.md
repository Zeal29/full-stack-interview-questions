# NestJS Interview Questions — Top 50 Most Probable Questions

> **Difficulty Split:** 20 Basic (Q1–Q20) | 23 Intermediate (Q21–Q43) | 7 Advanced (Q44–Q50)
> **Sources:** NestJS Docs, Digiqt, gasangw/NestJS-Interview-Questions, SharpSkill, Medium, MoldStud

---

## 🟢 BASIC (Q1–Q20) — Foundational Concepts

---

### Q1. What is NestJS?

**Answer:** NestJS is a progressive Node.js framework for building efficient, scalable server-side applications. It's built on top of Express (or Fastify) and fully supports TypeScript. It's heavily inspired by Angular and uses decorators, modules, dependency injection, and an object-oriented approach.

**Memory Tip:** 🏗️ **N**estJS = **N**ode + **E**xpress + **S**tructure + **T**ypeScript. Think of it as "Angular for backend."

---

### Q2. What are the main components of a NestJS application?

**Answer:** Three core components: **Modules** (organize related features into cohesive blocks), **Controllers** (handle incoming HTTP requests and return responses), and **Providers/Services** (handle business logic and data access, injected via DI).

**Memory Tip:** 🧱 **M-C-P** = **M**odules (container), **C**ontrollers (traffic cop), **P**roviders (worker).

---

### Q3. What is a Module in NestJS?

**Answer:** A module is a class annotated with `@Module()` decorator that organizes application structure. It groups related controllers, providers, and imports/exports into a cohesive feature unit. Every NestJS app has at least one root module (`AppModule`).

```js
@Module({
  imports: [OtherModule],
  controllers: [UsersController],
  providers: [UsersService],
  exports: [UsersService],
})
export class UsersModule {}
```

**Memory Tip:** 📦 Module = **box** that groups related things together. `imports` = what you need. `exports` = what you share.

---

### Q4. What is a Controller in NestJS?

**Answer:** A controller handles incoming HTTP requests and returns responses. Annotated with `@Controller('route')`, it uses method decorators (`@Get()`, `@Post()`, `@Put()`, `@Delete()`, `@Patch()`) to define route handlers.

```js
@Controller('users')
export class UsersController {
  @Get() findAll() { return this.usersService.findAll(); }
  @Get(':id') findOne(@Param('id') id: string) { ... }
  @Post() create(@Body() dto: CreateUserDto) { ... }
}
```

**Memory Tip:** 🎮 Controller = **game controller** — it receives input (requests) and triggers actions.

---

### Q5. What is a Provider/Service in NestJS?

**Answer:** A provider is any class annotated with `@Injectable()` that can be injected as a dependency. Services are the most common type of provider — they contain business logic, interact with databases, and are injected into controllers via constructor.

**Memory Tip:** 💉 Provider = **injectable thing**. Service = **provider with business logic**. `@Injectable()` = "I can be injected."

---

### Q6. What is Dependency Injection in NestJS?

**Answer:** DI is a design pattern where a class receives its dependencies from an external source (IoC container) rather than creating them. NestJS has a built-in IoC container that resolves dependencies at runtime via constructor parameters. This enables modularity, testability, and loose coupling.

```js
@Controller('users')
export class UsersController {
  constructor(private usersService: UsersService) {} // Injected automatically
}
```

**Memory Tip:** 💉 DI = **"Don't fetch, I'll deliver."** The IoC container injects dependencies. Constructor = injection point.

---

### Q7. What is the difference between `@Injectable()` and `@Inject()`?

**Answer:** `@Injectable()` marks a CLASS as a provider that can be managed by the DI container. `@Inject()` is used inside a constructor to inject a SPECIFIC dependency by token — needed for non-class providers (values, factories) or when TypeScript reflection can't infer the type.

**Memory Tip:** 🏷️ `@Injectable()` = **"I am available for injection"** (class-level). `@Inject()` = **"Give me THIS specific thing"** (parameter-level).

---

### Q8. What are DTOs (Data Transfer Objects)?

**Answer:** DTOs define the shape of data exchanged between layers — typically request bodies. They enable validation (with `class-validator`), type safety, and Swagger documentation. Used with `@Body()` decorator in controllers.

```js
export class CreateUserDto {
  @IsString() name: string;
  @IsEmail() email: string;
  @IsInt() @Min(18) age: number;
}
```

**Memory Tip:** 📋 DTO = **contract** for data shape. Validates input + documents API + catches errors early.

---

### Q9. What are Decorators in NestJS?

**Answer:** Decorators are special functions prefixed with `@` that add metadata to classes, methods, or properties. NestJS provides built-in decorators: class-level (`@Controller`, `@Module`, `@Injectable`), method-level (`@Get`, `@Post`), parameter-level (`@Body`, `@Param`, `@Query`, `@Req`), and you can create custom ones.

**Memory Tip:** 🎀 Decorators = **labels** that tell NestJS what to do with a class/method/parameter. `@Get()` = "handle GET requests."

---

### Q10. What are Pipes in NestJS?

**Answer:** Pipes transform or validate data before it reaches the route handler. They implement `PipeTransform` interface. Built-in pipes: `ValidationPipe`, `ParseIntPipe`, `ParseUUIDPipe`, `ParseBoolPipe`, `ParseFloatPipe`, `ParseArrayPipe`, `ParseEnumPipe`, `DefaultValuePipe`, `ParseFilePipe`.

```js
@Get(':id')
findOne(@Param('id', ParseIntPipe) id: number) { ... }
```

**Memory Tip:** 🔧 Pipe = **plumber** — either **transforms** data (string → int) or **validates** it (reject bad input). Runs before handler.

---

### Q11. What are Guards in NestJS?

**Answer:** Guards determine whether a request should be handled by the route handler based on conditions (permissions, roles, authentication). They implement `CanActivate` interface and return `boolean | Promise<boolean>`. They execute AFTER middleware but BEFORE pipes and interceptors.

```js
@Injectable()
export class AuthGuard implements CanActivate {
  canActivate(context: ExecutionContext): boolean {
    const request = context.switchToHttp().getRequest();
    return !!request.user; // true = allow, false = deny
  }
}
```

**Memory Tip:** 🛡️ Guard = **bouncer** at the club. Returns `true` = you're in. `false` = denied (403).

---

### Q12. What are Interceptors in NestJS?

**Answer:** Interceptors wrap method execution using RxJS observables. They can execute logic before/after a handler, transform the response, handle errors, or implement caching/logging. They implement `NestInterceptor` interface and use `rxjs` `tap`, `map`, `catchError` operators.

```js
@Injectable()
export class LoggingInterceptor implements NestInterceptor {
  intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
    const now = Date.now();
    return next.handle().pipe(tap(() => console.log(`Took ${Date.now()-now}ms`)));
  }
}
```

**Memory Tip:** 🎯 Interceptor = **wrapper** around handler. Before + after logic. RxJS observable chain. Think "AOP (Aspect-Oriented Programming)."

---

### Q13. What is Middleware in NestJS?

**Answer:** NestJS middleware (similar to Express) runs BEFORE route handlers. They have access to `req`, `res`, `next`. Implemented as a class implementing `NestMiddleware` or as a simple function. Registered in module `configure()` method.

**Memory Tip:** 🚪 Middleware = **first door** the request hits. Executes before guards, pipes, and interceptors.

---

### Q14. What is the difference between Interceptors and Middleware?

**Answer:** Middleware is HTTP-specific, runs before route handlers, and can't modify the response. Interceptors work with HTTP, WebSockets, AND microservices, run around the handler (before + after), can transform responses, and use RxJS observables.

**Memory Tip:** 📊 Middleware = **before only, HTTP only**. Interceptor = **before + after, all transports**.

---

### Q15. What is the request lifecycle order in NestJS?

**Answer:** Incoming request → **Middleware** → **Guard** → **Interceptor (before)** → **Pipe** → **Route Handler** → **Interceptor (after)** → **Response**.

**Memory Tip:** 🔄 **M-G-I-P-H-I-R** = **M**iddleware → **G**uard → **I**nterceptor(before) → **P**ipe → **H**andler → **I**nterceptor(after) → **R**esponse. "My Good Inspector Patrolled Here Immediately Right."

---

### Q16. What is the `@Body()` decorator?

**Answer:** Extracts the entire body from the incoming HTTP request. Commonly used with POST/PUT/PATCH routes. Usually paired with a DTO for type safety and validation: `@Body() createUserDto: CreateUserDto`.

---

### Q17. What is the `@Param()` decorator?

**Answer:** Extracts route parameters from the URL. Used with dynamic routes: `@Param('id') id: string` extracts `:id` from `/users/:id`.

---

### Q18. What is the `@Query()` decorator?

**Answer:** Extracts query string parameters from the URL. `@Query('page') page: string` extracts `page` from `/users?page=2`.

---

### Q19. What is the entry file of a NestJS application?

**Answer:** `main.ts` is the entry point. It creates the Nest application with `NestFactory.create(AppModule)` and optionally configures global pipes, filters, CORS, and starts listening.

```js
async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  app.useGlobalPipes(new ValidationPipe());
  await app.listen(3000);
}
bootstrap();
```

---

### Q20. How do you create a new NestJS project?

**Answer:** Install CLI globally: `npm i -g @nestjs/cli`. Create project: `nest new project-name`. Generate components: `nest g module users`, `nest g controller users`, `nest g service users`, or generate full CRUD: `nest g resource users`.

**Memory Tip:** 🛠️ `nest g` = **generate**. `resource` = full CRUD scaffold. `module/controller/service` = individual pieces.

---

## 🟡 INTERMEDIATE (Q21–Q43) — Production Patterns

---

### Q21. What is the `@Module()` decorator and its properties?

**Answer:** `@Module()` takes an object with: `controllers` (route handlers), `providers` (services/repositories), `imports` (other modules needed), `exports` (providers shared with importing modules). It organizes the dependency graph.

**Memory Tip:** 📦 Module = **C-P-I-E** = **C**ontrollers, **P**roviders, **I**mports, **E**xports. Like a box: contains stuff, needs stuff, shares stuff.

---

### Q22. How does NestJS handle dependency injection scopes?

**Answer:** Three scopes: **DEFAULT** (singleton — one instance shared app-wide), **REQUEST** (new instance per HTTP request), **TRANSIENT** (new instance every time injected). Set via `@Injectable({ scope: Scope.REQUEST })`.

**Memory Tip:** 🎯 **Singleton** = one for all. **Request** = one per request. **Transient** = new every time. Default = singleton (most efficient).

---

### Q23. What are Custom Providers in NestJS?

**Answer:** Custom providers go beyond `@Injectable()` classes. Types: **Value providers** (`useValue`), **Factory providers** (`useFactory` for dynamic creation), **Class providers** (`useClass`), and **Alias providers** (`useExisting`). Useful for configurations, interfaces, and dynamic dependencies.

```js
{ provide: 'CONFIG', useValue: { dbUrl: '...' } }
{ provide: 'LOGGER', useFactory: () => new LoggerService() }
```

**Memory Tip:** 🏭 Custom providers = **V-F-C-A** = **V**alue, **F**actory, **C**lass, **A**lias. Use when `@Injectable()` isn't enough.

---

### Q24. How do you handle errors/exceptions in NestJS?

**Answer:** Use built-in exception classes (`NotFoundException`, `BadRequestException`, `UnauthorizedException`, etc.) or create custom ones extending `HttpException`. Use Exception Filters (`@Catch()`) for custom error handling logic.

```js
throw new NotFoundException('User not found');
throw new HttpException('Custom error', HttpStatus.FORBIDDEN);
```

**Memory Tip:** 🚨 Throw **specific** exceptions, not generic ones. `NotFoundException` > `HttpException(404)`.

---

### Q25. What are Exception Filters?

**Answer:** Exception Filters catch and handle exceptions. Create custom filters implementing `ExceptionFilter` with `@Catch()` decorator. They can log errors, transform responses, or implement custom error formats.

```js
@Catch(HttpException)
export class HttpExceptionFilter implements ExceptionFilter {
  catch(exception: HttpException, host: ArgumentHost) {
    const ctx = host.switchToHttp();
    const response = ctx.getResponse();
    response.status(exception.getStatus()).json({ error: exception.message });
  }
}
```

**Memory Tip:** 🎣 Exception Filter = **fishing net** for errors. Catches specific exception types and handles them.

---

### Q26. How do you implement validation in NestJS?

**Answer:** Use `class-validator` decorators on DTOs + `ValidationPipe` globally or per-route. The pipe automatically validates incoming data against DTO rules.

```js
app.useGlobalPipes(new ValidationPipe({ whitelist: true, transform: true }));
```

**Memory Tip:** ✅ `class-validator` decorators on DTO + `ValidationPipe` globally = **automatic validation**. `whitelist: true` strips unknown properties.

---

### Q27. How do you implement authentication in NestJS?

**Answer:** Use `@nestjs/passport` with strategies (Local, JWT, OAuth). Create a guard extending `AuthGuard('jwt')`, apply it with `@UseGuards()`. Combine with `@nestjs/jwt` for token management.

```js
@UseGuards(AuthGuard('jwt'))
@Get('profile')
getProfile(@Request() req) { return req.user; }
```

**Memory Tip:** 🔐 Auth flow: **Passport strategy** validates credentials → **JWT** signs token → **Guard** protects routes.

---

### Q28. How does NestJS handle CORS?

**Answer:** Enable with `app.enableCors()` in `main.ts` or pass options: `app.enableCors({ origin: 'https://myapp.com', credentials: true })`. Same as Express CORS under the hood.

---

### Q29. What is ExecutionContext in NestJS?

**Answer:** `ExecutionContext` provides details about the current execution context — extends `ArgumentsHost`. It lets you access the request/response (HTTP), client/socket (WebSocket), or context (microservice). Used in guards, interceptors, and filters.

```js
const request = context.switchToHttp().getRequest();
```

**Memory Tip:** 🎯 ExecutionContext = **context chameleon**. Same code works for HTTP, WebSocket, or microservices by switching context.

---

### Q30. How do you connect to a database in NestJS?

**Answer:** NestJS doesn't handle DB directly — use ORMs via dedicated modules: `@nestjs/typeorm` (TypeORM), `@nestjs/mongoose` (MongoDB), `@nestjs/prisma` (Prisma), or `@nestjs/sequelize`. Register the module in your app module with connection config.

**Memory Tip:** 🗄️ NestJS DB = **pick your ORM**: TypeORM (SQL), Mongoose (MongoDB), Prisma (modern). Install the `@nestjs/` module.

---

### Q31. What is `@InjectRepository()` in NestJS?

**Answer:** Used with TypeORM to inject a repository for a specific entity into a service. It provides type-safe CRUD methods for that entity.

```js
constructor(@InjectRepository(User) private userRepo: Repository<User>) {}
```

---

### Q32. How do you implement caching in NestJS?

**Answer:** Use `@nestjs/cache-manager` with `CacheModule`. Apply `@UseInterceptors(CacheInterceptor)` on routes. Supports in-memory, Redis, and other stores. Configure TTL (time-to-live) globally or per route.

**Memory Tip:** 📦 `CacheModule` + `CacheInterceptor` = **automatic response caching**. Set TTL to avoid stale data.

---

### Q33. How do you generate API documentation with Swagger?

**Answer:** Use `@nestjs/swagger`. Configure `DocumentBuilder` in `main.ts`, create document with `SwaggerModule.createDocument()`, serve at a path. Decorate DTOs with `@ApiProperty()` and controllers with `@ApiOperation()`.

```js
const config = new DocumentBuilder().setTitle('My API').setVersion('1.0').build();
const document = SwaggerModule.createDocument(app, config);
SwaggerModule.setup('api-docs', app, document);
```

---

### Q34. What are custom decorators in NestJS?

**Answer:** Create custom parameter decorators with `createParamDecorator()` for reusable logic — like extracting user from request.

```js
export const User = createParamDecorator((data, ctx) => ctx.switchToHttp().getRequest().user);
// Usage: @User() user: UserEntity
```

---

### Q35. How do you handle file uploads in NestJS?

**Answer:** Use `@nestjs/platform-express` with Multer. `@UseInterceptors(FileInterceptor('field'))` for single file, `FilesInterceptor` for multiple. Access with `@UploadedFile()` decorator.

```js
@Post('upload')
@UseInterceptors(FileInterceptor('file'))
uploadFile(@UploadedFile() file: Express.Multer.File) { ... }
```

**Memory Tip:** 📎 File upload = **FileInterceptor + @UploadedFile()**. Same Multer under the hood as Express.

---

### Q36. How do you implement environment configuration?

**Answer:** Use `@nestjs/config` with `ConfigModule`. Call `ConfigModule.forRoot()` in app module. Access variables via `ConfigService.get('KEY')`. Supports `.env` files, validation schemas, and async loading.

```js
constructor(private configService: ConfigService) {}
const dbUrl = this.configService.get<string>('DATABASE_URL');
```

**Memory Tip:** ⚙️ `ConfigModule` + `ConfigService` = **centralized config**. Never access `process.env` directly — use ConfigService.

---

### Q37. How do you schedule tasks in NestJS?

**Answer:** Use `@nestjs/schedule` module. Decorators: `@Cron('0 * * * *')` (cron jobs), `@Interval(5000)` (fixed interval), `@Timeout(5000)` (one-time delay). Enable with `ScheduleModule.forRoot()`.

**Memory Tip:** ⏰ `@Cron` = **recurring on schedule**. `@Interval` = **every N ms**. `@Timeout` = **once after N ms**.

---

### Q38. How do you handle circular dependencies in NestJS?

**Answer:** Circular dependencies (A → B → A) can be resolved using `forwardRef()` on both sides: `@Inject(forwardRef(() => ServiceB))` in ServiceA and wrap the module import. Better to restructure code to avoid them.

**Memory Tip:** 🔄 Circular dependency = **two mirrors facing each other**. Fix with `forwardRef()` or better: **restructure your code**.

---

### Q39. What are the different types of modules in NestJS?

**Answer:** **Root module** (AppModule — app entry), **Feature modules** (organize specific features), **Shared modules** (export providers for reuse), **Global modules** (`@Global()` — available everywhere without importing), and **Dynamic modules** (configurable at runtime, like database modules).

**Memory Tip:** 🧩 Module types = **R-F-S-G-D** = **R**oot, **F**eature, **S**hared, **G**lobal, **D**ynamic.

---

### Q40. How do you implement versioning in NestJS APIs?

**Answer:** Enable with `app.enableVersioning()`. Types: URI (`/v1/users`), Header, Media Type, or Custom. Apply with `@Version('1')` decorator on controllers or routes.

```js
app.enableVersioning({ type: VersioningType.URI });
@Controller('users')
@Version('1')
export class UsersV1Controller { ... }
```

---

### Q41. What is the difference between Provider and Service?

**Answer:** A **provider** is any value injectable by NestJS DI (service, repository, factory, value). A **service** is a specific type of provider (class with `@Injectable()`) that contains business logic. All services are providers, but not all providers are services.

**Memory Tip:** 📐 Provider = **broad category**. Service = **specific provider with business logic**. Square vs rectangle relationship.

---

### Q42. How do you test NestJS applications?

**Answer:** Use `@nestjs/testing` with Jest. Create test modules with `Test.createTestingModule()`, override providers with mocks, and test controllers/services in isolation.

```js
const module = await Test.createTestingModule({
  controllers: [UsersController],
  providers: [{ provide: UsersService, useValue: mockService }],
}).compile();
```

**Memory Tip:** 🧪 `@nestjs/testing` + `Test.createTestingModule()` = **isolated unit tests**. Mock providers for pure testing.

---

### Q43. How do you implement logging in NestJS?

**Answer:** Use built-in `Logger` class (context-aware, log levels) or replace with custom implementations (Pino, Winston). Inject into services or use in controllers.

```js
constructor(private logger = new Logger(UsersService.name)) {}
this.logger.log('User created'); // includes context
```

**Memory Tip:** 📋 Built-in `Logger` > `console.log`. It adds **context** (class name) and supports **log levels**.

---

## 🔴 ADVANCED (Q44–Q50) — Enterprise Patterns & Architecture

---

### Q44. What are Dynamic Modules in NestJS?

**Answer:** Dynamic modules allow configuration at runtime — the module's providers/imports change based on parameters. Database modules (`TypeOrmModule.forRootAsync()`) are the classic example. Create by returning a `DynamicModule` from a static method.

```js
@Module({})
export class DatabaseModule {
  static forRoot(config: DbConfig): DynamicModule {
    return {
      module: DatabaseModule,
      providers: [{ provide: 'DB_CONFIG', useValue: config }],
    };
  }
}
```

**Memory Tip:** 🎛️ Dynamic module = **module factory**. `forRoot()` = global config. `forFeature()` = feature-specific. Same pattern as TypeORM/Mongoose.

---

### Q45. What is the Dependency Inversion Principle in NestJS?

**Answer:** DIP states high-level modules shouldn't depend on low-level modules — both should depend on abstractions. In NestJS: define interfaces for services, inject them via custom providers (`{ provide: 'UserService', useClass: UsersServiceImpl }`). This enables easy testing and swapping implementations.

**Memory Tip:** 🔄 DIP = depend on **interfaces**, not **implementations**. Custom providers let you swap implementations without changing consumers.

---

### Q46. How do you implement microservices with NestJS?

**Answer:** Use `@nestjs/microservices`. Create with `NestFactory.createMicroservice()` instead of regular app. Supports transports: TCP, Redis, NATS, RabbitMQ, Kafka, gRPC. Use `@MessagePattern()` and `@EventPattern()` decorators.

```js
const app = await NestFactory.createMicroservice(AppModule, { transport: Transport.RMQ });
@MessagePattern('user_created') handleUserCreated(data) { ... }
```

**Memory Tip:** 📡 `createMicroservice()` + `@MessagePattern()` = **message-based communication**. Not HTTP — uses message brokers.

---

### Q47. How do you handle database transactions in NestJS?

**Answer:** With TypeORM: inject `DataSource` or use `@Transaction()` decorator (deprecated in newer versions). Modern approach: use `QueryRunner` or wrap in `entityManager.transaction()`. For Prisma: use `prisma.$transaction()`.

**Memory Tip:** 📦 Transactions = **"all or nothing"** for DB operations. Either all succeed or all roll back.

---

### Q48. What is the role of `useGlobalGuards`, `useGlobalInterceptors`, and `useGlobalPipes`?

**Answer:** These register guards, interceptors, and pipes globally — applied to every route without per-route decorators. Set in `main.ts`: `app.useGlobalGuards(new AuthGuard())`. They're singleton-scoped and can't inject dependencies unless registered as providers.

**Memory Tip:** 🌐 `useGlobal*` = **applies everywhere**. Set once in `main.ts`. For DI-enabled globals, register as providers in any module.

---

### Q49. How do you implement soft deletes in NestJS?

**Answer:** Use TypeORM's `@DeleteDateColumn()` which adds a `deletedAt` timestamp. Queries automatically exclude soft-deleted records. Restore with `repository.restore(id)`. Raw SQL: add `isDeleted` flag and filter queries.

**Memory Tip:** 🗑️ Soft delete = **trash bin** not **shredder**. `deletedAt` = null means active. Non-null means deleted.

---

### Q50. How would you architect a large-scale NestJS application?

**Answer:** Use modular architecture: each feature as a self-contained module with its own controller, service, DTOs, and entities. Apply DDD (Domain-Driven Design) with bounded contexts. Use global guards for auth, interceptors for logging/transform, and pipes for validation. Shared modules for common functionality. Microservices for scaling specific domains.

```
src/
├── modules/
│   ├── auth/ (module, controller, service, guards, strategies)
│   ├── users/ (module, controller, service, dto, entity)
│   └── orders/ (module, controller, service, dto, entity)
├── common/ (filters, interceptors, decorators, pipes)
├── config/ (database, jwt, app config)
└── main.ts
```

**Memory Tip:** 🏗️ Large-scale NestJS = **one module per feature** + shared common layer + config layer. "Module = bounded context."

---

*Sources: [NestJS Docs](https://docs.nestjs.com), [Digiqt](https://digiqt.com/blog/nestjs-interview-questions/), [gasangw/NestJS-Interview-Questions](https://github.com/gasangw/NestJS-Interview-Questions-And-Answers), [SharpSkill](https://sharpskill.dev), [Mindbowser](https://www.mindbowser.com/understanding-nestjs-architecture/)*
