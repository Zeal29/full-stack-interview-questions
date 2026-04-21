# React Interview Questions — Top 50

> **Difficulty Split**: ~40% Basic (Q1–20), ~45% Intermediate (Q21–42), ~15% Advanced (Q43–50)
> **Focus**: Hooks, lifecycle, state management, performance — tailored for backend/full-stack engineers

---

## SECTION A: BASIC / FUNDAMENTAL (Q1–20)

---

### Q1. What is React? How is it different from other frontend frameworks?

**Answer**: React is a JavaScript **library** (not a framework) for building user interfaces through composable components. Unlike Angular (full framework) or Vue (progressive framework), React only handles the view layer — you choose your own router, state management, and build tools. It uses a virtual DOM for efficient updates and a unidirectional data flow.

**Memory Tip**: "React is a **library**, not a framework. Think of it as the 'rendering engine' — you bolt on routing, state, and other pieces yourself."

---

### Q2. What is JSX? How does it work?

**Answer**: JSX is a syntax extension that lets you write HTML-like code inside JavaScript. It's syntactic sugar for `React.createElement()` calls — `<div className="x">Hello</div>` compiles to `React.createElement('div', {className: 'x'}, 'Hello')`. Babel or the TypeScript compiler transforms JSX at build time.

**Memory Tip**: "JSX = JavaScript + XML. It's NOT HTML in JS — it's `createElement()` calls with a pretty face."

---

### Q3. What is the difference between a functional component and a class component?

**Answer**: Functional components are plain JavaScript functions that return JSX. Class components extend `React.Component` and use a `render()` method. Since React 16.8 (Hooks), functional components can do everything class components can — state, lifecycle, refs. Modern React development almost exclusively uses functional components.

**Memory Tip**: "Functional = the modern way (with hooks). Class = the legacy way (with lifecycle methods). If starting fresh, go functional."

---

### Q4. What are props in React?

**Answer**: Props (properties) are read-only data passed from a parent component to a child component. They flow **one direction** — parent → child. The child cannot modify its props; it can only read them. To communicate back up, the parent passes callback functions as props.

**Memory Tip**: "Props = function arguments for components. **P**arent **R**esolves **O**utput **P**arameters **S**ecurely (one-way)."

---

### Q5. What is state in React?

**Answer**: State is mutable data owned by a component that determines its behavior and rendering. When state changes (via `setState` or the setter from `useState`), React re-renders the component. State is local to the component unless lifted up or shared via context.

**Memory Tip**: "State = the component's memory. Props are what you're given; state is what you own."

---

### Q6. What is the `useState` hook?

**Answer**: `useState` adds local state to a functional component. It returns an array `[value, setter]`. Example: `const [count, setCount] = useState(0)`. The setter can accept a new value or an updater function `prev => prev + 1`. State updates are asynchronous and may be batched.

**Memory Tip**: "`useState` = the simplest hook. Returns `[what you have, how to change it]`. Always destructure as a pair."

---

### Q7. What is the `useEffect` hook?

**Answer**: `useEffect` runs side effects in functional components — data fetching, subscriptions, DOM manipulation. It runs after every render by default, or only when dependencies change if you provide a dependency array `[]`. Return a cleanup function to unsubscribe or clean up timers.

**Memory Tip**: "`useEffect` = 'do something after render.' Think: componentDidMount + componentDidUpdate + componentWillUnmount in one hook."

---

### Q8. What is the dependency array in `useEffect`?

**Answer**: The dependency array `[]` controls when `useEffect` re-runs. No array = runs after every render. Empty array `[]` = runs only on mount. `[a, b]` = runs when `a` or `b` change. Missing dependencies is the #1 cause of stale closure bugs. ESLint's `exhaustive-deps` rule catches this.

**Memory Tip**: "Dependency array = 'watch list.' Empty = watch nothing (mount only). Missing = watch everything (every render)."

---

### Q9. What is the virtual DOM?

**Answer**: The virtual DOM is a lightweight JavaScript representation of the real DOM. React keeps a virtual copy, and when state/props change, it creates a new virtual tree, diffs it against the old one (reconciliation), and applies only the minimal set of changes to the real DOM. This makes updates efficient.

**Memory Tip**: "Virtual DOM = a draft. React edits the draft first, compares with the current version, then applies only the changes (diff/patch)."

---

### Q10. What is reconciliation in React?

**Answer**: Reconciliation is React's algorithm for diffing two virtual DOM trees and determining the minimum number of DOM operations needed. React uses heuristics: elements of different types trigger full replacement; same-type elements trigger attribute updates; `key` props help identify list item changes.

**Memory Tip**: "Reconciliation = React's 'what changed?' algorithm. Like comparing two documents side by side."

---

### Q11. Why does React need `key` props in lists?

**Answer**: The `key` prop helps React identify which items in a list have changed, been added, or removed. Without unique keys, React uses array indices, which causes incorrect DOM reuse when items are reordered, inserted, or deleted. Always use stable, unique identifiers (like IDs) as keys — never array indices for dynamic lists.

**Memory Tip**: "`key` = name tag for list items. Without it, React plays guessing games. With it, React knows exactly who's who."

---

### Q12. What is the `useRef` hook?

**Answer**: `useRef` returns a mutable reference object `{ current: value }` that persists across renders without triggering re-renders. Use cases: accessing DOM elements directly, storing previous values, keeping mutable instance variables. Unlike state, changing `.current` does NOT cause a re-render.

**Memory Tip**: "`useRef` = a box you can write to without triggering re-renders. 'The silent storage'."

---

### Q13. What is the `useContext` hook?

**Answer**: `useContext` reads a value from a React Context without prop drilling. Create context with `createContext()`, provide it with `<Context.Provider value={...}>`, and consume it with `const value = useContext(MyContext)`. It re-renders the component when the context value changes.

**Memory Tip**: "`useContext` = a teleport for data. Skip the prop-drilling chain and beam data directly to where it's needed."

---

### Q14. What is prop drilling?

**Answer**: Prop drilling is the process of passing data through multiple intermediate components that don't need it, just to reach a deeply nested child. Example: `App → Layout → Sidebar → UserMenu` passing `user` prop through each layer. Solutions: Context API, component composition, or state management libraries.

**Memory Tip**: "Prop drilling = playing 'telephone' with props. By the time data reaches the last component, three middlemen handled it for no reason."

---

### Q15. What is conditional rendering in React?

**Answer**: Conditional rendering means showing different UI based on conditions. Common patterns: ternary `{isLoggedIn ? <Dashboard /> : <Login />}`, logical AND `{error && <ErrorMessage />}`, early returns (`if (!data) return <Loading />`), or enum/object mapping for multiple states.

**Memory Tip**: "Conditional rendering = `if/else` for UI. Ternary for two options, `&&` for show/hide, early return for guards."

---

### Q16. How do you handle events in React?

**Answer**: React uses synthetic events — cross-browser wrappers around native DOM events. Attach handlers via JSX attributes: `onClick={handleClick}`. Handler receives a `SyntheticEvent` object. Key difference from HTML: use `onClick` (camelCase), not `onclick` (lowercase), and pass a function reference, not a string.

**Memory Tip**: "React events = camelCase (`onClick`), pass function reference (`{fn}`), receive `SyntheticEvent`. Same concept, different syntax than vanilla HTML."

---

### Q17. What is the difference between controlled and uncontrolled components?

**Answer**: **Controlled** components have their value driven by React state: `<input value={name} onChange={e => setName(e.target.value)} />`. **Uncontrolled** components use refs to access DOM values directly: `inputRef.current.value`. Controlled = React is the "source of truth." Uncontrolled = the DOM is.

**Memory Tip**: "Controlled = React drives the car. Uncontrolled = React reads the odometer. Prefer controlled for predictability."

---

### Q18. What are React fragments?

**Answer**: Fragments (`<></>` or `<React.Fragment>`) let you group multiple elements without adding extra DOM nodes. Useful when a component needs to return multiple top-level elements but you don't want a wrapper `<div>`. The keyed version `<React.Fragment key={id}>` is needed in lists.

**Memory Tip**: "Fragments = invisible containers. They group elements without polluting the DOM with extra `<div>`s."

---

### Q19. What is the component lifecycle in React?

**Answer**: In class components: `constructor → render → componentDidMount → (updates) → componentDidUpdate → componentWillUnmount`. In functional components with hooks: `useState` initialization → render → `useEffect` (mount) → (updates) → `useEffect` (update) → cleanup function (unmount).

**Memory Tip**: "Lifecycle = Birth (mount), Growth (update), Death (unmount). Hooks map: mount = `useEffect([], {})`, update = `useEffect([deps])`, unmount = cleanup function."

---

### Q20. What is `React.memo`?

**Answer**: `React.memo` is a higher-order component that memoizes a functional component — it only re-renders if its props change. Use it for pure components that receive complex props and render expensively. It performs a shallow comparison by default; pass a custom comparison function as the second argument for deep comparison.

**Memory Tip**: "`React.memo` = 'only re-render if props changed.' Like a memo — don't repeat yourself if nothing's different."

---

## SECTION B: INTERMEDIATE (Q21–42)

---

### Q21. What is the `useReducer` hook? When would you use it over `useState`?

**Answer**: `useReducer` manages complex state via a reducer function `(state, action) => newState`, similar to Redux. Use it when: state has multiple related fields, next state depends on previous state, or you need predictable state transitions. `useState` is simpler for independent primitive values.

**Memory Tip**: "`useReducer` = Redux-lite inside a component. If your `useState` logic has too many `if/else` branches, switch to `useReducer`."

---

### Q22. What is the `useCallback` hook?

**Answer**: `useCallback` memoizes a function so it keeps the same reference across renders (unless dependencies change). This prevents unnecessary re-renders of child components that receive the function as a prop. Syntax: `const fn = useCallback(() => { ... }, [deps])`.

**Memory Tip**: "`useCallback` = 'remember this function.' Use it when passing callbacks to memoized child components."

---

### Q23. What is the `useMemo` hook?

**Answer**: `useMemo` memoizes the **result** of an expensive computation so it only recalculates when dependencies change. Syntax: `const value = useMemo(() => computeExpensive(a, b), [a, b])`. Don't overuse it — the memoization overhead can outweigh the benefit for cheap calculations.

**Memory Tip**: "`useMemo` = 'remember this VALUE.' `useCallback` = 'remember this FUNCTION.' Both take dependency arrays."

---

### Q24. What is the difference between `useMemo` and `useCallback`?

**Answer**: `useMemo` memoizes a computed value (result of a function call). `useCallback` memoizes the function itself (reference equality). `useCallback(fn, deps)` is equivalent to `useMemo(() => fn, deps)`. Use `useMemo` for expensive calculations; use `useCallback` for stable function references passed as props.

**Memory Tip**: "`useMemo` = cache the **result**. `useCallback` = cache the **function**. Same deps concept, different target."

---

### Q25. What are custom hooks?

**Answer**: Custom hooks are reusable functions that start with `use` and can call other hooks. They extract shared stateful logic from components into reusable modules. Example: `useFetch(url)` could encapsulate loading/error/data state. They follow the same rules as built-in hooks (top-level calls only).

**Memory Tip**: "Custom hooks = 'useYourImagination.' If two components share stateful logic, extract it into a `useSomething` function."

---

### Q26. What is the Context API?

**Answer**: Context provides a way to pass data through the component tree without prop drilling. Create with `createContext(default)`, provide with `<Provider value={...}>`, consume with `useContext()`. Best for global data like theme, auth, locale. Not ideal for high-frequency updates (causes broad re-renders).

**Memory Tip**: "Context = a global variable scoped to a subtree. Good for rarely-changing global data (theme, auth). Bad for frequently-changing data."

---

### Q27. What is the `useLayoutEffect` hook?

**Answer**: `useLayoutEffect` is identical to `useEffect` but fires **synchronously after all DOM mutations**, before the browser paints. Use it for DOM measurements or mutations that must happen before the user sees anything (e.g., scrolling to a position). For most side effects, `useEffect` is preferred (non-blocking).

**Memory Tip**: "`useLayoutEffect` = 'I need to change the DOM BEFORE the user sees it.' `useEffect` = 'after they see it, do cleanup work.'"

---

### Q28. What are React error boundaries?

**Answer**: Error boundaries are class components that catch JavaScript errors anywhere in their child component tree during rendering, lifecycle methods, and constructors. Define `static getDerivedStateFromError(error)` or `componentDidCatch(error, info)` to handle errors gracefully. Functional components cannot be error boundaries (as of React 18).

**Memory Tip**: "Error boundaries = try/catch for JSX. Must be class components. Wrap risky subtrees to prevent full app crashes."

---

### Q29. What is the Strict Mode component?

**Answer**: `<React.StrictMode>` activates additional checks and warnings during development. It intentionally double-invokes functions (constructor, render, effects) to help identify side effects. It has **no effect in production**. It helps catch: unsafe lifecycle methods, legacy string ref API, unexpected side effects.

**Memory Tip**: "StrictMode = a drill sergeant that runs everything twice in dev to catch sloppy code. No impact in production."

---

### Q30. What are higher-order components (HOCs)?

**Answer**: A HOC is a function that takes a component and returns a new enhanced component: `const Enhanced = withAuth(BaseComponent)`. They wrap components with additional behavior (auth checks, data fetching, styling). Pattern: `function withFeature(WrappedComponent) { return (props) => <WrappedComponent {...props} extra={...} />; }`.

**Memory Tip**: "HOC = a component factory. `withX(Component)` = 'wrap Component with X capability.' Like middleware for components."

---

### Q31. What are render props?

**Answer**: Render props is a pattern where a component receives a function as a prop that returns JSX: `<DataProvider render={(data) => <Display data={data} />} />`. The component calls this function to determine what to render. It's an alternative to HOCs for sharing logic between components.

**Memory Tip**: "Render props = 'you tell me what to render.' The parent provides a function that returns the UI."

---

### Q32. What is code splitting in React?

**Answer**: Code splitting lazy-loads parts of your app on demand, reducing the initial bundle size. React supports it via `React.lazy(() => import('./Component'))` combined with `<Suspense fallback={<Loading />}>`. Route-based splitting (loading components per route) is the most common pattern.

**Memory Tip**: "Code splitting = 'don't load everything upfront.' `React.lazy` + `Suspense` = load on demand."

---

### Q33. What is the `Suspense` component?

**Answer**: `<Suspense>` lets you wait for something to load before rendering. Wrap lazy-loaded components: `<Suspense fallback={<Spinner />}><LazyComponent /></Suspense>`. In React 18+, it also works with data fetching via libraries that support the Suspense API (React Query, Relay). Multiple Suspense boundaries allow progressive loading.

**Memory Tip**: "`Suspense` = 'hold on, it's loading.' Show a fallback until the child is ready."

---

### Q34. What are React portals?

**Answer**: Portals render children into a DOM node outside the parent component's DOM hierarchy: `createPortal(child, container)`. Common use case: modals, tooltips, and overlays that need to escape CSS overflow hidden or z-index stacking contexts. Events from portals bubble up through the React tree (not the DOM tree).

**Memory Tip**: "Portals = teleporting UI. Render in a different DOM location but keep React event bubbling intact."

---

### Q35. What is the difference between `useEffect` and `useLayoutEffect`?

**Answer**: `useEffect` runs asynchronously after the browser paints — user sees the render first, then the effect runs. `useLayoutEffect` runs synchronously after DOM mutations but before the browser paints — user never sees the intermediate state. Use `useLayoutEffect` for DOM measurements/mutations; use `useEffect` for everything else.

**Memory Tip**: "`useEffect` = after paint (async). `useLayoutEffect` = before paint (sync). Same API, different timing."

---

### Q36. What is the `useId` hook?

**Answer**: `useId` generates unique, stable IDs for accessibility attributes across server and client rendering. Example: `const id = useId()` → `:r1:`. It solves the hydration mismatch problem that occurs with random IDs or incrementing counters. Use for `htmlFor`, `aria-describedby`, and similar ARIA attributes.

**Memory Tip**: "`useId` = SSR-safe unique IDs. Never use `Math.random()` or counters for IDs in SSR apps."

---

### Q37. What are React server components (RSC)?

**Answer**: Server Components (React 18+) render exclusively on the server — they never ship JavaScript to the client. They can directly access databases, file systems, and environment variables. They reduce bundle size and improve initial load times. In Next.js App Router, components are server components by default.

**Memory Tip**: "RSC = 'this component never reaches the browser.' Server renders it, client receives only HTML."

---

### Q38. What is the `useTransition` hook?

**Answer**: `useTransition` marks a state update as non-urgent, keeping the UI responsive during expensive renders. It returns `[isPending, startTransition]`. Wrap heavy state updates in `startTransition` to avoid blocking user input. It's part of React's concurrent features.

**Memory Tip**: "`useTransition` = 'this update can wait.' Mark expensive renders as low priority so the UI stays snappy."

---

### Q39. What is the `useDeferredValue` hook?

**Answer**: `useDeferredValue` returns a deferred version of a value that lags behind the latest value during urgent updates. Useful for search inputs where you want to keep the input responsive while deferring the expensive filtered list rendering: `const deferredQuery = useDeferredValue(query)`.

**Memory Tip**: "`useDeferredValue` = 'show the old value while I catch up.' Like buffering — display stale data while new data renders."

---

### Q40. What are React concurrent features?

**Answer**: Concurrent features (React 18+) allow React to prepare multiple versions of the UI simultaneously. Key features: `startTransition`, `useTransition`, `useDeferredValue`, and Suspense integration. They enable interruptible rendering — urgent updates (typing) are not blocked by non-urgent updates (filtering).

**Memory Tip**: "Concurrent = 'multitasking React.' It can pause rendering to handle urgent work first."

---

### Q41. What is hydration in React?

**Answer**: Hydration is the process where React attaches event listeners to server-rendered HTML, making it interactive. The server sends static HTML; React "hydrates" it by reconciling the virtual DOM with the existing DOM. Mismatches between server and client HTML cause hydration errors and performance issues.

**Memory Tip**: "Hydration = 'bringing static HTML to life.' Server renders the skeleton, React adds the nervous system (events)."

---

### Q42. How do you prevent unnecessary re-renders in React?

**Answer**: Strategies include: `React.memo` for child components, `useMemo` for expensive values, `useCallback` for function references, moving state down to the smallest component that needs it, splitting context to avoid broad re-renders, and using `key` correctly. Profile with React DevTools Profiler before optimizing.

**Memory Tip**: "Prevent re-renders = memoize (`memo`, `useMemo`, `useCallback`) + colocate state + split context. Profile first, optimize second."

---

## SECTION C: ADVANCED (Q43–50)

---

### Q43. How does React's batching work? What about automatic batching in React 18?

**Answer**: React batches multiple state updates into a single re-render for performance. Before React 18, batching only worked inside React event handlers. React 18's automatic batching (`createRoot`) batches updates everywhere — promises, setTimeout, native event handlers, and even async functions.

**Memory Tip**: "Batching = 'collect all state changes, then render once.' React 18 does this everywhere, not just in event handlers."

---

### Q44. What is the fiber architecture in React?

**Answer**: Fiber is React's reimplementation of the reconciliation algorithm (React 16+). It enables incremental rendering — work can be split into chunks, paused, resumed, and discarded. Each fiber node represents a unit of work. This architecture enables concurrent features like `startTransition` and Suspense.

**Memory Tip**: "Fiber = React's task scheduler. It breaks rendering into small units of work that can be interrupted, enabling smooth UI."

---

### Q45. What is the `useSyncExternalStore` hook?

**Answer**: `useSyncExternalStore` subscribes to an external data store (Redux, Zustand, browser APIs) in a way that's consistent with concurrent rendering. It takes `subscribe`, `getSnapshot`, and optional `getServerSnapshot`. It prevents "tearing" — where different parts of the UI show different states.

**Memory Tip**: "`useSyncExternalStore` = the 'safe bridge' between React and non-React state. Required for concurrent-safe external subscriptions."

---

### Q46. What is the difference between `flushSync` and `startTransition`?

**Answer**: `flushSync` forces synchronous rendering of all pending updates — it blocks the browser and updates the DOM immediately (use sparingly). `startTransition` marks updates as non-urgent and interruptible — React can yield to more important work. They're opposites: `flushSync` = urgent/blocking, `startTransition` = non-urgent/interruptible.

**Memory Tip**: "`flushSync` = 'DO IT NOW!' (blocks everything). `startTransition` = 'do it when you can' (yields to urgent work)."

---

### Q47. How do you test React components?

**Answer**: Use **Jest** as the test runner and **React Testing Library (RTL)** for component testing. RTL renders components in a DOM-like environment and queries by role/text (not implementation details). Test user behavior, not implementation: "when user clicks button, X happens." Avoid `shallow` rendering — prefer full renders.

**Memory Tip**: "Test like a user: 'click button → see text.' RTL philosophy: test behavior, not hooks or state internals."

---

### Q48. What are the rules of hooks?

**Answer**: 1) Only call hooks at the **top level** of your component (no loops, conditions, or nested functions). 2) Only call hooks from **React function components** or **custom hooks**. These rules ensure hooks are called in the same order every render, which is how React tracks hook state internally.

**Memory Tip**: "Hooks rules: **Top level** only, **React functions** only. No `if` statements around hooks. No hooks in regular JS functions."

---

### Q49. What is the `useImperativeHandle` hook?

**Answer**: `useImperativeHandle` customizes what a ref exposes to parent components when using `forwardRef`. Instead of exposing the entire DOM node, you control which methods/properties are available: `useImperativeHandle(ref, () => ({ focus, scroll }))`. It's an escape hatch for imperative parent → child communication.

**Memory Tip**: "`useImperativeHandle` = 'parent, here's what you're allowed to call on me.' A controlled ref API."

---

### Q50. What is the difference between `forwardRef` and `useRef` for accessing child components?

**Answer**: `useRef` creates a ref for DOM elements or values within the same component. `forwardRef` passes a ref from a parent through to a child component's internal element. Without `forwardRef`, you can't ref a functional child component. Combined with `useImperativeHandle`, the child controls what the parent sees.

**Memory Tip**: "`useRef` = 'I need a ref for myself.' `forwardRef` = 'my parent wants to ref my internals.' Like forwarding mail."

---

## Quick Reference: Hooks Cheat Sheet

| Hook | Purpose | Common Use |
|---|---|---|
| `useState` | Local state | Form inputs, toggles, counters |
| `useEffect` | Side effects | Data fetching, subscriptions |
| `useContext` | Read context | Theme, auth, locale |
| `useRef` | Mutable ref | DOM access, previous values |
| `useReducer` | Complex state | Forms with many fields |
| `useCallback` | Memoize function | Stable callbacks for children |
| `useMemo` | Memoize value | Expensive computations |
| `useLayoutEffect` | Sync DOM effect | DOM measurements |
| `useId` | Unique IDs | ARIA attributes |
| `useTransition` | Non-urgent updates | Search filtering |
| `useDeferredValue` | Deferred rendering | Typeahead results |
| `useImperativeHandle` | Custom ref API | Exposing child methods |
| `useSyncExternalStore` | External store | Redux, browser APIs |

---

*Sources: [GreatFrontend](https://www.greatfrontend.com/blog/30-essential-react-hooks-interview-questions-you-must-know), [ShadeCoder](https://www.shadecoder.com/blogs/react-interview-questions-(2025)), [TheLinuxCode](https://thelinuxcode.com/react-hooks-interview-questions-answers-2025-deep-explanations-practical-patterns-and-pitfalls/), [GeeksforGeeks](https://www.geeksforgeeks.org/top-react-hooks-interview-questions-answers/)*