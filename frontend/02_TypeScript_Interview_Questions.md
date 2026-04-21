# TypeScript Interview Questions — Top 50

> **Difficulty Split**: ~40% Basic (Q1–20), ~45% Intermediate (Q21–42), ~15% Advanced (Q43–50)
> **Focus**: Type system, generics, utility types — tailored for backend/full-stack engineers

---

## SECTION A: BASIC / FUNDAMENTAL (Q1–20)

---

### Q1. What is TypeScript and why would you use it over JavaScript?

**Answer**: TypeScript is a statically typed superset of JavaScript developed by Microsoft. It adds compile-time type checking, catching bugs before runtime. It compiles down to plain JavaScript, so it runs anywhere JS does. You'd use it for larger codebases where type safety, IDE autocompletion, and refactorability matter.

**Memory Tip**: "TS = JS + **S**tatic types + **C**ompile-time checks". Think: TypeScript catches bugs at compile time, JavaScript catches them in production.

---

### Q2. What are the primitive types in TypeScript?

**Answer**: TypeScript has seven primitive types: `string`, `number`, `boolean`, `bigint`, `symbol`, `undefined`, and `null`. Unlike JavaScript, TypeScript lets you explicitly annotate these, and the compiler enforces them at build time.

**Memory Tip**: "**S**tring, **N**umber, **B**oolean, **B**igInt, **S**ymbol, **U**ndefined, **N**ull" — "SNBBSUN" (think: "Sun" with extra letters).

---

### Q3. What is type inference? When does TypeScript infer types?

**Answer**: Type inference means TypeScript automatically determines the type of a variable when you don't explicitly annotate it. For example, `let x = 5` infers `x` as `number`. TypeScript infers types during variable initialization, function return values, and property assignments.

**Memory Tip**: "If you don't tell TS the type, it **infers** it from context — like a detective reading clues."

---

### Q4. What is the difference between `interface` and `type`?

**Answer**: Both define object shapes, but `interface` supports declaration merging (multiple declarations with the same name combine), while `type` supports unions, intersections, and mapped types more naturally. Use `interface` for public APIs and class contracts; use `type` for complex compositions and unions.

**Memory Tip**: "**I**nterface = **I**dentity (merges with same name). **T**ype = **T**ransform (unions, intersections, mapped)."

---

### Q5. What are union and intersection types?

**Answer**: A **union type** (`A | B`) allows a value to be one of several types — like an "OR". An **intersection type** (`A & B`) combines multiple types into one — like an "AND". Unions are common for flexible parameters; intersections are common for mixing capabilities.

**Memory Tip**: "Union = `|` = **OR** (one of these). Intersection = `&` = **AND** (all of these)."

---

### Q6. What is the `any` type and why should you avoid it?

**Answer**: `any` disables all type checking — you can assign anything to it and call any method on it. It defeats TypeScript's entire purpose. It's a escape hatch for migrating JS codebases or dealing with truly dynamic data, but should be replaced with `unknown` or proper types as soon as possible.

**Memory Tip**: "`any` = **A**void **N**othing **Y**ields safety. Use `unknown` instead — it forces you to narrow the type before using it."

---

### Q7. What is the `unknown` type?

**Answer**: `unknown` is the type-safe counterpart to `any`. You can assign any value to `unknown`, but you **must narrow** the type (via type guards like `typeof`, `instanceof`) before performing operations on it. It's ideal for API responses or user input where the type is genuinely uncertain.

**Memory Tip**: "`unknown` = "I don't know yet, but I'll check before I use it." `any` = "I don't care.""

---

### Q8. What is the `void` type?

**Answer**: `void` represents the absence of a return value. It's commonly used as the return type of functions that don't return anything (e.g., `function log(msg: string): void`). A `void` variable can only be assigned `undefined` or `null` (with `strictNullChecks` off).

**Memory Tip**: "`void` = **V**acant **O**utput **I**n **D**eclaration — the function returns nothing."

---

### Q9. What is the `never` type?

**Answer**: `never` represents values that **never occur**. It's the return type of functions that never return (infinite loops, functions that always throw). It's also used in exhaustive checks in switch statements — if all cases are handled, the `default` branch has type `never`.

**Memory Tip**: "`never` = **N**o **E**xisting **V**alue **E**ver **R**eturns. Used for impossible states."

---

### Q10. What is a type assertion?

**Answer**: A type assertion tells the compiler "trust me, I know this type" — it's a way to override the inferred type. Syntax: `value as Type` or `<Type>value`. It doesn't perform runtime checks; it's purely a compile-time hint. Overuse is a code smell.

**Memory Tip**: "Type assertion = casting in other languages. Think: 'as if' — `data as UserResponse`."

---

### Q11. What are enums in TypeScript?

**Answer**: Enums define a set of named constants. Numeric enums auto-increment from 0 (or a custom start), while string enums require explicit values. TypeScript also supports `const enum` which is inlined at compile time (no runtime object).

**Memory Tip**: "Enums = **E**xplicit **N**amed **U**nits of **M**eaning. Use string enums for clarity: `Status.Active` beats `1`."

---

### Q12. What is the `readonly` modifier?

**Answer**: `readonly` marks a property as immutable after initialization. For arrays, `readonly number[]` prevents push/pop. It's a compile-time check only — JavaScript has no runtime immutability. Use `Readonly<T>` utility type to make all properties readonly at once.

**Memory Tip**: "`readonly` = one-time assignment, like a const declaration for object properties."

---

### Q13. What is an optional property?

**Answer**: Adding `?` after a property name makes it optional: `{ name: string; age?: number }`. The property can be omitted entirely. TypeScript treats it as `T | undefined` when accessed. Useful for partial updates and flexible configurations.

**Memory Tip**: "The `?` is literally a question: 'Is this property here? Maybe, maybe not.'"

---

### Q14. What is `null` vs `undefined` in TypeScript?

**Answer**: Both represent "absence of value," but `undefined` means a variable was declared but never assigned, while `null` is an explicitly assigned "no value." With `strictNullChecks`, TypeScript forces you to handle both. `undefined` is the default for uninitialized variables.

**Memory Tip**: "`undefined` = 'never set'. `null` = 'intentionally empty'. Like an empty inbox vs. a cleared inbox."

---

### Q15. What are literal types?

**Answer**: Literal types allow you to specify exact values as types: `let direction: "north" | "south" | "east" | "west"`. Combined with unions, they create narrow, precise types. Numeric literals work too: `type DiceRoll = 1 | 2 | 3 | 4 | 5 | 6`.

**Memory Tip**: "Literal types = the type IS the value. `'hello'` is both a value AND a type."

---

### Q16. What is the `keyof` operator?

**Answer**: `keyof` creates a union type from the keys of an object type. `keyof User` gives `"name" | "age" | "email"`. It's essential for type-safe property access, generic constraints, and mapped types.

**Memory Tip**: "`keyof` = 'give me the keys!' Like `Object.keys()` but at the type level."

---

### Q17. What is structural typing in TypeScript?

**Answer**: TypeScript uses **structural typing** (duck typing) — types are compatible if they have the same shape, regardless of their names. A `{ name: string }` is compatible with any other type that has a `name: string` property, even if they have different type names.

**Memory Tip**: "If it walks like a duck and quacks like a duck, TypeScript says it's a duck — structure, not name."

---

### Q18. What are type guards?

**Answer**: Type guards are runtime checks that narrow a type within a code block. Common forms: `typeof x === "string"` (primitives), `instanceof Error` (classes), `"prop" in obj` (property check), and custom type predicates (`arg is Type`).

**Memory Tip**: "Type guards = bouncers at a club. They check the 'type ID' before letting code through."

---

### Q19. What is type narrowing?

**Answer**: Type narrowing is the process where TypeScript refines a variable's type from a broader type to a more specific one based on control flow analysis. After an `if (typeof x === "string")` check, TypeScript knows `x` is a `string` inside the block.

**Memory Tip**: "Narrowing = zooming in. `string | number` → `string` after the check."

---

### Q20. What are tuples in TypeScript?

**Answer**: Tuples are fixed-length, fixed-type arrays: `[string, number]` means exactly two elements — first a string, then a number. They're useful for key-value pairs, function return types (`[data, error]`), and CSV-like rows.

**Memory Tip**: "Tuples = arrays with a strict guest list. Position 0 must be type A, position 1 must be type B."

---

## SECTION B: INTERMEDIATE (Q21–42)

---

### Q21. What are generics? Give a practical example.

**Answer**: Generics let you write reusable functions/types that work with multiple types while preserving type safety. Example: `function identity<T>(arg: T): T { return arg; }`. The caller decides `T` — `identity<string>("hello")`. Generics preserve the relationship between input and output types.

**Memory Tip**: "Generics = **G**eneric **E**xpected **N**ame **E**nabling **R**euse **I**n **C**ode. Think of `<T>` as a placeholder that gets filled in later."

---

### Q22. What are generic constraints (`extends`)?

**Answer**: You can constrain a generic parameter with `extends`: `<T extends { length: number }>` means `T` must have a `length` property. This lets you use `.length` inside the function while still being generic. Without constraints, `T` is treated as `unknown`.

**Memory Tip**: "`extends` in generics = 'T must at least have these capabilities.' Like a job requirement."

---

### Q23. What is the `Partial<T>` utility type?

**Answer**: `Partial<T>` makes all properties of `T` optional. Useful for update functions where you only pass changed fields: `function updateUser(id: string, data: Partial<User>)`. It's the type equivalent of making every property nullable.

**Memory Tip**: "`Partial` = partially filled form. Not all fields required."

---

### Q24. What is the `Required<T>` utility type?

**Answer**: `Required<T>` is the inverse of `Partial` — it makes all properties required (removes all `?` modifiers). Useful when you've been working with a partial type and need to enforce that all fields are present before saving.

**Memory Tip**: "`Required` = 'no more excuses, fill in EVERYTHING.'"

---

### Q25. What is the `Readonly<T>` utility type?

**Answer**: `Readonly<T>` makes all properties of `T` read-only. Equivalent to adding `readonly` to every property. Commonly used for function parameters that should not be mutated, or for frozen configuration objects.

**Memory Tip**: "`Readonly` = 'look but don't touch.' Like museum exhibits."

---

### Q26. What is the `Pick<T, K>` utility type?

**Answer**: `Pick<T, K>` creates a new type by selecting a subset of properties `K` from type `T`. Example: `Pick<User, "id" | "name">` creates `{ id: number; name: string }`. Use it when you need a "view" of an object with fewer fields.

**Memory Tip**: "`Pick` = cherry-pick. 'I only want these specific fields.'"

---

### Q27. What is the `Omit<T, K>` utility type?

**Answer**: `Omit<T, K>` is the inverse of `Pick` — it creates a new type by removing properties `K` from `T`. Example: `Omit<User, "password">` removes the `password` field. Use for public-facing DTOs that exclude sensitive fields.

**Memory Tip**: "`Omit` = 'leave this out.' `Omit<User, 'password'>` = User without the secret."

---

### Q28. What is the `Record<K, V>` utility type?

**Answer**: `Record<K, V>` creates an object type with keys of type `K` and values of type `V`. Example: `Record<string, number>` is `{ [key: string]: number }`. Ideal for dictionaries, lookup tables, and maps.

**Memory Tip**: "`Record` = a database record. Key-value pairs where all keys are type K and all values are type V."

---

### Q29. What is the `ReturnType<T>` utility type?

**Answer**: `ReturnType<T>` extracts the return type of a function type. `ReturnType<typeof fetchUser>` gives you the return type of `fetchUser` without calling it. Useful for deriving types from functions instead of duplicating type definitions.

**Memory Tip**: "`ReturnType` = 'what does this function give back?' No need to call it to find out."

---

### Q30. What is the `Parameters<T>` utility type?

**Answer**: `Parameters<T>` extracts the parameter types of a function as a tuple. `Parameters<typeof fn>` returns `[string, number]` if `fn(a: string, b: number)`. Useful for wrapping or proxying functions while preserving type safety.

**Memory Tip**: "`Parameters` = 'what does this function take?' Returns a tuple of param types."

---

### Q31. What is the `Exclude<T, U>` utility type?

**Answer**: `Exclude<T, U>` removes types from a union that are assignable to `U`. `Exclude<"a" | "b" | "c", "a">` results in `"b" | "c"`. Use it to narrow a union by removing unwanted members.

**Memory Tip**: "`Exclude` = 'remove these from the options.'"

---

### Q32. What is the `Extract<T, U>` utility type?

**Answer**: `Extract<T, U>` extracts from union `T` those types assignable to `U`. `Extract<string | number | boolean, string | number>` gives `string | number`. It's the inverse of `Exclude`.

**Memory Tip**: "`Extract` = 'keep only these types from the union.'"

---

### Q33. What is the `NonNullable<T>` utility type?

**Answer**: `NonNullable<T>` removes `null` and `undefined` from `T`. `NonNullable<string | null | undefined>` becomes `string`. Useful after runtime null checks when you want the type system to know the value is present.

**Memory Tip**: "`NonNullable` = 'no nulls allowed here.' The type-system bouncer for null/undefined."

---

### Q34. What are mapped types?

**Answer**: Mapped types create new types by transforming each property of an existing type. Syntax: `{ [K in keyof T]: NewType }`. All built-in utility types (`Partial`, `Required`, `Readonly`, `Pick`) are implemented as mapped types. You can also add/remove modifiers (`?`, `readonly`) with `+`/`-` prefixes.

**Memory Tip**: "Mapped types = a for-loop for types. Iterate over keys, transform each property."

---

### Q35. What are conditional types?

**Answer**: Conditional types choose between two types based on a condition: `T extends U ? X : Y`. If `T` is assignable to `U`, the result is `X`; otherwise `Y`. They distribute over union types. Combined with `infer`, they enable powerful type-level programming.

**Memory Tip**: "Conditional types = ternary operator for types. `T extends U ? X : Y` = 'if T matches U, use X, else Y.'"

---

### Q36. What is the `infer` keyword?

**Answer**: `infer` declares a type variable inside a conditional type, extracting types from other types. Example: `type ReturnOf<T> = T extends (...args: any[]) => infer R ? R : never` extracts the return type `R` from a function type `T`. It's the backbone of `ReturnType`, `Parameters`, and other utility types.

**Memory Tip**: "`infer` = 'figure out this type for me.' Like a regex capture group for types."

---

### Q37. What is declaration merging?

**Answer**: Declaration merging combines multiple declarations with the same name into one definition. Interfaces merge naturally (properties combine), while `type` aliases cannot be merged. This is why library authors prefer `interface` — consumers can extend them via merging.

**Memory Tip**: "Declaration merging = 'same name = same type.' Only interfaces can do this party trick."

---

### Q38. What are discriminated unions?

**Answer**: A discriminated union uses a shared literal property (the "discriminant") to distinguish between union members. Example: `{ kind: "circle"; radius: number } | { kind: "rect"; width: number }`. TypeScript narrows the type when you check `kind`, giving type-safe access to `radius` or `width`.

**Memory Tip**: "Discriminated union = 'check the tag to know what you're dealing with.' Like luggage tags at an airport."

---

### Q39. What is the `satisfies` operator?

**Answer**: `satisfies` (TypeScript 4.9+) checks that an expression matches a type without widening it. Example: `const palette = { red: [255, 0, 0], green: "#00ff00" } satisfies Record<string, string | number[]>`. The variable retains its precise literal types while being validated against the broader type.

**Memory Tip**: "`satisfies` = 'I want to check it matches, but keep the specific type.' Best of both worlds."

---

### Q40. What is the `as const` assertion?

**Answer**: `as const` tells TypeScript to infer the narrowest possible type — literal types instead of widened ones. `[1, 2, 3] as const` becomes `readonly [1, 2, 3]` instead of `number[]`. Essential for defining constants, action types, and config objects.

**Memory Tip**: "`as const` = 'freeze this value at its most specific type.' Like `Object.freeze()` for types."

---

### Q41. What is the `索引签名` (index signature)?

**Answer**: An index signature defines a type for dynamic keys: `{ [key: string]: number }` means any string key maps to a number value. It's used when you don't know all property names at design time. Note: index signatures allow any key, which can be too permissive.

**Memory Tip**: "Index signature = 'any key of this type maps to a value of that type.' Like a hashmap type."

---

### Q42. What are function overloads in TypeScript?

**Answer**: Function overloads let you define multiple type signatures for the same function. The implementation signature must be compatible with all overloads. This provides better type inference at call sites — the compiler knows the exact return type based on which overload matches.

**Memory Tip**: "Overloads = multiple faces for one function. Different inputs → different output types, same function name."

---

## SECTION C: ADVANCED (Q43–50)

---

### Q43. What are template literal types?

**Answer**: Template literal types combine string literal types using template syntax: `type EventName = `on${Capitalize<string>}``. They create new string literal types from existing ones. Combined with mapped types, they enable powerful string-based type transformations (e.g., CSS property types, event handler names).

**Memory Tip**: "Template literal types = template literals for types. Same backtick syntax, but at the type level."

---

### Q44. What is the `Record` type vs a Map? When would you use each?

**Answer**: `Record<K, V>` is a plain object type with string/number keys, while `Map<K, V>` is a JavaScript built-in with richer API (`size`, `forEach`, `entries`). Use `Record` for simple string-keyed lookups; use `Map` when you need non-string keys, ordered iteration, or easy size checking.

**Memory Tip**: "`Record` = lightweight object dictionary. `Map` = full-featured data structure."

---

### Q45. How does TypeScript handle `this` typing?

**Answer**: TypeScript lets you explicitly type `this` as the first parameter of a function: `function handleClick(this: HTMLElement, event: Event)`. This ensures `this` refers to the correct type inside the function body. Without it, `this` defaults to `any` in non-strict mode.

**Memory Tip**: "`this` as first parameter = 'the real first argument is what `this` points to.'"

---

### Q46. What are variadic tuple types?

**Answer**: Variadic tuple types (TypeScript 4.0+) allow generic type parameters in spread positions: `[...T, ...U]`. This enables type-safe function composition, `concat` operations, and tuple transformations. Example: `function concat<T extends any[], U extends any[]>(a: [...T], b: [...U]): [...T, ...U]`.

**Memory Tip**: "Variadic tuples = 'spread generics.' The `...` can now work with type parameters."

---

### Q47. What is tail-call optimization at the type level? How do you avoid recursive type depth errors?

**Answer**: TypeScript has a recursion depth limit (~1000 for conditional types). To avoid "Type instantiation is excessively deep" errors, use tail-recursive conditional types (the recursive call is the last operation), or break the recursion with intermediate types. TypeScript 4.5+ supports tail-call optimization for conditional types.

**Memory Tip**: "Deep recursion in types = stack overflow. Keep recursive conditional types in tail position."

---

### Q48. How do you implement a `DeepPartial<T>` utility type?

**Answer**: `DeepPartial<T>` recursively makes all properties (including nested objects) optional:
```typescript
type DeepPartial<T> = {
  [P in keyof T]?: T[P] extends object ? DeepPartial<T[P]> : T[P];
};
```
This is a mapped type that checks if each property is an object and, if so, applies `DeepPartial` recursively.

**Memory Tip**: "DeepPartial = Partial, but it goes ALL the way down. Think 'deep clone' but for optionality."

---

### Q49. What are branded/opaque types in TypeScript?

**Answer**: Branded types use intersection with a unique symbol to create nominally-typed wrappers: `type USD = number & { __brand: "USD" }`. This prevents accidentally mixing `USD` and `EUR` even though both are `number`. It's a pattern for nominal typing in TypeScript's structural type system.

**Memory Tip**: "Branded types = 'this isn't just a number, it's a SPECIFIC KIND of number.' Like a branded product vs. generic."

---

### Q50. How does TypeScript's type system relate to set theory?

**Answer**: TypeScript's type system is fundamentally set-theoretic: `A | B` is union (set union), `A & B` is intersection (set intersection), `keyof T` is the set of keys, and `T extends U` means T is a subset of U. `never` is the empty set ∅, `unknown` is the universal set. Understanding this makes advanced types intuitive.

**Memory Tip**: "Types = sets. `|` = union ∪, `&` = intersection ∩, `never` = ∅, `unknown` = universal set. Think like a mathematician."

---

## Quick Reference: Utility Types Cheat Sheet

| Utility Type | What It Does | Example |
|---|---|---|
| `Partial<T>` | All properties optional | `Partial<User>` |
| `Required<T>` | All properties required | `Required<User>` |
| `Readonly<T>` | All properties readonly | `Readonly<Config>` |
| `Pick<T, K>` | Select subset of properties | `Pick<User, "id" \| "name">` |
| `Omit<T, K>` | Remove properties | `Omit<User, "password">` |
| `Record<K, V>` | Object with key-value map | `Record<string, number>` |
| `ReturnType<T>` | Extract function return type | `ReturnType<typeof fn>` |
| `Parameters<T>` | Extract function params as tuple | `Parameters<typeof fn>` |
| `Exclude<T, U>` | Remove types from union | `Exclude<A \| B, B>` → `A` |
| `Extract<T, U>` | Keep matching types | `Extract<A \| B, B>` → `B` |
| `NonNullable<T>` | Remove null/undefined | `NonNullable<T \| null>` |
| `Awaited<T>` | Unwrap Promise type | `Awaited<Promise<string>>` → `string` |
| `NoInfer<T>` | Prevent inference on T | `NoInfer<string>` |

---

*Sources: [uByte](https://ubyte.dev/blog/typescript-interview-questions), [InterviewBit](https://www.interviewbit.com/typescript-interview-questions/), [QuizMaster](https://www.thequizmaster.io/interview-questions/typescript), [CodeForGeek](https://codeforgeek.com/top-50-typescript-interview-questions-with-answers/), [LambdaTest](https://lambdatest.com/learning-hub/typescript-interview-questions)*