# JavaScript Interview Questions — Top 50

> **Difficulty Split:** Basic (Q1–Q20) | Intermediate (Q21–Q40) | Advanced (Q41–Q50)

---

## Basic Questions (Q1–Q20)

### Q1. What are the data types in JavaScript?

**Answer:** JavaScript has 8 data types:
- **Primitive**: `string`, `number`, `bigint`, `boolean`, `undefined`, `null`, `symbol`
- **Non-primitive**: `object` (includes arrays, functions, dates)

Primitives are immutable and compared by value. Objects are mutable and compared by reference.

**💡 Tip:** Remember "**SNNBUNS**" — String, Number, Null, BigInt, Undefined, Null, Symbol + Object. Use `typeof` to check.

---

### Q2. What is the difference between `var`, `let`, and `const`?

**Answer:**
- `var`: function-scoped, hoisted (initialized as `undefined`), can be redeclared
- `let`: block-scoped, hoisted but not initialized (TDZ), can be reassigned
- `const`: block-scoped, hoisted but not initialized (TDZ), cannot be reassigned (but objects/arrays can be mutated)

**💡 Tip:** Always use `const` by default. Use `let` only when reassignment is needed. **Never use `var`.**

---

### Q3. What is hoisting in JavaScript?

**Answer:** Hoisting moves `var` declarations and `function` declarations to the top of their scope during compilation. Only the **declaration** is hoisted, not the initialization. `let`/`const` are hoisted but remain in the **Temporal Dead Zone** (TDZ) until their declaration line.

**💡 Tip:** `var` hoists with `undefined`. `let`/`const` hoist but stay in **TDZ** (ReferenceError if accessed early). Functions hoist entirely.

---

### Q4. What is the difference between `==` and `===`?

**Answer:**
- `==` (loose equality): performs type coercion before comparing (`"5" == 5` → `true`)
- `===` (strict equality): no type coercion, values AND types must match (`"5" === 5` → `false`)

**💡 Tip:** Always use `===`. `==` has unpredictable coercion rules (`[] == false` is `true`). Strict equality prevents bugs.

---

### Q5. What is closure in JavaScript?

**Answer:** A closure is a function that remembers and accesses variables from its outer (lexically enclosing) scope even after the outer function has returned. Every function in JS creates a closure.

```javascript
function outer() {
  let count = 0;
  return function inner() { return ++count; };
}
const counter = outer();
counter(); // 1
counter(); // 2
```

**💡 Tip:** Closure = **function + its birth environment**. Like a person who never forgets where they came from.

---

### Q6. What is the difference between `null` and `undefined`?

**Answer:**
- `undefined`: variable declared but not assigned a value (default)
- `null`: intentionally assigned "empty" or "nothing" value
- `typeof undefined` → `"undefined"`, `typeof null` → `"object"` (JS bug)

**💡 Tip:** `undefined` = **JavaScript says** "I don't know". `null` = **Developer says** "this is empty".

---

### Q7. What are the different ways to create objects in JavaScript?

**Answer:**
1. Object literal: `const obj = { key: "value" }`
2. `new Object()`: `const obj = new Object()`
3. Constructor function: `function Obj() { }` then `new Obj()`
4. `Object.create()`: `Object.create(proto)`
5. ES6 Class: `class Obj { }` then `new Obj()`

**💡 Tip:** Object literal is simplest and most common. Classes are syntactic sugar over constructor functions.

---

### Q8. What is the `this` keyword in JavaScript?

**Answer:** `this` refers to the object that is currently executing the code:
- In a **method**: the object before the dot
- Alone: `globalThis` (window in browser, global in Node)
- In an **arrow function**: inherits `this` from enclosing scope (lexically bound)
- With `new`: the newly created instance
- Explicit binding: `call()`, `apply()`, `bind()` set `this` manually

**💡 Tip:** Arrow functions **don't have their own `this`** — they borrow from parent. Regular functions get `this` from how they're called.

---

### Q9. What is the event loop?

**Answer:** The event loop is the mechanism that allows JavaScript to perform non-blocking asynchronous operations despite being single-threaded. It continuously checks: (1) Execute synchronous code on call stack, (2) When stack empty, check microtask queue (Promises), (3) Then check macrotask queue (setTimeout, I/O).

**💡 Tip:** Event loop = **waiter metaphor** — take orders (sync), give to kitchen (async APIs), serve other tables (other code), deliver food when ready (callback).

---

### Q10. What are Promises?

**Answer:** A Promise represents the eventual result of an async operation. Three states: **pending** → **fulfilled** (resolved) or **rejected**. Chain with `.then()`, `.catch()`, `.finally()`.

```javascript
fetch('/api')
  .then(res => res.json())
  .catch(err => console.error(err));
```

**💡 Tip:** Promise = **IOU** (I owe you). "I promise to give you data later — either the data (resolve) or an error (reject)."

---

### Q11. What is `async/await`?

**Answer:** Syntactic sugar over Promises that makes async code look synchronous. `async` function returns a Promise. `await` pauses execution until the Promise resolves.

```javascript
async function getData() {
  try {
    const res = await fetch('/api');
    const data = await res.json();
    return data;
  } catch (err) {
    console.error(err);
  }
}
```

**💡 Tip:** `async/await` = **cleaner Promises**. Always use `try/catch` with `await`. `await` only works inside `async`.

---

### Q12. What is the difference between `call`, `apply`, and `bind`?

**Answer:**
- `call(thisArg, arg1, arg2)` — calls function with given `this` and individual arguments
- `apply(thisArg, [args])` — same as call but arguments as array
- `bind(thisArg)` — returns NEW function with `this` permanently bound (doesn't call immediately)

**💡 Tip:** **C**all = **C**omma-separated args. **A**pply = **A**rray args. **B**ind = **B**orrow permanently.

---

### Q13. What is prototypal inheritance?

**Answer:** Objects inherit directly from other objects via the prototype chain. Every object has a hidden `[[Prototype]]` link. When a property isn't found on the object, JS looks up the chain until it reaches `null`. `Object.getPrototypeOf(obj)` or `__proto__` accesses it.

**💡 Tip:** Prototype chain = **inheritance through delegation**. Object → Prototype → Prototype → ... → null. Like asking your parent, then grandparent for something.

---

### Q14. What are arrow functions and how do they differ from regular functions?

**Answer:** Arrow functions (`() => {}`) are concise function expressions that:
- Don't have their own `this` (lexically bound)
- Don't have `arguments` object
- Can't be used as constructors (no `new`)
- No `prototype` property
- Implicit return for single expressions

**💡 Tip:** Arrow functions = **lean functions**. Best for callbacks and closures. Don't use them as object methods (you lose `this`).

---

### Q15. What is array destructuring and object destructuring?

**Answer:** Extract values from arrays/objects into variables:
```javascript
// Array
const [first, second, ...rest] = [1, 2, 3, 4];
// Object
const { name, age, role = "dev" } = { name: "Ali", age: 25 };
// Rename
const { name: userName } = obj;
```

**💡 Tip:** Destructuring = **unpack values** with pattern matching. `[]` for arrays, `{}` for objects. Default values with `=`.

---

### Q16. What is the spread operator (`...`)?

**Answer:** The spread operator expands iterables:
- Copy arrays: `[...arr1, ...arr2]`
- Copy objects: `{ ...obj1, ...obj2 }` (shallow merge)
- Function args: `func(...args)`
- Always creates a **shallow copy** (nested objects still by reference)

**💡 Tip:** Spread = **unpack** values. Spread in `[]`/`{}` = copy/merge. Spread in function args = expand. It's **shallow** — nested objects share references.

---

### Q17. What are template literals?

**Answer:** Template literals use backticks `` ` `` for strings with embedded expressions and multi-line support:
```javascript
const greeting = `Hello, ${name}!`;
const html = `<div>${content}</div>`;
```
Tagged templates: `fn\`string\`` allow custom processing.

**💡 Tip:** Backticks = **smart quotes**. `${}` for variables, multi-line without `\n`, tagged templates for advanced use.

---

### Q18. What is the difference between `map`, `filter`, and `reduce`?

**Answer:**
- `map(fn)` — transforms each element, returns new array of same length
- `filter(fn)` — keeps elements where fn returns true, returns shorter/new array
- `reduce(fn, init)` — accumulates all elements into a single value

**💡 Tip:** **Map** = transform all. **Filter** = keep some. **Reduce** = combine into one. They **don't mutate** the original array.

---

### Q19. What is event bubbling and event capturing?

**Answer:**
- **Capturing phase**: event travels from window down to target element
- **Target phase**: event reaches the target
- **Bubbling phase**: event bubbles up from target to window (default behavior)

Use `addEventListener(event, fn, true)` for capturing. `event.stopPropagation()` stops propagation.

**💡 Tip:** Event flow = **Capture** (top → down) → **Target** → **Bubble** (bottom → up). Most handlers fire during bubble phase.

---

### Q20. What is the `typeof` operator?

**Answer:** Returns a string indicating the type:
- `typeof "hello"` → `"string"`
- `typeof 42` → `"number"`
- `typeof undefined` → `"undefined"`
- `typeof null` → `"object"` ⚠️ (famous JS bug)
- `typeof []` → `"object"` (use `Array.isArray()`)
- `typeof function(){}` → `"function"`

**💡 Tip:** `typeof null` = `"object"` is a **legacy bug**. For arrays, always use `Array.isArray()` instead of `typeof`.

---

## Intermediate Questions (Q21–Q40)

### Q21. What is the Temporal Dead Zone (TDZ)?

**Answer:** The TDZ is the period between entering a scope and the actual declaration of a `let`/`const` variable. Accessing the variable in this zone throws a `ReferenceError`. Unlike `var`, which is hoisted with `undefined`, `let`/`const` are hoisted but uninitialized.

**💡 Tip:** TDZ = "no touching until declared." It prevents using variables before their declaration — a safety net `var` didn't have.

---

### Q22. What is the difference between shallow copy and deep copy?

**Answer:**
- **Shallow copy**: copies top-level values; nested objects share references (`Object.assign()`, spread `{...obj}`)
- **Deep copy**: recursively copies all nested objects (`structuredClone()`, `JSON.parse(JSON.stringify(obj))`)

**💡 Tip:** `structuredClone()` = modern deep clone. `JSON.parse(JSON.stringify())` = old hack (loses functions, dates, undefined).

---

### Q23. What is the `new` keyword doing?

**Answer:** `new` does four things:
1. Creates a new empty object
2. Sets the object's prototype to the constructor's `.prototype`
3. Calls the constructor with `this` bound to the new object
4. Returns the object (unless constructor returns another object)

**💡 Tip:** `new` = **birth** — creates object, links prototype, runs constructor, returns instance.

---

### Q24. What are generator functions?

**Answer:** Generators (`function*`) can pause and resume execution using `yield`:
```javascript
function* idGenerator() {
  let id = 1;
  while (true) yield id++;
}
const gen = idGenerator();
gen.next(); // { value: 1, done: false }
gen.next(); // { value: 2, done: false }
```

**💡 Tip:** Generator = **pause/resume function**. `yield` = pause point. `next()` = resume. Useful for sequences, lazy evaluation, iterators.

---

### Q25. What is the `Promise.all()`, `Promise.race()`, `Promise.allSettled()`, and `Promise.any()`?

**Answer:**
- `Promise.all([...])` — resolves when ALL resolve, rejects on first rejection
- `Promise.race([...])` — resolves/rejects with the first settled promise
- `Promise.allSettled([...])` — resolves when ALL settle (regardless of reject/resolve)
- `Promise.any([...])` — resolves with first fulfilled, rejects only if ALL reject

**💡 Tip:** **all** = everyone must pass. **race** = first to finish wins. **allSettled** = wait for everyone, don't care if they fail. **any** = first success.

---

### Q26. What is the difference between `Object.freeze()` and `Object.seal()`?

**Answer:**
- `Object.freeze(obj)`: cannot add, remove, OR modify properties (deep freeze needed for nested objects)
- `Object.seal(obj)`: cannot add or remove properties, but CAN modify existing ones

Both only work at the top level — nested objects are not frozen/sealed.

**💡 Tip:** Freeze = **total lockdown**. Seal = **no new/existing properties added/removed but values can change**.

---

### Q27. What is debouncing and throttling?

**Answer:**
- **Debounce**: delays execution until a pause in calls (e.g., search input — only fire after user stops typing for 300ms)
- **Throttle**: executes at most once per time period (e.g., scroll handler — fire at most every 100ms)

Both prevent performance issues from rapid repeated function calls.

**💡 Tip:** Debounce = **"wait until they stop"** (search box). Throttle = **"at most every X ms"** (scroll/resize).

---

### Q28. What are ES6 Modules (`import`/`export`)?

**Answer:**
- Named export: `export const name = "value"` → `import { name } from './file'`
- Default export: `export default function()` → `import myFunc from './file'`
- Modules are **strict mode** by default
- Static structure (analyzed at compile time, not runtime)

**💡 Tip:** Named = `{ braces }` for specific imports. Default = no braces, any name. One default per file, unlimited named.

---

### Q29. What is the `Set` and `Map` data structure?

**Answer:**
- `Set`: collection of **unique** values. Use to remove duplicates: `[...new Set(arr)]`
- `Map`: key-value pairs where keys can be any type (not just strings like objects). Has `.size`, `.has()`, `.delete()`

**💡 Tip:** Set = **unique array**. Map = **better object** (any key type, size tracked, ordered). Use `Set` for deduplication, `Map` for flexible keys.

---

### Q30. What is the `WeakMap` and `WeakSet`?

**Answer:**
- `WeakMap`: keys must be objects, and keys are **weakly held** (garbage collected if no other references). No iteration, no `.size`.
- `WeakSet`: same concept for unique objects. Used for tracking/marking objects without preventing GC.

**💡 Tip:** Weak = **don't prevent garbage collection**. Use when you want to associate data with objects without leaking memory.

---

### Q31. What is the `Symbol` type?

**Answer:** `Symbol` creates unique, immutable identifiers: `const id = Symbol('id')`. Every Symbol is unique (`Symbol('id') !== Symbol('id')`). Used as object property keys to avoid naming collisions. Not enumerated in `for...in` or `Object.keys()`.

**💡 Tip:** Symbol = **truly unique ID**. Even with the same description, two Symbols are different. Used internally by JS (e.g., `Symbol.iterator`).

---

### Q32. What is the `Proxy` object?

**Answer:** `Proxy` intercepts and customizes fundamental operations on objects:
```javascript
const handler = {
  get(target, prop) { return prop in target ? target[prop] : 'default'; }
};
const proxy = new Proxy(obj, handler);
```

Traps: `get`, `set`, `has`, `deleteProperty`, `apply`, etc. Used for validation, logging, access control.

**💡 Tip:** Proxy = **middleman for objects**. Intercepts reads, writes, deletes. Vue 3 uses Proxies for reactivity.

---

### Q33. What are higher-order functions?

**Answer:** A higher-order function either: (1) takes a function as an argument, or (2) returns a function. Examples: `map()`, `filter()`, `reduce()`, `setTimeout()`, `addEventListener()`, `curry()`, `debounce()`.

**💡 Tip:** Higher-order function = **function that works with functions**. Like a manager who delegates tasks to workers.

---

### Q34. What is currying?

**Answer:** Currying transforms a function with multiple arguments into a sequence of functions each taking one argument:
```javascript
const add = a => b => c => a + b + c;
add(1)(2)(3); // 6
```

Enables partial application and function reuse.

**💡 Tip:** Currying = **one argument at a time**. `f(a, b, c)` becomes `f(a)(b)(c)`. Useful for creating specialized functions.

---

### Q35. What is the difference between `setTimeout` and `setInterval`?

**Answer:**
- `setTimeout(fn, delay)` — executes `fn` **once** after `delay` ms
- `setInterval(fn, delay)` — executes `fn` **repeatedly** every `delay` ms
- Both return an ID for `clearTimeout(id)` / `clearInterval(id)`

**💡 Tip:** Timeout = **one-shot timer**. Interval = **repeating timer**. Always clean up intervals to prevent memory leaks.

---

### Q36. What is the `Reflect` API?

**Answer:** `Reflect` provides static methods that mirror proxy trap methods: `Reflect.get()`, `Reflect.set()`, `Reflect.has()`, `Reflect.apply()`, `Reflect.construct()`. It's the functional alternative to operators and old `Object` methods. Returns booleans instead of throwing.

**💡 Tip:** Reflect = **cleaner object operations**. `Reflect.has(obj, key)` instead of `key in obj`. Always returns success boolean, never throws.

---

### Q37. What is the `structuredClone()` method?

**Answer:** `structuredClone(value)` creates a deep clone of any structured data: objects, arrays, Maps, Sets, Date, RegExp, ArrayBuffer, etc. Handles circular references. Supported in modern browsers and Node.js 17+.

**💡 Tip:** `structuredClone()` = **modern deep clone**. Replaces `JSON.parse(JSON.stringify())` hack. Handles dates, Maps, Sets — JSON can't.

---

### Q38. What are the `toString()` and `valueOf()` methods?

**Answer:** Every object inherits these:
- `valueOf()` — returns the primitive value of an object (used in arithmetic)
- `toString()` — returns a string representation (used in string concatenation)

JavaScript calls them automatically during type coercion. You can override them in custom objects.

**💡 Tip:** `valueOf()` = **number context** (math). `toString()` = **string context** (concatenation). JS calls them during implicit coercion.

---

### Q39. What is the `Intl` (Internationalization) API?

**Answer:** `Intl` provides locale-sensitive formatting:
- `Intl.DateTimeFormat` — date/time formatting per locale
- `Intl.NumberFormat` — number/currency formatting
- `Intl.RelativeTimeFormat` — "2 days ago", "in 3 hours"
- `Intl.ListFormat` — "A, B, and C"

**💡 Tip:** `Intl` = **globalization made easy**. No libraries needed for dates, numbers, currencies in different locales.

---

### Q40. What is the difference between `Array.forEach()` and `Array.map()`?

**Answer:**
- `forEach()` — iterates and executes a function, returns `undefined`. Cannot break/continue.
- `map()` — transforms each element, returns a **new array**. Chainable.

Use `forEach` for side effects (logging, DOM updates). Use `map` for data transformation.

**💡 Tip:** `forEach` = **do something** (no return). `map` = **transform** (returns new array). Prefer `map`/`filter`/`reduce` for functional style.

---

## Advanced Questions (Q41–Q50)

### Q41. What is the microtask queue vs macrotask queue?

**Answer:**
- **Microtask queue**: Promise callbacks (`.then/catch/finally`), `queueMicrotask()`, `MutationObserver`. Executes after every task, before rendering.
- **Macrotask queue**: `setTimeout`, `setInterval`, I/O callbacks, UI rendering. Executes one per event loop iteration.

Priority: Sync code → Microtasks (ALL) → ONE Macrotask → Microtasks → Render → repeat.

**💡 Tip:** Microtasks = **VIP line** (Promises). Macrotasks = **regular line** (setTimeout). VIP always goes first, and ALL VIPs go before one regular.

---

### Q42. What is the memory lifecycle and garbage collection in JavaScript?

**Answer:** JS memory lifecycle: **Allocate** → **Use** → **Release**. Garbage collection uses **mark-and-sweep**: starts from roots (global, call stack), marks all reachable objects, sweeps (frees) unreachable ones. Modern engines also use generational GC (young/old generations).

**💡 Tip:** Memory leaks happen when: forgotten timers, detached DOM references, closures holding references, global variables. GC frees **unreachable** objects.

---

### Q43. What is the Event Delegation pattern?

**Answer:** Instead of attaching event listeners to each child, attach ONE listener to the parent. Use `event.target` to identify which child was clicked. Leverages event bubbling.

```javascript
document.getElementById('list').addEventListener('click', (e) => {
  if (e.target.tagName === 'LI') { /* handle */ }
});
```

**💡 Tip:** Event delegation = **one listener for many children**. More efficient, works with dynamically added elements. Uses event bubbling.

---

### Q44. What is the difference between `ArrayBuffer`, `TypedArray`, and `DataView`?

**Answer:**
- `ArrayBuffer` — fixed-length raw binary data buffer (can't read/write directly)
- `TypedArray` (`Int32Array`, `Float64Array`, etc.) — typed view into an ArrayBuffer
- `DataView` — flexible view that reads/writes different types at any offset (multi-endian)

**💡 Tip:** ArrayBuffer = **raw bytes**. TypedArray = **specific type view** (all same type). DataView = **mixed types** (flexible reading).

---

### Q45. What are Web Workers?

**Answer:** Web Workers run JavaScript in a **background thread** without blocking the UI. Communication via `postMessage()` and `onmessage`. Cannot access DOM directly. Types: Dedicated Worker (one-to-one), Shared Worker (many-to-one), Service Worker (offline/proxy).

**💡 Tip:** Workers = **background threads**. Heavy computation goes here to keep UI responsive. No DOM access — communicate via messages.

---

### Q46. What is memoization?

**Answer:** Memoization caches function results to avoid recomputation:
```javascript
function memoize(fn) {
  const cache = {};
  return (...args) => {
    const key = JSON.stringify(args);
    return cache[key] ?? (cache[key] = fn(...args));
  };
}
```

Trade-off: memory for speed. Best for expensive, pure functions with repeated inputs.

**💡 Tip:** Memoization = **remember past results**. Like saving calculator results — don't recompute if you already know the answer.

---

### Q47. What is the difference between ES6 Class and Constructor Functions?

**Answer:** ES6 Classes are syntactic sugar over constructor functions + prototypes. Key differences:
- Classes can't be called without `new` (constructors can)
- Class methods are non-enumerable
- Classes have `super()` for cleaner inheritance
- Hoisting: classes are NOT hoisted (constructors are)

**💡 Tip:** Class = **cleaner syntax** for the same prototype system. Under the hood, it's still prototypal inheritance, just prettier.

---

### Q48. What is tail call optimization?

**Answer:** TCO allows recursive functions to reuse the current stack frame if the recursive call is the **last operation** (tail position). Prevents stack overflow for deep recursion. Only supported in Safari/WebKit — NOT in Chrome/Firefox (V8/SpiderMonkey opted out).

**💡 Tip:** TCO = **recursion without stack growth**. Limited browser support — use iterative approaches in production instead.

---

### Q49. What is the difference between `Error` and `Exception` in JavaScript?

**Answer:** JavaScript doesn't have "exceptions" as a separate type — it has **Error objects** that are **thrown**:
- `Error`, `TypeError`, `ReferenceError`, `SyntaxError`, `RangeError`, `URIError`
- `throw new Error('msg')` creates the error
- `try/catch/finally` handles it
- Custom errors: `class AppError extends Error {}`

**💡 Tip:** Error = the **object**. Throwing it = the **action**. Catching it = the **handling**. Always extend `Error` for custom errors.

---

### Q50. What is the requestAnimationFrame (rAF)?

**Answer:** `requestAnimationFrame(cb)` schedules a function to run before the next browser repaint (~60fps = every 16.67ms). It's the correct way to animate in JavaScript — pauses when tab is inactive (saves CPU), syncs with browser paint cycle.

**💡 Tip:** rAF = **sync with screen refresh**. Always use it for animations instead of `setInterval`. Cancel with `cancelAnimationFrame(id)`.

---
