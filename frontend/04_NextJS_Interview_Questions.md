# Next.js Interview Questions — Top 50

> **Difficulty Split**: ~40% Basic (Q1–20), ~45% Intermediate (Q21–42), ~15% Advanced (Q43–50)
> **Focus**: SSR, SSG, routing, API routes, App Router — tailored for backend/full-stack engineers

---

## SECTION A: BASIC / FUNDAMENTAL (Q1–20)

---

### Q1. What is Next.js and why would you use it over plain React?

**Answer**: Next.js is a React **framework** that adds server-side rendering, file-based routing, API routes, image optimization, and a build pipeline on top of React. Plain React is a UI library — it only handles the view layer. Next.js gives you a full production stack out of the box: routing, multiple rendering strategies (SSR/SSG/ISR), and deployment-ready output.

**Memory Tip**: "React = engine. Next.js = the entire car. Routing, SSR, API routes, optimization — all built in."

---

### Q2. What is the difference between the Pages Router and the App Router?

**Answer**: The **Pages Router** (`/pages`) is the original system using `getStaticProps`/`getServerSideProps`. The **App Router** (`/app`, stable since v13.4) is built on React Server Components, uses `async/await` for data fetching, supports composable nested layouts, streaming, and Server Actions. The App Router is the default for new projects.

**Memory Tip**: "Pages Router = old school (`getStaticProps`). App Router = modern (`async` components, RSC). Interview in 2026? Expect App Router questions."

---

### Q3. How does file-based routing work in Next.js?

**Answer**: In the App Router, the folder structure under `/app` defines the URL structure. Every folder is a route segment. A `page.tsx` makes the route publicly accessible. Special files like `layout.tsx`, `loading.tsx`, `error.tsx`, and `not-found.tsx` activate automatically for their route segment.

**Memory Tip**: "File system = routing. `app/about/page.tsx` → `/about`. `app/blog/[slug]/page.tsx` → `/blog/my-post`. The folder tree IS the URL tree."

---

### Q4. What are dynamic routes in Next.js?

**Answer**: Folders wrapped in square brackets create dynamic segments: `[slug]` matches a single segment, `[...slug]` is a catch-all (matches `/a/b/c`), and `[[...slug]]` is an optional catch-all (also matches the base path). The param is passed to `page.tsx` as `params.slug`.

**Memory Tip**: "`[slug]` = one dynamic piece. `[...slug]` = everything after. `[[...slug]]` = everything after OR nothing."

---

### Q5. What is SSR (Server-Side Rendering) in Next.js?

**Answer**: SSR generates HTML on the server for each request. In the Pages Router, use `getServerSideProps`. In the App Router, use `fetch` with `cache: 'no-store'` or call dynamic functions like `cookies()`/`headers()`. SSR ensures content is always fresh and SEO-friendly.

**Memory Tip**: "SSR = 'generate HTML per request.' Fresh data every time. Best for personalized/auth-gated pages."

---

### Q6. What is SSG (Static Site Generation) in Next.js?

**Answer**: SSG generates HTML at **build time**. The page is compiled once and served from a CDN. In Pages Router: `getStaticProps`. In App Router: the default behavior — `fetch` without cache options caches indefinitely. Best for content that rarely changes (blogs, docs, marketing pages).

**Memory Tip**: "SSG = 'build once, serve forever.' Fastest possible load. Update content = rebuild or use ISR."

---

### Q7. What is ISR (Incremental Static Regeneration)?

**Answer**: ISR regenerates static pages **in the background** after a time interval without a full rebuild. Pages Router: `revalidate: 60` in `getStaticProps`. App Router: `fetch(url, { next: { revalidate: 60 } })`. Stale page is served until the new one is ready — no downtime.

**Memory Tip**: "ISR = 'static but auto-refreshing.' Like a self-updating cache. Stale-while-revalidate for pages."

---

### Q8. What are layouts in the App Router?

**Answer**: A `layout.tsx` file wraps all pages in its folder and subfolders. Layouts **persist across navigations** — they don't unmount when users move between child routes. Nested layouts stack: root `layout.tsx` wraps everything, child `layout.tsx` wraps only its segment via `{children}`.

**Memory Tip**: "Layouts = persistent wrappers. Root layout = always there. Nested layouts = Russian nesting dolls of UI shells."

---

### Q9. What are `loading.tsx` and `error.tsx` files?

**Answer**: `loading.tsx` wraps a route's page in a `<Suspense>` boundary — Next.js streams the layout immediately and shows the loading UI while data resolves. `error.tsx` is a React Error Boundary that catches runtime errors in its route segment. Error boundaries must be Client Components (`"use client"`).

**Memory Tip**: "`loading.tsx` = automatic spinner. `error.tsx` = automatic error boundary. Convention over configuration."

---

### Q10. What is the `public` folder in Next.js?

**Answer**: Files in `/public` are served as static assets at the root URL. `/public/logo.png` → `https://yoursite.com/logo.png`. Ideal for `robots.txt`, `sitemap.xml`, favicons, and images that don't need optimization. Next.js doesn't process or optimize these files.

**Memory Tip**: "`public` = the folder that maps directly to `/` on your website. Drop a file, access it at the root URL."

---

### Q11. How do environment variables work in Next.js?

**Answer**: Next.js auto-loads `.env.local`. Variables prefixed with `NEXT_PUBLIC_` are exposed to the browser bundle. All others remain server-only. **Never** put secrets in `NEXT_PUBLIC_` variables — they're baked into client JavaScript. Access via `process.env.VARIABLE_NAME`.

**Memory Tip**: "`NEXT_PUBLIC_` = browser-visible. Everything else = server-only. Like 'public' vs 'classified' folders."

---

### Q12. What is `next/image` and why should you use it?

**Answer**: `next/image` is a built-in `<Image>` component that lazy-loads images, serves modern formats (WebP, AVIF), resizes on-demand, and prevents Cumulative Layout Shift by requiring dimensions. Use `priority` on above-the-fold images for eager loading. It's a core performance optimization tool.

**Memory Tip**: "`next/image` = automatic image optimization. Lazy loading, modern formats, no CLS. Always use it instead of `<img>`."

---

### Q13. What is `next/link` and how does prefetching work?

**Answer**: `next/link` renders an `<a>` tag that navigates client-side without a full page reload. It **prefetches** the linked page's JavaScript when the link enters the viewport (production builds only). This makes navigation feel instant. Use `prefetch={false}` for low-traffic routes.

**Memory Tip**: "`next/link` = instant navigation. It pre-downloads the page before the user clicks. Like pre-loading a video."

---

### Q14. What is `next/font`?

**Answer**: `next/font` downloads Google Fonts (or local fonts) at build time and serves them self-hosted. It eliminates external network requests, applies `font-display: swap`, prevents layout shift, and inlines CSS — all with zero runtime JavaScript.

**Memory Tip**: "`next/font` = fonts without the Google CDN hit. Downloaded at build time, served from your own server."

---

### Q15. What is the `not-found.tsx` file?

**Answer**: `not-found.tsx` renders when `notFound()` is called from a route segment, or when no route matches a URL. `app/not-found.tsx` is the global 404 page. Segment-specific ones (e.g., `app/blog/not-found.tsx`) handle 404s within that segment. Call `notFound()` from `next/navigation`.

**Memory Tip**: "`not-found.tsx` = custom 404 page. Global at root, specific in segments. Call `notFound()` to trigger it."

---

### Q16. What is code splitting in Next.js?

**Answer**: Next.js automatically splits code at the route level — each page only loads its own JavaScript. Dynamic imports (`dynamic(() => import('./Component'))`) enable component-level splitting. This reduces initial bundle size and improves time-to-interactive.

**Memory Tip**: "Code splitting = automatic in Next.js. Each route = separate bundle. Dynamic imports = component-level splitting."

---

### Q17. What is the difference between SSR, SSG, ISR, and CSR?

**Answer**: **SSG** = build time (fastest, static). **ISR** = build time + background refresh (fresh within revalidation window). **SSR** = every request (always fresh, slower). **CSR** = in the browser (client-side only, no SEO). Choose based on data freshness needs and SEO requirements.

**Memory Tip**: "SSG = snapshot. ISR = auto-updating snapshot. SSR = live camera. CSR = webcam on user's device."

---

### Q18. What is `next/head` vs the App Router metadata API?

**Answer**: In Pages Router, `<Head>` from `next/head` adds `<title>`, `<meta>`, and other head elements. In App Router, use `metadata` export or `generateMetadata()` async function. The metadata API is more powerful — supports templates, dynamic generation, and Open Graph tags.

**Memory Tip**: "Pages Router = `<Head>` component. App Router = `export const metadata` or `generateMetadata()`. The latter is async and more powerful."

---

### Q19. How do you create an API route in Next.js?

**Answer**: **Pages Router**: Create a file in `pages/api/` — `pages/api/users.ts` becomes `/api/users`. Export a default handler `(req, res) => { ... }`. **App Router**: Create `app/api/users/route.ts` and export named HTTP methods: `export async function GET() { return Response.json(data) }`.

**Memory Tip**: "Pages Router: `pages/api/*.ts` with `(req, res)`. App Router: `app/api/*/route.ts` with `GET()`, `POST()`, etc."

---

### Q20. What is the `redirect` function in Next.js?

**Answer**: `redirect()` from `next/navigation` performs a server-side redirect (307 status). Use it in Server Components and Server Actions. For client-side navigation, use `useRouter().push()`. It throws internally (using React's error boundary mechanism), so code after it doesn't execute.

**Memory Tip**: "`redirect()` = server-side redirect. `useRouter().push()` = client-side navigation. Know the boundary."

---

## SECTION B: INTERMEDIATE (Q21–42)

---

### Q21. What are React Server Components (RSC)?

**Answer**: Server Components run **exclusively on the server** — they never ship JavaScript to the client. They can directly access databases, file systems, and secrets. They render to HTML and send the result to the browser. In the App Router, every component is a Server Component by default unless marked `"use client"`.

**Memory Tip**: "RSC = 'this code never reaches the browser.' Zero client JS. Direct DB access. Default in App Router."

---

### Q22. What is the difference between Server Components and Client Components?

**Answer**: Server Components run server-only (no hooks, no events, no browser APIs, zero JS bundle). Client Components (marked `"use client"`) run on both server and client (hooks, events, browser APIs). Server Components can import Client Components, but not vice versa (Client Components can't import Server Components directly — they pass them as children instead).

**Memory Tip**: "Server = 'no hooks, no events, no browser.' Client = 'hooks, events, browser OK.' Server can import Client, not the reverse."

---

### Q23. When should you use `"use client"`?

**Answer**: Add `"use client"` when a component needs: event handlers (`onClick`), React hooks (`useState`, `useEffect`), browser APIs (`window`, `localStorage`), or third-party libraries using any of these. Best practice: push `"use client"` to leaf components — keep as much of the tree as Server Components as possible.

**Memory Tip**: "`use client` = 'I need interactivity or browser APIs.' Push it DOWN to the smallest component that needs it."

---

### Q24. How does data fetching work in the App Router?

**Answer**: Server Components can be `async` — `await` data directly in the component body. No `getStaticProps`, no `useEffect`. Use `fetch` with `cache: 'force-cache'` (SSG, default), `next: { revalidate: N }` (ISR), or `cache: 'no-store'` (SSR). For client-side fetching, use SWR or React Query in Client Components.

**Memory Tip**: "App Router data fetching = `async` component + `await fetch()`. Cache option determines strategy: force-cache (SSG), revalidate (ISR), no-store (SSR)."

---

### Q25. What is `generateStaticParams`?

**Answer**: `generateStaticParams` replaces `getStaticPaths` in the App Router. It returns the list of param values to pre-render at build time for dynamic routes. Next.js builds one static page per returned object and falls back to on-demand rendering for unlisted slugs.

**Memory Tip**: "`generateStaticParams` = 'build these pages ahead of time.' Returns an array of param objects for dynamic routes."

---

### Q26. What is `generateMetadata`?

**Answer**: `generateMetadata` is an async function exported from `page.tsx` or `layout.tsx` that returns SEO metadata (title, description, OG tags). It replaces the `<Head>` component. It receives `params`, so dynamic routes can generate page-specific titles. It's the App Router's way to handle `<head>`.

**Memory Tip**: "`generateMetadata` = SEO metadata as data, not JSX. Async, dynamic, typed. The App Router way."

---

### Q27. What are Route Groups?

**Answer**: A folder wrapped in parentheses `(groupName)` creates a **Route Group** — it organizes routes without affecting the URL. Use it to apply different layouts to routes at the same URL depth, or to group features like `(marketing)` and `(app)` with separate layouts.

**Memory Tip**: "`(groupName)` = invisible folder. Organizes code but doesn't change the URL. Like a hidden label."

---

### Q28. What are Parallel Routes?

**Answer**: Parallel Routes render multiple pages simultaneously in the same layout using named slots (`@slotName` folders). Each slot has its own `loading.tsx`, `error.tsx`, and independent navigation. Use case: dashboards with `@analytics` and `@notifications` loading independently.

**Memory Tip**: "Parallel Routes = `@slot` folders. Multiple pages rendered at once in one layout. Each slot is independent."

---

### Q29. What are Intercepting Routes?

**Answer**: Intercepting Routes load a route within a different layout context. Classic example: clicking a photo in a feed opens it in a modal (intercepted), but navigating directly shows the full page. Use `(.)`, `(..)`, `(...)` prefixes to intercept at different depths.

**Memory Tip**: "Intercepting Routes = 'show this differently depending on where you came from.' Modal from feed, full page from URL."

---

### Q30. What are Server Actions?

**Answer**: Server Actions are async functions marked `"use server"` that run on the server when called from forms or Client Components. They handle mutations (create, update, delete) without writing API routes. Combine with `revalidatePath`/`revalidateTag` to bust cache after mutations.

**Memory Tip**: "Server Actions = 'server function called from client.' Like RPC — no API endpoint needed. `use server` + `revalidatePath`."

---

### Q31. What is `revalidatePath` vs `revalidateTag`?

**Answer**: `revalidatePath("/blog")` purges cache for a specific URL path. `revalidateTag("posts")` purges all `fetch` calls tagged with `"posts"` anywhere in the app. Use `revalidatePath` when you know exactly which page changed; use `revalidateTag` when multiple pages share the same underlying data.

**Memory Tip**: "`revalidatePath` = 'bust this page's cache.' `revalidateTag` = 'bust everything tagged with X.' Tag is more powerful but broader."

---

### Q32. What is Next.js Middleware?

**Answer**: Middleware is a function in `middleware.ts` at the project root that runs **before every matched request** on the Edge Runtime. It intercepts requests before they hit pages or API routes. Ideal for: auth checks, redirects, A/B testing, header injection, and geo-based routing.

**Memory Tip**: "Middleware = the bouncer. Runs BEFORE pages. Edge Runtime (no Node.js APIs). Keep it thin: auth, redirects, headers."

---

### Q33. How do you read cookies and headers in a Server Component?

**Answer**: Use `cookies()` and `headers()` from `next/headers`. These are **async** functions in Next.js 15+. Calling them opts the component out of static caching (makes it dynamic). Example: `const token = (await cookies()).get("auth")?.value`.

**Memory Tip**: "`cookies()` and `headers()` from `next/headers`. Async in Next.js 15+. Calling them = dynamic rendering."

---

### Q34. What is the App Router caching model?

**Answer**: Four caching layers: (1) **Request Memoization** — deduplicates identical `fetch` calls during one render. (2) **Data Cache** — stores `fetch` results indefinitely (or with revalidation). (3) **Full Route Cache** — caches static HTML + RSC payload. (4) **Router Cache** — client-side RSC payload cache (30s–5min session).

**Memory Tip**: "4 caches: Request Memo (per render), Data Cache (fetch results), Full Route Cache (static HTML), Router Cache (client-side). Know which one your bug lives in."

---

### Q35. What does `cache: 'force-cache'` vs `cache: 'no-store'` do?

**Answer**: `cache: 'force-cache'` (default) caches the `fetch` response indefinitely — equivalent to `getStaticProps`. `cache: 'no-store'` bypasses the Data Cache on every request — equivalent to `getServerSideProps`. `next: { revalidate: N }` is ISR.

**Memory Tip**: "`force-cache` = SSG (cached forever). `no-store` = SSR (never cached). `revalidate: N` = ISR (cached + auto-refresh)."

---

### Q36. What are Route Handlers in the App Router?

**Answer**: Route Handlers (`app/api/.../route.ts`) are the App Router equivalent of API Routes. They use standard `Request`/`Response` Web APIs (not Express-style `req, res`). Export named HTTP methods: `GET`, `POST`, `PUT`, `DELETE`. They run on Edge Runtime by default.

**Memory Tip**: "Route Handlers = REST endpoints in App Router. Named exports per method (`GET`, `POST`). Web API standard, not Express."

---

### Q37. How does Suspense work in the App Router?

**Answer**: `<Suspense>` boundaries let Next.js stream HTML progressively. Content outside the boundary sends immediately; content inside streams when data resolves. `loading.tsx` files are auto-wrapped in Suspense. Use explicit `<Suspense>` for granular control over individual async components.

**Memory Tip**: "Suspense = 'show this while waiting, show that when ready.' Next.js auto-creates them via `loading.tsx`."

---

### Q38. How do you handle authentication in the App Router?

**Answer**: Recommended pattern: validate session in **Middleware** for route protection, read session in **Server Components** for personalization, use **Server Actions** for login/logout mutations. Libraries like Auth.js (NextAuth) and Clerk integrate with this pattern.

**Memory Tip**: "Auth pattern: Middleware = bouncer (route protection). Server Component = reader (personalization). Server Action = mutator (login/logout)."

---

### Q39. What is `next/script` and when do you use it?

**Answer**: `next/script` loads third-party scripts with controlled strategies: `strategy="lazyOnload"` (idle), `strategy="afterInteractive"` (after hydration), `strategy="beforeInteractive"` (before hydration). Use it for analytics, chat widgets, and consent managers instead of raw `<script>` tags.

**Memory Tip**: "`next/script` = controlled script loading. `lazyOnload` = least urgent. `beforeInteractive` = most urgent."

---

### Q40. How do you deploy a Next.js application?

**Answer**: **Vercel** is the reference platform — zero config, automatic ISR, edge caching, image optimization, preview deployments per PR. **Self-hosted** via `next start` (Node.js server), Docker (use `output: "standalone"`), or adapters for AWS, Cloudflare. Self-hosting requires managing ISR, image optimization, and caching yourself.

**Memory Tip**: "Vercel = easiest deployment. Docker + standalone = self-hosted. `output: 'standalone'` in `next.config.js` for Docker."

---

### Q41. What is the `useRouter` hook in Next.js?

**Answer**: In App Router, import `useRouter` from `next/navigation` (NOT `next/router`). Methods: `router.push(href)` for client-side navigation, `router.refresh()` to re-fetch the current route's data, `router.back()` for browser back, `router.prefetch(href)` to preload a route. Only works in Client Components.

**Memory Tip**: "App Router: `useRouter` from `next/navigation`. Pages Router: `next/router`. Don't mix them."

---

### Q42. What is `useSearchParams` and why does it need Suspense?

**Answer**: `useSearchParams()` from `next/navigation` reads URL query parameters in Client Components. It must be wrapped in a `<Suspense>` boundary because it opts the component into client-side rendering. Without Suspense, the entire page falls back to client-side rendering during static generation.

**Memory Tip**: "`useSearchParams` = read URL params client-side. Must wrap in `<Suspense>` or the whole page becomes CSR."

---

## SECTION C: ADVANCED (Q43–50)

---

### Q43. What is Partial Prerendering (PPR)?

**Answer**: PPR (experimental, Next.js 14+) splits a single page into a **static shell** and **dynamic holes**. The static shell is pre-rendered at build time and served from CDN instantly. Dynamic sections (wrapped in Suspense) stream from the server. It's the convergence of SSG and SSR — no trade-off needed.

**Memory Tip**: "PPR = 'static shell + dynamic holes.' Best of both worlds: instant static + fresh dynamic. One page, two speeds."

---

### Q44. What is the `"use cache"` directive? (Next.js 15)

**Answer**: `"use cache"` is a Next.js 15 experimental directive that marks a function or component's output for caching. It's more granular than `fetch` cache — you can cache any async operation (DB queries, computations). Combine with `cacheTag()` for invalidation and `cacheLife()` for TTL control.

**Memory Tip**: "`use cache` = 'cache this function's output.' Beyond fetch — cache DB queries, computations. Tag + Life for control."

---

### Q45. How does streaming work in the App Router?

**Answer**: The App Router uses React's streaming architecture. When a Server Component suspends (waiting for data), Next.js streams the HTML progressively — sending the layout and resolved sections first, then streaming in slow sections as data resolves. This is powered by the underlying HTTP streaming protocol.

**Memory Tip**: "Streaming = 'send what's ready, stream the rest when it's done.' Progressive page loading without blocking."

---

### Q46. What is the difference between Edge Runtime and Node.js Runtime in Next.js?

**Answer**: Edge Runtime is lightweight, runs in V8 isolates globally (like Cloudflare Workers), has sub-millisecond cold starts but limited API access (no `fs`, no `crypto` module). Node.js Runtime has full API access but slower cold starts. Middleware runs on Edge by default. Route Handlers and Server Components can choose.

**Memory Tip**: "Edge = fast, limited, global. Node.js = full power, slower cold start. Middleware = Edge. API routes = choose."

---

### Q47. How do you optimize performance in a production Next.js app?

**Answer**: Top optimizations: (1) Use Server Components to reduce client JS bundle. (2) `next/image` with `priority` on LCP images. (3) `next/font` for zero CLS. (4) Route-level code splitting (automatic). (5) Suspense streaming for slow data. (6) `revalidate`/ISR over `cache: 'no-store'` wherever possible. (7) Push `"use client"` to leaf components.

**Memory Tip**: "Performance = Server Components + next/image + next/font + ISR + streaming + minimal client JS. Profile with Lighthouse."

---

### Q48. What is the `dynamic` export in a route segment?

**Answer**: `export const dynamic = 'force-dynamic'` forces a route to SSR on every request (equivalent to `getServerSideProps`). `export const dynamic = 'force-static'` forces static generation. `export const revalidate = 60` sets ISR at the route level. These are route segment config options.

**Memory Tip**: "`dynamic` = 'how should this route render?' `force-dynamic` = always SSR. `force-static` = always SSG. Route-level control."

---

### Q49. How do you handle internationalization (i18n) in Next.js?

**Answer**: Next.js has built-in i18n routing support. Configure locales in `next.config.js`. Use `next-intl` or `next-i18next` for translation management. In the App Router, use dynamic route segments (`/[lang]/about/page.tsx`) and `generateStaticParams` to pre-render all locale variants. Middleware can detect and redirect based on browser language.

**Memory Tip**: "i18n = dynamic `[lang]` segment + middleware for detection + `generateStaticParams` for pre-rendering locales."

---

### Q50. What is Turbopack and how does it improve the developer experience?

**Answer**: Turbopack is Next.js's new bundler (built in Rust), designed as a Webpack replacement. It offers significantly faster incremental builds — updates in milliseconds instead of seconds. It's the default for `next dev` in Next.js 15+. It handles TypeScript, JSX, and CSS out of the box with zero configuration.

**Memory Tip**: "Turbopack = Webpack's replacement. Rust-based, blindingly fast. Default in `next dev` for Next.js 15+."

---

## Quick Reference: App Router vs Pages Router

| Feature | Pages Router | App Router |
|---|---|---|
| Directory | `/pages` | `/app` |
| Default component | Client Component | Server Component |
| Data fetching | `getStaticProps` / `getServerSideProps` | `async` component + `fetch` |
| Layouts | `_app.js` (single, global) | `layout.tsx` (nested, composable) |
| API routes | `pages/api/*.ts` (`req, res`) | `app/api/*/route.ts` (`GET`, `POST`) |
| Streaming | Not supported | Built-in via Suspense |
| Server Actions | Not supported | Built-in |
| Dynamic params | `getStaticPaths` | `generateStaticParams` |
| Metadata | `<Head>` component | `metadata` / `generateMetadata()` |
| 404 pages | `pages/404.js` | `not-found.tsx` |
| Loading states | Manual | `loading.tsx` (auto-Suspense) |
| Error handling | `ErrorBoundary` class | `error.tsx` (auto-boundary) |

---

## Quick Reference: Rendering Strategy Decision Tree

```
Does the page need fresh data per request?
├── YES → Does it need SEO/crawlers to see content?
│   ├── YES → SSR (cache: 'no-store')
│   └── NO  → CSR (Client Component + SWR/React Query)
└── NO  → Is the data completely static?
    ├── YES → SSG (default, cached forever)
    └── NO  → ISR (next: { revalidate: N })
```

---

*Sources: [StackInterview](https://stackinterview.abhijeetkushwaha.dev/guides/nextjs-interview-questions-and-answers-2026), [DigiQt](https://digiqt.com/blog/nextjs-interview-questions/), [ProjectPractical](https://www.projectpractical.com/next-js-interview-questions-and-answers/), [Mavis Digital](https://mavisdigital.com/en/blog/nextjs-2026-advanced-interview-questions)*