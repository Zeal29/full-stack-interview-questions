# JavaScript Interview Questions - Top 50 Most Asked (2026)

> **Difficulty Split:** ~40% Basic (Q1-20) | ~45% Intermediate (Q21-42) | ~15% Advanced (Q43-50)
> **Audience:** Backend/Full-Stack Engineers
> **Sources:** ToolPal, WebDevSimplified, GeeksforGeeks, InterviewBit, Turing, JavaScript in Plain English

---

## GREEN BASIC (Questions 1-20)

---

### Q1. What is the difference between var, let, and const?

**Answer:**
| Feature | var | let | const |
|---------|-----|-----|-------|
| Scope | Function | Block | Block |
| Hoisting | Yes (initialized undefined) | Yes (TDZ) | Yes (TDZ) |
| Reassign | Yes | Yes | No |
| Redeclare | Yes | No | No |

Rule of thumb: Use const by default, let when reassignment is needed, never var in modern code. Note: const objects can have properties mutated -- const prevents reassignment, not mutation.

**Memory Tip:** var = vintage (avoid). let = changeable. const = constant (default choice).

---

### Q2. What is the difference between == and ===?

**Answer:**
- == (loose equality): Performs type coercion before comparing. 0 == false is true.
- === (strict equality): Compares both value AND type without coercion. 0 === false is false.

``javascript
0 == false    // true  (coerced)
0 === false   // false (different types)
null == undefined  // true  (special rule)
null === undefined // false (different types)
``

**Memory Tip:** === = three checks (type + value + reference). Always use === unless you explicitly want coercion.

---

### Q3. What are JavaScript data types?

**Answer:**
- Primitives (immutable): string, number, bigint, boolean, undefined, null, symbol
- Reference (mutable): object (includes arrays, functions, dates)

``javascript
typeof "hello"     // "string"
typeof 42          // "number"
typeof undefined   // "undefined"
typeof null        // "object" (historical bug!)
typeof {}          // "object"
typeof function(){} // "function"
``

**Memory Tip:** SNNBBUS -- String, Number, BigInt, Boolean, Undefined, null, Symbol. All primitives. Everything else is Object.

---

### Q4. What is hoisting in JavaScript?

**Answer:** Hoisting moves declarations to the top of their scope during compilation. var declarations are hoisted and initialized as undefined. let/const are hoisted but not initialized (Temporal Dead Zone -- accessing before declaration throws ReferenceError). Function declarations are fully hoisted (including body). Function expressions are NOT.

``javascript
console.log(a); // undefined (var hoisted)
var a = 5;

console.log(b); // ReferenceError (TDZ)
let b = 10;

greet(); // "Hello" (function declaration hoisted)
function greet() { console.log("Hello"); }
``

**Memory Tip:** var = hoisted with pillow (undefined). let/const = hoisted in danger zone (TDZ). Only declarations hoist, not initializations.

---

### Q5. What are closures?

**Answer:** A closure is a function that retains access to variables from its outer (enclosing) scope, even after the outer function has returned.

``javascript
function createCounter() {
  let count = 0;
  return {
    increment: () => ++count,
    getCount: () => count
  };
}
const counter = createCounter();
counter.increment(); // 1
counter.increment(); // 2
counter.getCount();  // 2
``

Use cases: data privacy, function factories, memoization, maintaining state in callbacks.

**Memory Tip:** Closure = function carrying its birth environment in a backpack. Variables live on even after the parent returns.

---

### Q6. What is the difference between null and undefined?

**Answer:**
- undefined: Variable declared but not assigned. Default for missing params and absent properties.
- null: Intentional absence of value. Must be explicitly assigned.

``javascript
let a;              // undefined
let b = null;       // null
typeof undefined    // "undefined"
typeof null         // "object" (legacy bug)
null == undefined   // true
null === undefined  // false
``

**Memory Tip:** undefined = doesn't exist yet (language default). null = intentionally nothing (developer set).

---

### Q7. What are template literals?

**Answer:** Template literals use backticks for string interpolation and multi-line strings. Use dollar-brace syntax to embed expressions. Support tagged templates for custom processing.

**Memory Tip:** Backticks = smart quotes. dollar-brace = inject JavaScript into strings.

---

### Q8. What is destructuring?

**Answer:** Extracting values from arrays/objects into variables:

``javascript
// Object destructuring
const { name, age, role = "user" } = { name: "Alice", age: 30 };

// Array destructuring
const [first, second, ...rest] = [1, 2, 3, 4, 5];

// Renaming
const { name: firstName } = { name: "Alice" };

// Nested
const { address: { city } } = { address: { city: "NYC" } };
``

**Memory Tip:** Destructuring = unpacking a suitcase into labeled drawers.

---

### Q9. What are spread and rest operators?

**Answer:** Both use triple-dot syntax:

``javascript
// Spread: Expands iterables
const arr2 = [...arr1, 4, 5];
const obj2 = { ...obj1, b: 2 };

// Rest: Collects remaining items
function sum(...numbers) {
  return numbers.reduce((a, b) => a + b, 0);
}
const [first, ...rest] = [1, 2, 3, 4];
``

**Memory Tip:** Spread = explode (unpack). Rest = vacuum (collect). Same triple-dot syntax, different direction.

---

### Q10. What is the difference between map, filter, and reduce?

**Answer:**

``javascript
const nums = [1, 2, 3, 4, 5, 6];

// map: Transform each element
nums.map(n => n * 2); // [2, 4, 6, 8, 10, 12]

// filter: Keep elements matching condition
nums.filter(n => n % 2 === 0); // [2, 4, 6]

// reduce: Accumulate into single value
nums.reduce((sum, n) => sum + n, 0); // 21
``

**Memory Tip:** map = transform each. filter = keep if true. reduce = fold into one.

---

### Q11. What are arrow functions and how do they differ from regular functions?

**Answer:** Arrow functions have no own this (inherits from enclosing scope), no arguments object, cannot be used as constructors, and cannot be hoisted. Ideal for callbacks. Avoid for object methods.

**Memory Tip:** Arrow = lean function. No own this = borrows from parent. Great for callbacks, bad for methods.

---

### Q12. What is the event loop in JavaScript?

**Answer:** JavaScript is single-threaded but handles async via the event loop:
1. Call Stack: Executes synchronous code
2. Web APIs: Browser features (setTimeout, fetch, DOM events)
3. Microtask Queue: Promises (higher priority)
4. Callback Queue: setTimeout, setInterval

``javascript
console.log('1');
setTimeout(() => console.log('2'), 0);
Promise.resolve().then(() => console.log('3'));
console.log('4');
// Output: 1, 4, 3, 2 (microtask before macrotask)
``

**Memory Tip:** Sync first, then Microtasks (Promises), then Macrotasks (setTimeout). Microtasks always jump the queue.

---

### Q13. What is event delegation?

**Answer:** Attach a single listener to a parent instead of many listeners on children. Events bubble up the DOM:

``javascript
document.querySelector('.list').addEventListener('click', (e) => {
  if (e.target.classList.contains('item')) {
    handleItemClick(e.target);
  }
});
``

**Memory Tip:** Event delegation = parent handles kids problems. One bouncer at the door instead of guards at every table.

---

### Q14. What are Promises?

**Answer:** A Promise represents the eventual result of an async operation with three states: Pending, Fulfilled, or Rejected.

``javascript
const fetchData = () => new Promise((resolve, reject) => {
  setTimeout(() => resolve({ id: 1, name: "Alice" }), 1000);
});

fetchData()
  .then(data => console.log(data))
  .catch(error => console.error(error))
  .finally(() => console.log("Done"));
``

**Memory Tip:** Promise = IOU. Pending = coming. Fulfilled = delivered. Rejected = sorry, couldn't.

---

### Q15. What is async/await?

**Answer:** Syntactic sugar over Promises that makes async code look synchronous:

``javascript
async function getUser(id) {
  try {
    const response = await fetch('/api/users/' + id);
    const user = await response.json();
    return user;
  } catch (error) {
    console.error("Failed:", error);
  }
}
``

**Memory Tip:** async/await = Promises in disguise. await = pause here until resolved. Always try/catch.

---

### Q16. Promise.all vs Promise.allSettled vs Promise.race vs Promise.any?

**Answer:**
- Promise.all: Rejects if ANY rejects. Use when all must succeed.
- Promise.allSettled: Waits for ALL, returns status+value for each.
- Promise.race: Resolves/rejects with FIRST to settle.
- Promise.any: Resolves with FIRST to FULFILL (ignores rejections).

**Memory Tip:** all = all must pass. allSettled = wait for everyone. race = first wins (good or bad). any = first success wins.

---

### Q17. What is the this keyword in JavaScript?

**Answer:** this depends on how a function is called:
- Regular function: this = global object (strict: undefined)
- Method: this = object before the dot
- Arrow function: this = enclosing scope (lexical)
- new keyword: this = newly created instance
- call/apply/bind: this = explicitly set first argument

**Memory Tip:** Arrow = lexical this (from where it's written). Regular = dynamic this (from how it's called).

---

### Q18. What is the DOM?

**Answer:** The DOM (Document Object Model) is a tree-like representation of an HTML document. JavaScript interacts with the DOM to read and modify page content, structure, and style using methods like getElementById, querySelector, addEventListener, createElement, appendChild.

**Memory Tip:** DOM = live tree map of your HTML. JavaScript = the hands that reshape it.

---

### Q19. What is event bubbling and capturing?

**Answer:** When an event fires on an element, it travels in three phases:
1. Capturing phase: Event goes from window down to target
2. Target phase: Event reaches the actual element
3. Bubbling phase: Event bubbles from target back up to window

Use event.stopPropagation() to stop propagation. event.preventDefault() prevents default behavior.

**Memory Tip:** Capturing = Cascade down. Bubbling = Bubble up. Most handlers listen on bubble phase.

---

### Q20. What is type coercion in JavaScript?

**Answer:** JavaScript automatically converts types in certain contexts. Falsy values: false, 0, -0, "", null, undefined, NaN, 0n. Everything else is truthy (including [], {}, "false").

``javascript
"5" + 3       // "53" (number to string)
"5" - 3       // 2   (string to number)
if (!value) { }  // true for all falsy values
if (value == null) { }  // only null and undefined (safer)
``

**Memory Tip:** Falsy list: 0, "", null, undefined, NaN, false. Everything else is truthy -- even empty arrays and objects!

---

## YELLOW INTERMEDIATE (Questions 21-42)

---

### Q21. What is prototypal inheritance?

**Answer:** Every JS object has an internal [[Prototype]] link. When accessing a property, JS walks up the prototype chain until found or null.

``javascript
const animal = { speak() { return this.name + ' makes a sound'; } };
const dog = Object.create(animal);
dog.name = "Rex";
dog.speak(); // "Rex makes a sound" (inherited from animal)
``

**Memory Tip:** Prototype chain = family tree lookup. JS asks: do I have this? does my parent? grandparent? until null.

---

### Q22. What are ES6 Classes?

**Answer:** Syntactic sugar over prototypal inheritance. extends sets up prototype chain. super calls parent. Hash prefix = private fields.

``javascript
class Animal {
  #name;
  constructor(name) { this.#name = name; }
  speak() { return this.#name + ' makes a sound'; }
}
class Dog extends Animal {
  constructor(name, breed) { super(name); this.breed = breed; }
}
``

**Memory Tip:** ES6 class = cleaner syntax for the same prototype system. Not true OOP classes like Java.

---

### Q23. Shallow copy vs deep copy?

**Answer:** Shallow copy (spread/Object.assign): Top-level only, nested objects shared. Deep copy (structuredClone): Everything cloned recursively.

``javascript
const original = { a: 1, nested: { b: 2 } };
const shallow = { ...original };
shallow.nested.b = 99;  // MUTATES original!
const deep = structuredClone(original);
deep.nested.b = 99;  // original unchanged
``

**Memory Tip:** structuredClone() = modern deep copy. Spread = shallow only.

---

### Q24. Object.freeze() vs const?

**Answer:** const prevents reassignment. Object.freeze() prevents property mutation (shallow only). Neither achieves deep immutability alone.

**Memory Tip:** const = can't reassign. freeze() = can't mutate (shallow).

---

### Q25. What is a Higher-Order Function?

**Answer:** A function that takes a function as argument or returns a function. Examples: map, filter, reduce, debounce, curry.

**Memory Tip:** HOF = function that works with functions.

---

### Q26. What are call, apply, and bind?

**Answer:** Methods to explicitly set this:
- call: Arguments comma-separated
- apply: Arguments as array
- bind: Returns new function with this bound

**Memory Tip:** call = comma. apply = array. bind = new function.

---

### Q27. What is currying?

**Answer:** Transforming a multi-argument function into single-argument functions:

``javascript
const multiply = a => b => a * b;
const double = multiply(2);
double(5);  // 10
``

**Memory Tip:** Currying = one argument at a time.

---

### Q28. What is debouncing?

**Answer:** Delays execution until a pause in calls. Only the last call executes.

``javascript
function debounce(func, delay) {
  let timeoutId;
  return (...args) => {
    clearTimeout(timeoutId);
    timeoutId = setTimeout(() => func(...args), delay);
  };
}
``

**Memory Tip:** Debounce = wait until quiet. Like an elevator waiting before closing.

---

### Q29. What is throttling?

**Answer:** Ensures function runs at most once per time period regardless of call frequency.

**Memory Tip:** Debounce = wait until quiet (last wins). Throttle = pace yourself (first wins, then wait).

---

### Q30. typeof operator gotchas?

**Answer:** typeof null === "object" and typeof NaN === "number" are historical bugs. Use Array.isArray() for arrays.

**Memory Tip:** Two JS bugs: typeof null = "object", typeof NaN = "number".

---

### Q31. instanceof operator?

**Answer:** Checks if object is instance of constructor via prototype chain. [] instanceof Array = true. Doesn't work across iframes.

---

### Q32. forEach vs map?

**Answer:** forEach iterates with side effects (returns undefined). map transforms and returns new array.

**Memory Tip:** forEach = do something. map = transform.

---

### Q33. Optional chaining (?.)?

**Answer:** Safely access nested properties: user?.profile?.address?.city returns undefined instead of error if any level is null/undefined.

**Memory Tip:** ?. = proceed if exists, else undefined.

---

### Q34. Nullish coalescing (??)?

**Answer:** Returns right value only for null/undefined (not 0 or ""):

``javascript
0 ?? 5000   // 0   (correct)
0 || 5000   // 5000 (wrong - falsy triggered)
"" ?? "hi"  // ""  (correct)
"" || "hi"  // "hi" (wrong)
``

**Memory Tip:** ?? = only null/undefined trigger default. || = any falsy triggers default.

---

### Q35. Generators?

**Answer:** Functions that pause/resume with yield:

``javascript
function* fibonacci() {
  let [a, b] = [0, 1];
  while (true) { yield a; [a, b] = [b, a + b]; }
}
const fib = fibonacci();
fib.next(); // { value: 0, done: false }
``

**Memory Tip:** Generator = pause button. yield = pause, .next() = resume.

---

### Q36. Set and Map?

**Answer:** Set = unique values collection. Map = key-value pairs with any key type, insertion-ordered.

**Memory Tip:** Set = unique club. Map = super object.

---

### Q37. structuredClone()?

**Answer:** Modern deep clone handling Dates, Maps, Sets, circular refs. Cannot clone Functions, DOM nodes.

**Memory Tip:** structuredClone = modern deep copy. Better than JSON round-trip.

---

### Q38. ES Modules?

**Answer:** Named exports (many per file), default export (one per file), dynamic import for code splitting. Static analysis at compile time vs CommonJS runtime require.

**Memory Tip:** export = send. import = receive. Default = one. Named = many.

---

### Q39. Object.assign() vs spread?

**Answer:** Both shallow merge. Spread preferred for readability. Object.assign mutates first arg.

**Memory Tip:** Spread = immutable. Object.assign = can mutate.

---

### Q40. JSON.parse/stringify?

**Answer:** stringify = object to text. parse = text to object. Loses undefined, functions; Dates become strings.

---

### Q41. try/catch/finally?

**Answer:** try = attempt. catch = handle errors. finally = cleanup (always runs).

---

### Q42. preventDefault vs stopPropagation?

**Answer:** preventDefault stops browser's default action. stopPropagation stops event from reaching parents.

**Memory Tip:** preventDefault = stop browser. stopPropagation = stop parent.

---

## RED ADVANCED (Questions 43-50)

---

### Q43. Memory leaks?

**Answer:** Common causes: forgotten listeners, uncleared timers, closure traps, detached DOM. Use WeakMap/WeakSet for caches. Chrome DevTools Memory panel to detect.

**Memory Tip:** Leaks = forgotten listeners, uncleared timers, closure traps, detached DOM. Clean up!

---

### Q44. Microtasks vs macrotasks?

**Answer:** Microtasks (Promises) = higher priority, run before rendering. Macrotasks (setTimeout) = lower priority, run after. Microtasks can starve macrotasks.

**Memory Tip:** Microtasks = VIP line. Macrotasks = regular line. VIPs first.

---

### Q45. WeakMap and WeakSet?

**Answer:** Weak references allowing garbage collection of keys. Not enumerable. Use for caches without preventing GC.

**Memory Tip:** Weak = GC can take it. Use for caches.

---

### Q46. Proxies?

**Answer:** Intercept object operations (get, set, delete) for validation, logging, reactivity.

``javascript
const user = new Proxy({ name: "Alice" }, {
  get(target, prop) { return prop in target ? target[prop] : "Not found"; },
  set(target, prop, value) { if (prop === "age" && value < 0) throw Error; target[prop] = value; return true; }
});
``

**Memory Tip:** Proxy = security guard for your object.

---

### Q47. LRU Cache implementation?

**Answer:** Use Map (insertion-ordered). First item = LRU. Access moves to end.

``javascript
class LRUCache {
  constructor(capacity) { this.capacity = capacity; this.cache = new Map(); }
  get(key) {
    if (!this.cache.has(key)) return -1;
    const v = this.cache.get(key);
    this.cache.delete(key); this.cache.set(key, v);
    return v;
  }
  put(key, value) {
    if (this.cache.has(key)) this.cache.delete(key);
    else if (this.cache.size >= this.capacity)
      this.cache.delete(this.cache.keys().next().value);
    this.cache.set(key, value);
  }
}
``

**Memory Tip:** LRU = Least Recently Used. Map order = access order. Evict first item.

---

### Q48. Object.keys vs values vs entries?

**Answer:** keys = property names. values = property values. entries = [key, value] pairs. fromEntries = pairs back to object.

---

### Q49. Memoization?

**Answer:** Cache function results by arguments:

``javascript
function memoize(fn) {
  const cache = new Map();
  return (...args) => {
    const key = JSON.stringify(args);
    if (cache.has(key)) return cache.get(key);
    const result = fn(...args);
    cache.set(key, result);
    return result;
  };
}
``

**Memory Tip:** Memoization = sticky note. Already calculated? Return cached. Not yet? Calculate and cache.

---

### Q50. Error handling in async code?

**Answer:** try/catch for async/await. .catch() for Promise chains. AbortController for cancellation. Promise.allSettled for partial failures.

``javascript
async function fetchData() {
  try {
    const res = await fetch("/api/data");
    if (!res.ok) throw new Error("HTTP " + res.status);
    return await res.json();
  } catch (err) { console.error(err); return null; }
  finally { hideLoader(); }
}
``

**Memory Tip:** try/catch + async/await. AbortController for cancellation. allSettled for partial failures.

---

## Quick Reference Table

| Difficulty | Questions | Focus |
|-----------|-----------|-------|
| Basic | Q1-Q20 | Data types, closures, hoisting, DOM, events, Promises |
| Intermediate | Q21-Q42 | Prototypes, classes, async patterns, HOFs, modules |
| Advanced | Q43-Q50 | Memory leaks, Proxies, WeakMap, LRU cache, memoization |

---

*Compiled from: ToolPal, WebDevSimplified, GeeksforGeeks, InterviewBit, Turing, JavaScript in Plain English (2024-2026)*