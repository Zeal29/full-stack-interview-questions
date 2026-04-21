# TypeScript Interview Questions — Top 50

> **Difficulty Split:** Basic (Q1–Q20) | Intermediate (Q21–Q40) | Advanced (Q41–Q50)

---

## Basic Questions (Q1–Q20)

### Q1. What is TypeScript and why use it?

**Answer:** TypeScript is a statically typed superset of JavaScript that compiles to plain JS. It adds optional type annotations, compile-time error checking, better IDE support (autocompletion, refactoring), and improved code maintainability. Catches bugs before runtime.

**💡 Tip:** TypeScript = **JavaScript + types**. Think of it as a spell-checker for your code — catches mistakes before you run it.

---

### Q2. What is the difference between TypeScript and JavaScript?

**Answer:** JavaScript is dynamically typed (types checked at runtime). TypeScript adds static typing (checked at compile time), interfaces, enums, generics, access modifiers, and advanced type features. TS code compiles to JS — browsers don't run TypeScript directly.

**💡 Tip:** JS = types at **runtime** ( surprises in production). TS = types at **compile time** (caught during development).

---

### Q3. What are the basic types in TypeScript?

**Answer:** `string`, `number`, `boolean`, `null`, `undefined`, `bigint`, `symbol`, `object`, `any`, `unknown`, `void`, `never`, `Array<T>`, `tuple`, `enum`.

```typescript
let name: string = "Ali";
let age: number = 25;
let isActive: boolean = true;
let items: string[] = ["a", "b"];
```

**💡 Tip:** Same as JS primitives + `any`, `unknown`, `void`, `never`, `enum`, `tuple` — TypeScript-specific additions.

---

### Q4. What is the `any` type and why avoid it?

**Answer:** `any` disables type checking entirely — you can assign anything to it and access any property. It defeats the purpose of TypeScript. Avoid it. Use `unknown` when you don't know the type — it requires type narrowing before use.

**💡 Tip:** `any` = **turn off TypeScript**. `unknown` = **"I don't know yet, but I'll check before using"**. Always prefer `unknown`.

---

### Q5. What is the difference between `interface` and `type`?

**Answer:**
- **Interface**: extends with `extends`, can be merged (declaration merging), better for object shapes
- **Type**: supports unions, intersections, mapped types, conditional types, more flexible

Use interfaces for object shapes and public APIs. Use types for unions, utility types, and complex compositions.

**💡 Tip:** Interface = **"I describe an object"** (open/extendable). Type = **"I can be anything"** (unions, computed). Both work for most cases — be consistent.

---

### Q6. What are generics in TypeScript?

**Answer:** Generics allow you to write reusable, type-safe functions/classes that work with multiple types:
```typescript
function identity<T>(value: T): T { return value; }
identity<string>("hello"); // T = string
identity<number>(42);      // T = number
```

Common: `Array<T>`, `Promise<T>`, `Record<K, V>`, `Map<K, V>`.

**💡 Tip:** Generics = **type variables** — `<T>` is a placeholder for a type that the user specifies when calling. Like function arguments but for types.

---

### Q7. What is type assertion?

**Answer:** Type assertion tells TypeScript "trust me, I know the type":
```typescript
let value: unknown = "hello";
let len: number = (value as string).length; // angle bracket: <string>value
```

Does NOT change runtime type — only tells compiler. Use sparingly.

**💡 Tip:** `as` = **"I know better than you"** to TypeScript. It's a **compiler hint**, not a runtime conversion. Avoid unless necessary.

---

### Q8. What is the `union` type?

**Answer:** Union allows a value to be one of several types:
```typescript
let id: string | number;
id = "abc"; // OK
id = 123;   // OK
```

Use with type narrowing (`typeof`, `in`, `instanceof`) to safely access type-specific properties.

**💡 Tip:** Union = **OR** (`|`). "This can be string **OR** number." Use type guards to narrow down which one it actually is.

---

### Q9. What is the `intersection` type?

**Answer:** Intersection combines multiple types into one:
```typescript
type Employee = { name: string } & { age: number };
// Must have both name AND age
```

Common with interfaces: `type Admin = User & { permissions: string[] }`.

**💡 Tip:** Intersection = **AND** (`&`). "Must have ALL properties from ALL types." Union = OR, Intersection = AND.

---

### Q10. What are enums in TypeScript?

**Answer:** Enums define a set of named constants:
```typescript
enum Direction { Up, Down, Left, Right }         // numeric (0,1,2,3)
enum Status { Active = "ACTIVE", Inactive = "INACTIVE" } // string
const enum Color { Red, Green, Blue }  // inlined at compile time
```

**💡 Tip:** Prefer **const enums** (no JS output) or **union types** (`"up" | "down"`) over regular enums for tree-shaking.

---

### Q11. What is `tsconfig.json`?

**Answer:** Configuration file for TypeScript compiler. Key options:
- `strict: true` — enables all strict checks
- `target` — JS version to compile to (ES2020, ESNext)
- `module` — module system (commonjs, ESNext)
- `outDir` — output directory
- `include/exclude` — files to compile

**💡 Tip:** Always enable `strict: true` — it catches the most bugs. It's the **seatbelt** of TypeScript.

---

### Q12. What is the `void` type?

**Answer:** `void` means a function returns nothing:
```typescript
function log(msg: string): void { console.log(msg); }
```

Used as the return type of functions that don't return a value. Variables of type `void` can only be `undefined` or `null` (if `strictNullChecks` is off).

**💡 Tip:** `void` = **"returns nothing"** (functions). Different from `undefined` = **"value is undefined"**.

---

### Q13. What is the `never` type?

**Answer:** `never` represents values that **never occur**:
- Functions that never return (infinite loop, always throw)
- Unreachable code in type narrowing (exhaustive checking)

```typescript
function fail(msg: string): never { throw new Error(msg); }
```

**💡 Tip:** `never` = **"impossible"**. Used for exhaustive switch checks: default case assigns to `never` to catch unhandled cases.

---

### Q14. What is the `unknown` type?

**Answer:** `unknown` is the type-safe counterpart of `any`. You can assign anything to `unknown`, but you **must narrow** the type before using it:
```typescript
let val: unknown = "hello";
val.toUpperCase(); // Error! Must narrow first
if (typeof val === "string") val.toUpperCase(); // OK
```

**💡 Tip:** `unknown` = **"I don't know, prove it"**. Forces you to check types before using. Always prefer `unknown` over `any`.

---

### Q15. What are optional and default parameters?

**Answer:**
```typescript
function greet(name: string, greeting?: string): string {
  return `${greeting || "Hello"}, ${name}`;
}
function greet2(name: string, greeting: string = "Hello"): string {
  return `${greeting}, ${name}`;
}
```

`?` makes parameter optional (can be `undefined`). `= value` provides a default.

**💡 Tip:** `?` = optional (might be `undefined`). `= default` = has fallback. Optional params must come **after** required ones.

---

### Q16. What are readonly properties?

**Answer:** `readonly` prevents property reassignment after initialization:
```typescript
interface Config { readonly apiUrl: string; }
const config: Readonly<Config> = { apiUrl: "/api" };
config.apiUrl = "/v2"; // Error!
```

Only prevents direct reassignment — nested objects can still be mutated. Use `Readonly<T>` for all properties.

**💡 Tip:** `readonly` = **write-once**. Like `const` but for object properties. For deep immutability, use utility types recursively.

---

### Q17. What is the `keyof` operator?

**Answer:** `keyof` creates a union type of an object's keys:
```typescript
type User = { name: string; age: number; email: string };
type UserKeys = keyof User; // "name" | "age" | "email"
function getProp(obj: User, key: keyof User) { return obj[key]; }
```

**💡 Tip:** `keyof` = **"give me all the keys as a union"**. Essential for generic utility functions.

---

### Q18. What is type narrowing?

**Answer:** TypeScript narrows types within conditional blocks:
- `typeof x === "string"` — type guard
- `x instanceof Error` — instance check
- `"prop" in obj` — property check
- `x !== null` — null check
- Discriminated unions: `shape.kind === "circle"`

**💡 Tip:** Type narrowing = **"prove what type it is, then I'll let you use it."** TypeScript infers narrower types in `if` blocks.

---

### Q19. What are literal types?

**Answer:** Literal types restrict a value to exact strings, numbers, or booleans:
```typescript
type Direction = "left" | "right" | "up" | "down";
type StatusCode = 200 | 404 | 500;
let dir: Direction = "left"; // OK
dir = "diagonal"; // Error!
```

**💡 Tip:** Literal types = **"exactly this value"**. Combine with unions to create precise, self-documenting types.

---

### Q20. What is the `Record<K, V>` utility type?

**Answer:** `Record<K, V>` creates an object type with keys `K` and values `V`:
```typescript
type UserRole = "admin" | "user" | "guest";
const roles: Record<UserRole, string[]> = {
  admin: ["read", "write", "delete"],
  user: ["read", "write"],
  guest: ["read"]
};
```

**💡 Tip:** `Record` = **typed dictionary/map**. `Record<string, number>` = any string key maps to number value.

---

## Intermediate Questions (Q21–Q40)

### Q21. What are mapped types?

**Answer:** Mapped types create new types by transforming each property:
```typescript
type Readonly<T> = { readonly [K in keyof T]: T[K] };
type Optional<T> = { [K in keyof T]?: T[K] };
type Stringify<T> = { [K in keyof T]: string };
```

The foundation of TypeScript's built-in utility types.

**💡 Tip:** Mapped type = **"loop over keys and transform"**. `[K in keyof T]` iterates over keys, `T[K]` gets the value type.

---

### Q22. What are conditional types?

**Answer:** Conditional types choose a type based on a condition:
```typescript
type IsString<T> = T extends string ? "yes" : "no";
type A = IsString<string>; // "yes"
type B = IsString<number>; // "no"
```

Advanced: `infer` keyword extracts types: `type ReturnType<T> = T extends (...args: any[]) => infer R ? R : never;`

**💡 Tip:** Conditional types = **ternary for types**. `T extends U ? X : Y`. `infer` = **"extract and capture a type"**.

---

### Q23. What are utility types in TypeScript?

**Answer:** Built-in helpers that transform types:
- `Partial<T>` — all properties optional
- `Required<T>` — all properties required
- `Readonly<T>` — all properties readonly
- `Pick<T, K>` — select specific properties
- `Omit<T, K>` — remove specific properties
- `Record<K, V>` — key-value mapping
- `Exclude<U, E>` — exclude from union
- `Extract<U, E>` — extract from union
- `ReturnType<T>` — function return type
- `Parameters<T>` — function params as tuple

**💡 Tip:** `Partial` = edit mode. `Pick` = select columns. `Omit` = remove columns. `Record` = typed dict. **Memorize these — they're used everywhere.**

---

### Q24. What is declaration merging?

**Answer:** Interfaces with the same name are automatically merged:
```typescript
interface User { name: string; }
interface User { age: number; }
// Result: User has both name and age
```

Useful for extending third-party types. Types (`type`) do NOT support declaration merging.

**💡 Tip:** Only **interfaces** merge, not types. Used to extend library types without modifying the original file.

---

### Q25. What is `typeof` in TypeScript (vs JavaScript)?

**Answer:** In TS, `typeof` can be used in **type position** to extract a type from a value:
```typescript
const config = { apiUrl: "/api", timeout: 5000 };
type Config = typeof config; // { apiUrl: string; timeout: number; }
```

In JS, `typeof` returns a string at runtime. In TS type annotations, it creates a type from a value.

**💡 Tip:** `typeof` in **type position** = **"copy the type of this value"**. Great for creating types from constants.

---

### Q26. What is `satisfies` operator?

**Answer:** `satisfies` validates a value matches a type **without widening**:
```typescript
const palette = {
  red: [255, 0, 0],
  green: "#00ff00",
} satisfies Record<string, string | number[]>;
// palette.green is still typed as string (not string | number[])
```

Unlike type annotation (`: Type`), it preserves literal types while checking conformance.

**💡 Tip:** `satisfies` = **"check the type but keep the specifics"**. Type annotation widens. `satisfies` narrows and validates.

---

### Q27. What are discriminated unions?

**Answer:** A pattern using a common `kind` (or `type`) literal field to distinguish union members:
```typescript
type Shape = 
  | { kind: "circle"; radius: number }
  | { kind: "rect"; width: number; height: number };

function area(s: Shape) {
  switch (s.kind) {
    case "circle": return Math.PI * s.radius ** 2;
    case "rect": return s.width * s.height;
  }
}
```

**💡 Tip:** Discriminated union = **tagged union**. The discriminant (`kind`) enables type-safe narrowing in switch/if. Always use for state machines.

---

### Q28. What are type guards?

**Answer:** Type guards narrow types at runtime:
- `typeof x === "string"` — primitive check
- `x instanceof Array` — class check
- `"prop" in x` — property check
- Custom: `function isString(x: any): x is string { return typeof x === "string"; }`

The `x is Type` return type is a **type predicate** that tells TS the narrowed type.

**💡 Tip:** Custom type guards with `x is Type` = **"I promise this function checks the type correctly."** TS trusts your predicate.

---

### Q29. What are index signatures?

**Answer:** Index signatures define types for dynamic keys:
```typescript
interface StringMap { [key: string]: string; }
const dict: StringMap = { hello: "world", foo: "bar" };

interface Config { [key: string]: string | number; env: string; }
```

All named properties must match the index signature type.

**💡 Tip:** Index signatures = **"any key of this type maps to this value type."** Use `Record<K, V>` when keys are known. Use index signatures for dynamic keys.

---

### Q30. What is `infer` in conditional types?

**Answer:** `infer` extracts and captures a type within a conditional:
```typescript
type ArrayElement<T> = T extends (infer E)[] ? E : never;
type Str = ArrayElement<string[]>; // string

type PromiseValue<T> = T extends Promise<infer V> ? V : T;
type Num = PromiseValue<Promise<number>>; // number
```

**💡 Tip:** `infer` = **"extract this type for me"**. Like a regex capture group but for types. Only works inside `extends` conditionals.

---

### Q31. What is the `as const` assertion?

**Answer:** `as const` makes TypeScript infer the **narrowest literal types**:
```typescript
const config = { host: "localhost", port: 3000 } as const;
// Type: { readonly host: "localhost"; readonly port: 3000 }
const roles = ["admin", "user"] as const;
// Type: readonly ["admin", "user"]
```

**💡 Tip:** `as const` = **"lock it down"** — makes everything readonly with literal types. Essential for tuples and discriminated unions.

---

### Q32. What are template literal types?

**Answer:** String manipulation at the type level:
```typescript
type EventName = `on${Capitalize<string>}`; // "onClick", "onHover", etc.
type CSSProperty = `margin-${"top" | "bottom" | "left" | "right"}`;
// "margin-top" | "margin-bottom" | "margin-left" | "margin-right"
```

Built-in: `Uppercase<S>`, `Lowercase<S>`, `Capitalize<S>`, `Uncapitalize<S>`.

**💡 Tip:** Template literal types = **template literals for types**. Backticks in type position generate string unions.

---

### Q33. What is the difference between `abstract` class and `interface`?

**Answer:**
- **Abstract class**: can have implementation, constructor, access modifiers, runs at runtime
- **Interface**: only type information, no runtime code, supports merging, can extend multiple

Use abstract classes when you share code. Use interfaces for type contracts.

**💡 Tip:** Abstract class = **code + contract**. Interface = **contract only**. Prefer interfaces for flexibility, abstract classes for shared logic.

---

### Q34. What is `Omit<T, K>` vs `Pick<T, K>`?

**Answer:**
```typescript
type User = { id: number; name: string; email: string; password: string };
type PublicUser = Omit<User, "password">;       // removes password
type LoginInfo = Pick<User, "email" | "password">; // keeps only email + password
```

`Omit` = exclude properties. `Pick` = select properties. Both create new types.

**💡 Tip:** `Omit` = **"everything except"**. `Pick` = **"only these"**. Use `Omit` to hide sensitive fields, `Pick` to select needed fields.

---

### Q35. What are accessor decorators?

**Answer:** TypeScript supports decorators (experimental) for classes:
- **Class decorator**: `@log class MyClass {}`
- **Method decorator**: `@cache myMethod() {}`
- **Property decorator**: `@required name: string;`
- **Parameter decorator**: `@inject() service: Service`

Enable with `experimentalDecorators: true` in tsconfig.

**💡 Tip:** Decorators = **"wrap this with extra behavior"**. Popular in NestJS, Angular, TypeORM. Still experimental in TS.

---

### Q36. What is function overloading in TypeScript?

**Answer:** Multiple function signatures with one implementation:
```typescript
function process(input: string): number;
function process(input: number): string;
function process(input: string | number): string | number {
  return typeof input === "string" ? input.length : input.toString();
}
```

**💡 Tip:** Overloads = **different signatures, one implementation**. The implementation signature must be compatible with all overloads.

---

### Q37. What is the `ReturnType<T>` utility type?

**Answer:** Extracts the return type of a function type:
```typescript
function getUser() { return { name: "Ali", age: 25 }; }
type User = ReturnType<typeof getUser>; // { name: string; age: number; }
```

**💡 Tip:** `ReturnType<typeof fn>` = **"what does this function return?"** Works with `typeof` to extract from actual functions.

---

### Q38. What are namespaces in TypeScript?

**Answer:** Namespaces organize code under a named scope (pre-modules pattern):
```typescript
namespace Utils {
  export function log(msg: string) { console.log(msg); }
}
Utils.log("hello");
```

Largely replaced by ES6 modules. Still used in `.d.ts` declarations and legacy code.

**💡 Tip:** Namespaces = **legacy organization**. Use ES6 `import/export` instead. Only use namespaces for declaration files.

---

### Q39. What are `.d.ts` declaration files?

**Answer:** Declaration files describe the types of JavaScript libraries without implementation:
```typescript
// types.d.ts
declare module "my-lib" {
  export function doSomething(input: string): number;
}
```

They tell TypeScript about JS code. `@types/package` provides declarations for npm packages.

**💡 Tip:** `.d.ts` = **type description without code**. Like a menu — tells you what's available without showing the recipe.

---

### Q40. What is `strictNullChecks`?

**Answer:** When enabled, `null` and `undefined` are NOT assignable to other types. You must explicitly handle them:
```typescript
let name: string = null; // Error with strictNullChecks
let name: string | null = null; // OK
```

One of the most important `strict` options — prevents null/undefined runtime errors.

**💡 Tip:** `strictNullChecks` = **"null is not a valid string/number/etc."** Forces you to handle null cases explicitly. Always enable it.

---

## Advanced Questions (Q41–Q50)

### Q41. What are recursive types?

**Answer:** Types that reference themselves — used for nested structures:
```typescript
type JSONValue = string | number | boolean | null | JSONValue[] | { [key: string]: JSONValue };
type TreeNode = { value: number; left?: TreeNode; right?: TreeNode };
```

**💡 Tip:** Recursive types = **self-referencing types**. Used for trees, nested objects, linked lists. TypeScript has depth limits.

---

### Q42. What are variadic tuple types?

**Answer:** Spread generic tuples to build new tuple types:
```typescript
type Push<T extends any[], V> = [...T, V];
type Result = Push<[1, 2, 3], 4>; // [1, 2, 3, 4]

type Concat<A extends any[], B extends any[]> = [...A, ...B];
type R = Concat<[1, 2], [3, 4]>; // [1, 2, 3, 4]
```

**💡 Tip:** Variadic tuples = **generic spread in tuples**. `[...T, V]` appends, `[V, ...T]` prepends. TypeScript 4.0+.

---

### Q43. What is the `extends` keyword in generics?

**Answer:** `extends` in generics creates **constraints**:
```typescript
function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key];
}
```

`K extends keyof T` means "K must be a key of T." Prevents invalid key access.

**💡 Tip:** `extends` in generics = **"must be at least this type"**. `T extends string` = T can be string or subtype of string.

---

### Q44. What is the `never` type for exhaustive checking?

**Answer:** Use `never` to ensure all cases are handled in a switch:
```typescript
type Shape = "circle" | "square";
function getArea(shape: Shape) {
  switch (shape) {
    case "circle": return Math.PI;
    case "square": return 4;
    default:
      const _exhaustive: never = shape; // Error if a case is missing!
      return _exhaustive;
  }
}
```

**💡 Tip:** Assign to `never` in default = **compiler-enforced exhaustive check**. If you add a new union member, TS errors until you handle it.

---

### Q45. What are branded/opaque types?

**Answer:** Creating nominally typed values (distinct even with same structure):
```typescript
type USD = number & { __brand: "USD" };
type EUR = number & { __brand: "EUR" };
function processUSD(amount: USD) {}
processUSD(100 as EUR); // Error! Can't mix EUR and USD
```

**💡 Tip:** Branded types = **same structure, different identity**. Prevent mixing semantically different values (USD vs EUR, UserId vs OrderId).

---

### Q46. What is `emitDecoratorMetadata`?

**Answer:** Compiler option that emits type metadata for decorators — used by NestJS and TypeORM for dependency injection:
```typescript
@Inject()
class MyService {
  constructor(private repo: Repository<User>) {} // type metadata emitted
}
```

Requires `reflect-metadata` package. Enables framework magic like auto-injection.

**💡 Tip:** `emitDecoratorMetadata` = **runtime type info for decorators**. NestJS needs it. Always pair with `reflect-metadata`.

---

### Q47. What is type-level programming in TypeScript?

**Answer:** Using TypeScript's type system as a mini programming language:
```typescript
type Length<T extends any[]> = T["length"];
type Reverse<T extends any[]> = T extends [infer F, ...infer R] ? [...Reverse<R>, F] : [];
type Fibonacci<N extends number> = // complex recursive type
```

Advanced: parsing, arithmetic, string manipulation — all at the type level.

**💡 Tip:** Type-level programming = **"code that runs in the compiler"**. Powerful but complex. Use for type-safe APIs, not for cleverness.

---

### Q48. What are `private`, `protected`, `public` in TypeScript?

**Answer:**
- `public` (default) — accessible everywhere
- `private` — only within the class
- `protected` — within class and subclasses
- `#` (ECMAScript private) — true runtime privacy (not just compile-time)

TS `private` is compile-time only. `#` is enforced at runtime.

**💡 Tip:** TS `private` = **compile-time illusion**. `#private` = **real privacy**. Prefer `#` for true encapsulation, `private` for team conventions.

---

### Q49. What is the difference between structural and nominal typing?

**Answer:**
- **Structural (TypeScript)**: types are compatible if they have the same shape/structure, regardless of name
- **Nominal (Java/C#)**: types are only compatible if they share the same explicit name/identity

```typescript
class A { x: number = 0; }
class B { x: number = 0; }
const a: A = new B(); // OK in TS (structural), Error in nominal systems
```

**💡 Tip:** TypeScript = **"if it looks like a duck, it is a duck"** (structural). Use branded types when you need nominal behavior.

---

### Q50. What are module augmentation and global augmentation?

**Answer:** Extend existing module types without modifying the source:
```typescript
// Module augmentation
declare module "express" {
  interface Request {
    userId?: string;
  }
}
// Global augmentation
declare global {
  interface Window { myApp: any; }
}
```

**💡 Tip:** Module augmentation = **"add types to someone else's module"**. Used when extending Express, Jest, etc. Must be in a module file (has import/export).

---
