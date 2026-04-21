# React Interview Questions — Top 50

> **Difficulty Split:** Basic (Q1–Q20) | Intermediate (Q21–Q40) | Advanced (Q41–Q50)

---

## Basic Questions (Q1–Q20)

### Q1. What is React?

**Answer:** React is a JavaScript library for building user interfaces using reusable components. It uses a virtual DOM for efficient updates, JSX for templating, and a unidirectional data flow. Created by Meta (Facebook).

**💡 Tip:** React = **Component-based UI library**. Not a framework — it focuses on the view layer. You add routing, state management, etc.

---

### Q2. What is JSX?

**Answer:** JSX is a syntax extension that lets you write HTML-like code in JavaScript: `const el = <h1>Hello</h1>`. Babel compiles JSX to `React.createElement()` calls. JSX is not HTML — it's syntactic sugar for JS function calls.

**💡 Tip:** JSX = **HTML in JavaScript** (not JavaScript in HTML). One element per return. Use `className` not `class`, `htmlFor` not `for`.

---

### Q3. What is the Virtual DOM?

**Answer:** Virtual DOM is a lightweight JavaScript representation of the real DOM. React compares (diffs) the old and new virtual DOM, calculates the minimum number of changes, and batches updates to the real DOM. This process is called **reconciliation**.

**💡 Tip:** Virtual DOM = **blueprint**. React compares blueprints, applies only the diffs to the real DOM. Much faster than rewriting everything.

---

### Q4. What are components in React?

**Answer:** Components are reusable UI building blocks. Two types:
- **Function components**: simple functions returning JSX (modern, with hooks)
- **Class components**: ES6 classes with `render()` method (legacy)

Modern React almost exclusively uses function components with hooks.

**💡 Tip:** Function components = **modern way**. Class components = **legacy**. Always prefer function components + hooks.

---

### Q5. What are props in React?

**Answer:** Props (properties) are read-only data passed from parent to child components. They flow **one direction** (top-down). Child components cannot modify props — they're immutable. Use callbacks in props to communicate upward.

**💡 Tip:** Props = **arguments for components**. One-way street: parent → child. To go upward, pass a callback function as a prop.

---

### Q6. What is `state` in React?

**Answer:** State is mutable data managed within a component. When state changes, React re-renders the component. Use `useState` hook in function components:
```jsx
const [count, setCount] = useState(0);
```

State is private to the component and persists across re-renders.

**💡 Tip:** State = **component's memory**. Change it → component re-renders. Props = external data. State = internal data.

---

### Q7. What is the `useState` hook?

**Answer:** `useState` adds state to function components. Returns `[currentValue, setterFunction]`:
```jsx
const [name, setName] = useState("Ali");
// Update: setName("Ahmed") or setName(prev => prev + " Khan")
```

Initial value is only used on first render. Use callback form for updates based on previous state.

**💡 Tip:** Always use the **callback form** (`setCount(prev => prev + 1)`) when new state depends on previous state. Prevents stale closure bugs.

---

### Q8. What is the `useEffect` hook?

**Answer:** `useEffect` runs side effects after render (API calls, subscriptions, DOM manipulation):
```jsx
useEffect(() => {
  fetchData();
  return () => cleanup(); // cleanup function
}, [dependency]); // dependency array
```

Dependency array: `[]` = run once, `[dep]` = run when dep changes, no array = run every render.

**💡 Tip:** `useEffect` = **"do something after render."** `[]` = mount only. `[x]` = when x changes. No deps = every render (rarely wanted). Always clean up subscriptions.

---

### Q9. What is the component lifecycle in React?

**Answer:** Three phases:
1. **Mounting**: component added to DOM (`useEffect(() => {}, [])`)
2. **Updating**: state/props change, re-render (`useEffect(() => {}, [dep])`)
3. **Unmounting**: component removed from DOM (cleanup function in useEffect)

Class components had explicit methods: `componentDidMount`, `componentDidUpdate`, `componentWillUnmount`.

**💡 Tip:** In hooks: Mount = `useEffect(fn, [])`. Update = `useEffect(fn, [deps])`. Unmount = `return () => cleanup` inside useEffect.

---

### Q10. What is the difference between controlled and uncontrolled components?

**Answer:**
- **Controlled**: React state controls the input value. `<input value={name} onChange={e => setName(e.target.value)} />`
- **Uncontrolled**: DOM controls the value. Access via `useRef`: `inputRef.current.value`

Controlled = React is the source of truth. Uncontrolled = DOM is the source.

**💡 Tip:** Controlled = **React drives** (state = value). Uncontrolled = **DOM drives** (ref to get value). Prefer controlled for most forms.

---

### Q11. What are keys in React lists?

**Answer:** Keys help React identify which list items changed. Use a **unique, stable** identifier: `items.map(item => <li key={item.id}>{item.name}</li>)`. Don't use array index as key (causes bugs with reordering/insertion).

**💡 Tip:** Keys = **name tags for list items**. Use unique IDs, never index. Wrong keys = weird re-render bugs and performance issues.

---

### Q12. What is conditional rendering in React?

**Answer:** Render different UI based on conditions:
```jsx
{isLoggedIn ? <Dashboard /> : <Login />}
{items.length > 0 && <ItemList />}
```

Use ternary for either/or, `&&` for show/hide, or extract to helper functions for complex logic.

**💡 Tip:** Ternary (`? :`) = **either A or B**. `&&` = **show or hide**. Don't use `if` inside JSX — use it above the return.

---

### Q13. What is React Fragment?

**Answer:** `<Fragment>` or `<>...</>` groups elements without adding extra DOM nodes:
```jsx
return (
  <>
    <td>Cell 1</td>
    <td>Cell 2</td>
  </>
);
```

Needed because JSX requires a single parent element, but sometimes you don't want a wrapper div.

**💡 Tip:** Fragment = **invisible wrapper**. Use `<>` when a parent element would break layout (like `<tr>` containing `<td>`s).

---

### Q14. What is the `useRef` hook?

**Answer:** `useRef` creates a mutable reference that persists across renders without causing re-renders:
```jsx
const inputRef = useRef(null);
<input ref={inputRef} />
// Access DOM: inputRef.current.focus()
// Store mutable value: inputRef.current = "any value"
```

Two uses: (1) access DOM elements, (2) store mutable values that don't trigger re-render.

**💡 Tip:** `useRef` = **"remember this, but don't re-render on change."** Use for DOM access, timers, previous values. NOT for state.

---

### Q15. What is prop drilling?

**Answer:** Prop drilling is passing props through multiple intermediate components that don't use them, just to reach a deeply nested child:
```
<App theme="dark"> → <Layout theme="dark"> → <Sidebar theme="dark"> → <Button theme="dark">
```

Solution: Context API, state management libraries (Redux, Zustand), or component composition.

**💡 Tip:** Prop drilling = **passing the baton through people who don't need it**. If 3+ intermediate components just forward a prop, use Context instead.

---

### Q16. What is the Context API?

**Answer:** Context provides a way to share values across the component tree without prop drilling:
```jsx
const ThemeContext = createContext("light");
<ThemeContext.Provider value="dark">
  <App />
</ThemeContext.Provider>
// Consumer: const theme = useContext(ThemeContext);
```

Use for: theme, auth, locale, global settings. Don't overuse — not a replacement for all state management.

**💡 Tip:** Context = **broadcasting** — one provider, many consumers anywhere below. Use for truly global data (theme, auth). Not for high-frequency updates.

---

### Q17. What are React events?

**Answer:** React uses synthetic events (cross-browser wrapper around native events):
```jsx
<button onClick={handleClick}>Click</button>
<input onChange={e => setValue(e.target.value)} />
<form onSubmit={e => { e.preventDefault(); }}>...</form>
```

Named with camelCase (`onClick`, not `onclick`). Pass function reference, not function call.

**💡 Tip:** React events = **camelCase** (`onClick`), pass **function** not call (`{fn}` not `{fn()}`). `e.preventDefault()` replaces `return false`.

---

### Q18. What is the `useMemo` hook?

**Answer:** `useMemo` memoizes expensive calculations, only recomputing when dependencies change:
```jsx
const sortedItems = useMemo(() => {
  return items.sort((a, b) => a.name.localeCompare(b.name));
}, [items]);
```

Prevents recalculating on every render when the result hasn't changed.

**💡 Tip:** `useMemo` = **"cache this calculation."** Use for expensive operations, not simple ones. Overusing it hurts more than helps.

---

### Q19. What is the `useCallback` hook?

**Answer:** `useCallback` memoizes a function reference:
```jsx
const handleClick = useCallback(() => {
  setCount(c => c + 1);
}, []);
```

Prevents function recreation on every render. Useful when passing callbacks to memoized child components.

**💡 Tip:** `useCallback` = **"don't recreate this function."** Only useful when the function is a dependency of something else (useEffect, memoized children).

---

### Q20. What is `React.memo`?

**Answer:** `React.memo` is a higher-order component that prevents re-rendering if props haven't changed:
```jsx
const Button = React.memo(({ label, onClick }) => {
  return <button onClick={onClick}>{label}</button>;
});
```

Does a shallow comparison of props. Use for components that render often with the same props.

**💡 Tip:** `React.memo` = **"skip re-render if props unchanged."** Only use when profiling shows unnecessary re-renders. Don't memo everything.

---

## Intermediate Questions (Q21–Q40)

### Q21. What is the difference between `useEffect` and `useLayoutEffect`?

**Answer:**
- `useEffect`: runs **after** paint (asynchronous) — non-blocking, good for most side effects
- `useLayoutEffect`: runs **before** paint (synchronous) — blocks visual update, use for DOM measurements/modifications that prevent flicker

**💡 Tip:** Use `useEffect` 99% of the time. Use `useLayoutEffect` only when you need to modify DOM before the user sees it (prevent flicker).

---

### Q22. What is reconciliation in React?

**Answer:** Reconciliation is React's algorithm for diffing the virtual DOM tree and determining the minimum DOM updates needed. It uses these rules:
1. Different element types → rebuild the tree
2. Same element type → update props/attributes
3. Key prop → match children across renders

**💡 Tip:** Reconciliation = **"what changed?"** Different type = nuke and rebuild. Same type = update in place. Keys = identity for list items.

---

### Q23. What are Higher-Order Components (HOC)?

**Answer:** An HOC is a function that takes a component and returns an enhanced component:
```jsx
const withAuth = (Component) => {
  return (props) => {
    if (!isLoggedIn()) return <Login />;
    return <Component {...props} />;
  };
};
const ProtectedPage = withAuth(Dashboard);
```

Pattern for cross-cutting concerns (auth, logging, data fetching). Less common with hooks.

**💡 Tip:** HOC = **wrapper component** — takes a component, returns a better one. Hooks replaced most HOC use cases. Still useful for complex cross-cutting logic.

---

### Q24. What are render props?

**Answer:** A pattern where a component receives a function as a prop that returns JSX:
```jsx
<DataProvider render={(data) => <List items={data} />} />
// Or children pattern:
<DataProvider>{(data) => <List items={data} />}</DataProvider>
```

Shares code between components without inheritance. Largely replaced by hooks.

**💡 Tip:** Render props = **"you tell me how to render, I give you the data."** Hooks replaced this pattern, but you'll see it in older codebases.

---

### Q25. What is the `useReducer` hook?

**Answer:** Alternative to `useState` for complex state logic:
```jsx
const [state, dispatch] = useReducer(reducer, initialState);
function reducer(state, action) {
  switch (action.type) {
    case "INCREMENT": return { count: state.count + 1 };
    default: return state;
  }
}
```

Best for: multiple related values, complex state transitions, next state depends on previous.

**💡 Tip:** `useReducer` = **Redux-lite**. Use when `useState` gets messy (multiple interdependent state values). dispatch → reducer → new state.

---

### Q26. What is React error boundary?

**Answer:** Error boundaries catch JavaScript errors in child components, log them, and display a fallback UI:
```jsx
class ErrorBoundary extends React.Component {
  state = { hasError: false };
  static getDerivedStateFromError(error) { return { hasError: true }; }
  render() {
    return this.state.hasError ? <Fallback /> : this.props.children;
  }
}
```

**Only class components** can be error boundaries. Functional alternative: `react-error-boundary` library.

**💡 Tip:** Error boundary = **try/catch for JSX**. Wraps components to prevent one crash from breaking the whole app. Must be a class component.

---

### Q27. What are React portals?

**Answer:** Portals render children into a DOM node outside the parent component's DOM hierarchy:
```jsx
ReactDOM.createPortal(<Modal />, document.getElementById("modal-root"));
```

Common use case: modals, tooltips, overlays — elements that visually break out of their container but stay in the React tree.

**💡 Tip:** Portal = **"render here but show there."** Modal lives in React tree (events bubble normally) but appears in a different DOM location.

---

### Q28. What is the `useId` hook?

**Answer:** `useId` generates unique, stable IDs for accessibility attributes:
```jsx
const id = useId();
<label htmlFor={id}>Name</label>
<input id={id} />
```

IDs are unique across the app and consistent between server and client rendering. Don't use for list keys.

**💡 Tip:** `useId` = **unique IDs for accessibility** (label-input pairs). SSR-safe. Not for list keys or database IDs.

---

### Q29. What are custom hooks?

**Answer:** Custom hooks extract reusable logic into functions starting with `use`:
```jsx
function useLocalStorage(key, initialValue) {
  const [value, setValue] = useState(() => {
    const stored = localStorage.getItem(key);
    return stored ? JSON.parse(stored) : initialValue;
  });
  useEffect(() => { localStorage.setItem(key, JSON.stringify(value)); }, [key, value]);
  return [value, setValue];
}
```

**💡 Tip:** Custom hooks = **reusable stateful logic**. Must start with `use`. Share behavior, not UI. Replace HOCs and render props.

---

### Q30. What is `StrictMode` in React?

**Answer:** `<React.StrictMode>` highlights potential problems during development:
- Detects unsafe lifecycle methods
- Warns about deprecated APIs
- Detects unexpected side effects (invokes functions twice in dev)

Has NO effect in production. Wraps your app or parts of it.

**💡 Tip:** StrictMode = **development safety net**. Double-invokes render/effects in dev to catch bugs. No impact on production.

---

### Q31. What are Suspense and lazy loading?

**Answer:** `React.lazy` + `Suspense` enable code splitting:
```jsx
const HeavyComponent = React.lazy(() => import("./HeavyComponent"));
<Suspense fallback={<Loading />}>
  <HeavyComponent />
</Suspense>
```

Loads components only when needed, reducing initial bundle size. `fallback` shows while loading.

**💡 Tip:** `lazy` + `Suspense` = **load on demand**. Split large components/routes. Always provide a `fallback` (spinner, skeleton).

---

### Q32. What is the React Fiber architecture?

**Answer:** Fiber is React's reconciliation engine (rewritten in React 16). It enables:
- **Incremental rendering**: splits work into chunks, can pause/resume
- **Priority-based updates**: user input > data fetching > animations
- **Concurrent features**: `startTransition`, `useDeferredValue`

**💡 Tip:** Fiber = React's **new engine** (since v16). Breaks work into units, can pause for urgent updates. Foundation for concurrent React.

---

### Q33. What is `startTransition`?

**Answer:** `startTransition` marks a state update as low-priority (non-urgent):
```jsx
const [search, setSearch] = useState("");
const handleChange = (e) => {
  setSearch(e.target.value); // urgent — updates input
  startTransition(() => {
    setFilter(e.target.value); // non-urgent — filters list
  });
};
```

Input stays responsive while expensive filtering happens in the background.

**💡 Tip:** `startTransition` = **"this update can wait."** Urgent (typing) vs non-urgent (filtering). Keeps UI responsive.

---

### Q34. What is `useDeferredValue`?

**Answer:** Returns a deferred version of a value that lags behind urgent updates:
```jsx
const [search, setSearch] = useState("");
const deferredSearch = useDeferredValue(search);
// Use deferredSearch for expensive filtering
```

Similar to `startTransition` but for values. The deferred value updates when the browser is idle.

**💡 Tip:** `useDeferredValue` = **"use the old value until you're free."** Like debouncing but based on browser availability, not time.

---

### Q35. What are server components (RSC)?

**Answer:** React Server Components run on the server and send rendered HTML to the client. They:
- Can access databases and file systems directly
- Add **zero JavaScript** to the bundle
- Can be async (use `async/await` directly)
- Client components: `"use client"` directive

**💡 Tip:** Server components = **zero-JS backend rendering**. `async` component = fetch data directly. `"use client"` = opt into client-side rendering.

---

### Q36. What is the `useTransition` hook?

**Answer:** `useTransition` returns `[isPending, startTransition]` to mark updates as transitions:
```jsx
const [isPending, startTransition] = useTransition();
startTransition(() => { setResults(heavyFilter(query)); });
// isPending = true while transition is processing
```

Shows loading state for non-urgent updates without blocking user input.

**💡 Tip:** `useTransition` = **non-urgent update + loading indicator.** `isPending` tells you if the transition is still processing.

---

### Q37. What is the `useSyncExternalStore` hook?

**Answer:** `useSyncExternalStore` subscribes to external stores (Redux, Zustand, browser APIs) in a way that's compatible with concurrent rendering:
```jsx
const snapshot = useSyncExternalStore(
  subscribe,     // (callback) => unsubscribe
  getSnapshot,   // () => current value
  getServerSnapshot
);
```

Prevents "tearing" (inconsistent UI) in concurrent mode.

**💡 Tip:** `useSyncExternalStore` = **safe external data bridge**. Required when connecting non-React state to React components in concurrent mode.

---

### Q38. What is `flushSync`?

**Answer:** `flushSync` forces React to update the DOM synchronously:
```jsx
import { flushSync } from "react-dom";
flushSync(() => { setCount(c => c + 1); });
// DOM is fully updated after this line
```

Use sparingly — defeats React's batching. Needed for: measuring DOM after update, integrating with non-React code.

**💡 Tip:** `flushSync` = **"update NOW, no batching."** Escape hatch. Use only when you absolutely need synchronous DOM updates.

---

### Q39. What are refs and forwardRef?

**Answer:** `forwardRef` allows parent components to pass a ref to child components:
```jsx
const Input = React.forwardRef((props, ref) => (
  <input ref={ref} {...props} />
));
// Parent: <Input ref={inputRef} />
```

Needed when you want to expose a child's DOM node or method to the parent.

**💡 Tip:** `forwardRef` = **"pass the ref through."** Without it, `ref` is just another prop. Used for input focus, scroll position, imperative handles.

---

### Q40. What is `useImperativeHandle`?

**Answer:** Customizes the instance value exposed to parent via `ref`:
```jsx
useImperativeHandle(ref, () => ({
  focus: () => inputRef.current.focus(),
  scroll: () => inputRef.current.scrollIntoView(),
}));
```

Controls what the parent can access — not the entire DOM node, only specific methods.

**💡 Tip:** `useImperativeHandle` = **"here's what I'll let the parent do."** Encapsulates the ref. Expose only needed methods, not the whole DOM node.

---

## Advanced Questions (Q41–Q50)

### Q41. How does React's batching work?

**Answer:** React batches multiple state updates into a single re-render for performance:
```jsx
// React 18+: ALL updates are batched (even inside promises, timeouts)
setCount(1); setName("Ali"); setActive(true); // One re-render, not three
```

Before React 18, only event handler updates were batched. React 18 batches everywhere with automatic batching.

**💡 Tip:** React 18 = **automatic batching everywhere**. Multiple `setState` calls = one render. Use `flushSync` to opt out if needed.

---

### Q42. What is the difference between React and React DOM?

**Answer:**
- `react`: core library — components, hooks, reconciliation algorithm (platform-agnostic)
- `react-dom`: DOM-specific code — renders to browser DOM, handles events, `createRoot`, `hydrate`

React core is platform-agnostic. `react-dom` is the browser adapter. `react-native` is the mobile adapter.

**💡 Tip:** `react` = **brain** (logic). `react-dom` = **hands** (DOM manipulation). Same brain, different hands for web vs native.

---

### Q43. What is hydration?

**Answer:** Hydration connects React's virtual DOM to server-rendered HTML. React "adopts" the existing DOM instead of recreating it:
```jsx
// Client
hydrateRoot(document.getElementById("root"), <App />);
```

If server and client HTML mismatch, React warns and re-renders. Critical for SSR performance.

**💡 Tip:** Hydration = **"wake up the server HTML with JavaScript."** Server sends static HTML, React attaches event listeners and makes it interactive.

---

### Q44. What is the React compiler (React Forget)?

**Answer:** React Compiler automatically adds memoization. You write normal code, the compiler figures out what needs memoization:
- Auto-memoizes values and callbacks
- Eliminates manual `useMemo`, `useCallback`, `React.memo`
- Currently experimental (Babel plugin)

**💡 Tip:** React Compiler = **automatic optimization**. Write simple code, compiler adds memoization. No more manual `useMemo`/`useCallback`.

---

### Q45. What are React actions (Server Actions)?

**Answer:** Server Actions allow server-side mutations directly from client components:
```jsx
async function handleSubmit(formData) {
  "use server";
  await db.users.create({ name: formData.get("name") });
}
<form action={handleSubmit}>
  <input name="name" />
  <button>Submit</button>
</form>
```

Work with forms natively, no API routes needed. Progressive enhancement compatible.

**💡 Tip:** Server Actions = **form actions that run on the server**. No fetch, no API route. `"use server"` = runs on server. Works without JavaScript!

---

### Q46. What are compound components pattern?

**Answer:** Components that implicitly share state through context:
```jsx
<Tabs>
  <Tabs.List>
    <Tabs.Tab>Tab 1</Tabs.Tab>
  </Tabs.List>
  <Tabs.Panels>
    <Tabs.Panel>Content 1</Tabs.Panel>
  </Tabs.Panels>
</Tabs>
```

Parent manages state, children consume it via context. Clean API, flexible composition.

**💡 Tip:** Compound components = **implicit state sharing**. Like `<select>` + `<option>` in HTML. Children know about parent state without props.

---

### Q47. What is the difference between uncontrolled and controlled forms?

**Answer:** In practice for real apps:
- **Controlled**: every input change updates React state. Full control over validation, formatting, submission. More code but predictable.
- **Uncontrolled**: use `useRef` to get values on submit. Less code, less control. Good for simple forms, integrating with non-React libraries.

**💡 Tip:** Use controlled for **complex forms** (validation, formatting). Uncontrolled for **simple forms** or non-React integrations. Libraries like React Hook Form bridge both.

---

### Q48. What is `useFormStatus` hook?

**Answer:** `useFormStatus` gives pending state of a parent form (React 19):
```jsx
function SubmitButton() {
  const { pending } = useFormStatus();
  return <button disabled={pending}>{pending ? "Submitting..." : "Submit"}</button>;
}
```

Must be used inside a `<form>`. Gives access to form's pending state, action, method, data.

**💡 Tip:** `useFormStatus` = **"is the form submitting?"** Must be a child of `<form>`. No need to pass loading state as props.

---

### Q49. What is `useOptimistic` hook?

**Answer:** `useOptimistic` shows an optimistic UI update while an async operation is in progress:
```jsx
const [optimisticName, setOptimisticName] = useOptimistic(name);
const updateName = async (newName) => {
  setOptimisticName(newName); // instant UI update
  await updateOnServer(newName); // real update
};
```

Reverts automatically if the operation fails. Improves perceived performance.

**💡 Tip:** `useOptimistic` = **"assume it works, revert if it doesn't."** Like a restaurant showing your order before kitchen confirms.

---

### Q50. What is the `use` hook (React 19)?

**Answer:** `use` reads resources (promises, context) during render:
```jsx
function Component({ promise }) {
  const data = use(promise); // suspends until resolved
  const theme = use(ThemeContext); // reads context
}
```

Can be called conditionally (unlike other hooks). Integrates with Suspense for loading states.

**💡 Tip:** `use` = **read async values during render**. Only hook that can be called in conditions/loops. Works with promises and context.

---
