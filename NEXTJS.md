# Next.js Interview Questions — Top 50

> **Difficulty Split:** Basic (Q1–Q20) | Intermediate (Q21–Q40) | Advanced (Q41–Q50)

---

## Basic Questions (Q1–Q20)

### Q1. What is Next.js?

**Answer:** Next.js is a React framework by Vercel that provides server-side rendering (SSR), static site generation (SSG), API routes, file-based routing, and built-in optimizations. It handles what React doesn't: routing, SSR, bundling, and deployment.

**💡 Tip:** React = **library** (build UI). Next.js = **framework** (full production app). Next.js adds the "everything else" React doesn't include.

---

### Q2. What is the difference between SSR, SSG, ISR, and CSR?

**Answer:**
- **SSR** (Server-Side Rendering): HTML generated on each request (dynamic, fresh data)
- **SSG** (Static Site Generation): HTML generated at build time (fast, cached)
- **ISR** (Incremental Static Regeneration): static + revalidates at intervals (`revalidate: 60`)
- **CSR** (Client-Side Rendering): browser renders with JavaScript (SPA-like)

**💡 Tip:** SSR = **per request**. SSG = **at build**. ISR = **static + periodic refresh**. CSR = **browser renders**. Pick based on data freshness needs.

---

### Q3. What is the App Router vs Pages Router?

**Answer:**
- **Pages Router** (`pages/`): older, `getServerSideProps`, `getStaticProps`, file-based routing
- **App Router** (`app/`): newer (Next.js 13+), React Server Components, nested layouts, streaming, `loading.tsx`, `error.tsx`

App Router is the recommended approach. Both can coexist.

**💡 Tip:** App Router = **modern** (Server Components, layouts). Pages Router = **legacy** (still supported). Start new projects with App Router.

---

### Q4. What are React Server Components (RSC) in Next.js?

**Answer:** Components that render **only on the server** — never ship JavaScript to the client. They can directly access databases, file systems, and use `async/await`. Marked as server components by default in App Router. Use `"use client"` to opt into client rendering.

**💡 Tip:** In App Router, **all components are server components by default**. Add `"use client"` only when you need interactivity (hooks, event handlers, browser APIs).

---

### Q5. What is `"use client"` directive?

**Answer:** `"use client"` at the top of a file marks it as a Client Component. Needed when the component uses: `useState`, `useEffect`, event handlers (`onClick`), browser APIs (`window`, `document`), or React class components.

**💡 Tip:** `"use client"` = **"this needs the browser."** Default = server. Only opt in when you need interactivity or hooks.

---

### Q6. What is file-based routing in Next.js?

**Answer:** The file system defines routes:
- `app/page.tsx` → `/`
- `app/about/page.tsx` → `/about`
- `app/blog/[slug]/page.tsx` → `/blog/hello-world` (dynamic)
- `app/shop/[...slug]/page.tsx` → `/shop/a/b/c` (catch-all)

No need for a router library — the folder structure IS the routing.

**💡 Tip:** `page.tsx` = **route**. `layout.tsx` = **wrapper**. `[param]` = **dynamic segment**. `[...slug]` = **catch-all**.

---

### Q7. What are layouts in Next.js?

**Answer:** `layout.tsx` wraps pages and persists across navigation (no re-render):
```tsx
// app/layout.tsx
export default function RootLayout({ children }) {
  return <html><body>{children}</body></html>;
}
```

Nested layouts: each folder can have its own `layout.tsx`. They nest — child layout inside parent layout.

**💡 Tip:** Layout = **persistent wrapper**. Doesn't re-render on navigation. Perfect for navbars, sidebars, footers. Nest as deep as needed.

---

### Q8. How do you fetch data in Next.js App Router?

**Answer:** Fetch directly in Server Components with `async/await`:
```tsx
async function Page() {
  const data = await fetch("https://api.example.com/data", { cache: "no-store" });
  const posts = await data.json();
  return <PostList posts={posts} />;
}
```

`cache: "no-store"` = SSR. `next: { revalidate: 60 }` = ISR. No fetch = SSG.

**💡 Tip:** In Server Components, just `fetch()` directly. No `useEffect`, no `getServerSideProps`. Set caching strategy via fetch options.

---

### Q9. What are dynamic routes in Next.js?

**Answer:** Use `[param]` for dynamic segments:
```tsx
// app/blog/[slug]/page.tsx
export default function BlogPost({ params }: { params: { slug: string } }) {
  return <h1>Post: {params.slug}</h1>;
}
```

Multiple segments: `[...slug]` catches all remaining path segments as an array.

**💡 Tip:** `[slug]` = **one dynamic segment**. `[...slug]` = **catch-all** (like `/a/b/c`). Access via `params.slug`.

---

### Q10. What is `next/image`?

**Answer:** Next.js Image component with built-in optimization:
- Automatic lazy loading
- Responsive images (srcset)
- Modern formats (WebP, AVIF)
- Prevents Cumulative Layout Shift (CLS) with width/height

```tsx
<Image src="/photo.jpg" width={500} height={300} alt="Photo" />
```

**💡 Tip:** Always use `next/image` instead of `<img>`. Free optimization. Must provide `width`/`height` or `fill` prop to prevent layout shift.

---

### Q11. What is `next/link`?

**Answer:** Next.js Link component for client-side navigation (no full page reload):
```tsx
<Link href="/about">About</Link>
<Link href={`/blog/${slug}`}>Post</Link>
```

Enables prefetching (loads page data on hover). Uses `pushState` under the hood.

**💡 Tip:** Always use `<Link>` for internal navigation, not `<a>`. Enables instant transitions and prefetching.

---

### Q12. What are API routes / Route Handlers?

**Answer:** Server-side API endpoints:
- **Pages Router**: `pages/api/hello.ts` → `/api/hello`
- **App Router**: `app/api/route.ts` → exports `GET`, `POST`, `PUT`, `DELETE`

```tsx
// app/api/users/route.ts
export async function GET() {
  const users = await db.users.findMany();
  return Response.json(users);
}
```

**💡 Tip:** API routes = **backend in your Next.js app**. App Router uses `route.ts` with named exports (`GET`, `POST`). Pages Router uses `pages/api/`.

---

### Q13. What is `next/font`?

**Answer:** Built-in font optimization that self-hosts Google fonts (no external network request):
```tsx
import { Inter } from "next/font/google";
const inter = Inter({ subsets: ["latin"] });
<body className={inter.className}>
```

Eliminates layout shift from font loading. Zero JavaScript added.

**💡 Tip:** `next/font` = **self-hosted, optimized fonts**. No FOUT (flash of unstyled text). Automatic subsetting. Always use it instead of Google Fonts CDN.

---

### Q14. What are loading.tsx and error.tsx?

**Answer:** Special files for UI states:
- `loading.tsx`: shows while page data loads (uses Suspense). Shimmer/skeleton UI.
- `error.tsx`: shows when page throws an error (error boundary). Must be client component.

```tsx
// app/dashboard/loading.tsx
export default function Loading() { return <DashboardSkeleton />; }

// app/dashboard/error.tsx
"use client";
export default function Error({ error, reset }) { return <ErrorDisplay />; }
```

**💡 Tip:** `loading.tsx` = **free skeleton UI**. `error.tsx` = **free error boundary**. Place them next to your `page.tsx` in the same folder.

---

### Q15. What is middleware in Next.js?

**Answer:** Middleware runs **before** a request completes — for auth, redirects, rewriting:
```tsx
// middleware.ts (root)
export function middleware(request) {
  if (!isAuthenticated(request)) {
    return NextResponse.redirect(new URL("/login", request.url));
  }
}
export const config = { matcher: ["/dashboard/:path*"] };
```

Runs on the Edge (fast, globally distributed). Can't access Node.js APIs.

**💡 Tip:** Middleware = **bouncer at the door**. Checks requests before they reach your page. Perfect for auth, geo-redirects, A/B testing.

---

### Q16. What is `generateStaticParams`?

**Answer:** Generates static pages for dynamic routes at build time:
```tsx
export async function generateStaticParams() {
  const posts = await getPosts();
  return posts.map(post => ({ slug: post.slug }));
}
```

Used with dynamic routes (`[slug]/page.tsx`) to pre-render specific paths. Enables SSG for dynamic routes.

**💡 Tip:** `generateStaticParams` = **"pre-build these pages"**. List all slugs/IDs at build time. Essential for SSG with dynamic routes.

---

### Q17. What is the `metadata` API?

**Answer:** Define page metadata for SEO in Server Components:
```tsx
export const metadata: Metadata = {
  title: "My Page",
  description: "Page description",
  openGraph: { image: "/og.png" },
};
// Dynamic: generateMetadata function
export async function generateMetadata({ params }) {
  return { title: `Post: ${params.slug}` };
}
```

**💡 Tip:** `metadata` export = **SEO in a box**. Static: `export const metadata`. Dynamic: `export function generateMetadata()`. Replaces `next/head`.

---

### Q18. What are Server Actions in Next.js?

**Answer:** Server-side functions called directly from client components:
```tsx
// app/actions.ts
"use server";
async function createUser(formData: FormData) {
  await db.users.create({ name: formData.get("name") });
}
```

No API route needed. Works with `<form action={createUser}>`. Progressive enhancement compatible.

**💡 Tip:** Server Actions = **"API without the API."** `"use server"` = runs on server. Call from forms or event handlers. No fetch needed.

---

### Q19. What is `next/script`?

**Answer:** Optimized script loading with strategies:
- `beforeInteractive`: loads before page interactive (high priority)
- `afterInteractive`: loads immediately after page interactive (default)
- `lazyOnload`: loads during idle time (analytics, low priority)
- `worker`: loads in web worker (experimental)

```tsx
<Script src="https://analytics.com/script.js" strategy="lazyOnload" />
```

**💡 Tip:** Use `strategy="lazyOnload"` for analytics. Use `afterInteractive` for needed scripts. Never load third-party scripts directly in `<head>`.

---

### Q20. What is the `notFound` function?

**Answer:** `notFound()` triggers the 404 page:
```tsx
import { notFound } from "next/navigation";
async function Page({ params }) {
  const post = await getPost(params.slug);
  if (!post) notFound(); // renders not-found.tsx
  return <Post post={post} />;
}
```

Create `not-found.tsx` for custom 404 UI.

**💡 Tip:** `notFound()` = **"this doesn't exist, show 404."** Create `not-found.tsx` in any folder for custom 404 UI at that route level.

---

## Intermediate Questions (Q21–Q40)

### Q21. What is streaming in Next.js?

**Answer:** Streaming sends HTML in chunks — fast initial display, progressive loading:
- `loading.tsx`: instant skeleton, data loads in background
- `<Suspense>`: wrap specific sections for progressive rendering
- Server can stream data as it becomes available

**💡 Tip:** Streaming = **"show what you have, load the rest."** No more blank page while waiting for all data. `loading.tsx` + Suspense enable this.

---

### Q22. What are parallel routes?

**Answer:** Render multiple pages simultaneously in the same layout:
```tsx
// app/dashboard/layout.tsx
export default function DashboardLayout({ team, analytics }) {
  return (
    <div>
      <section>{team}</section>
      <section>{analytics}</section>
    </div>
  );
}
// app/dashboard/@team/page.tsx + app/dashboard/@analytics/page.tsx
```

Uses `@folder` naming convention (not URL segments). Independent loading/error states.

**💡 Tip:** Parallel routes = **multiple pages in one layout**. `@folder` = slot name (not in URL). Each slot loads independently.

---

### Q23. What are intercepting routes?

**Answer:** Intercept routes to show modals/overlays without leaving the current page:
```tsx
// app/@modal/(.)photo/[id]/page.tsx — intercepts /photo/:id
// app/photo/[id]/page.tsx — direct URL shows full page
```

Convention: `(.)` same level, `(..)` one level up, `(...)` root level. Uses parallel routes (`@modal`).

**💡 Tip:** Interception = **"catch this route, show a modal instead."** Like Instagram — click photo shows modal, but direct URL shows full page.

---

### Q24. What is the difference between `useRouter` from `next/navigation` vs `next/router`?

**Answer:**
- `next/navigation` (App Router): `router.push()`, `router.replace()`, `router.back()`, `router.refresh()`. No `router.query` (use `searchParams` prop).
- `next/router` (Pages Router): includes `query`, `pathname`, `asPath`, events. Legacy.

**💡 Tip:** App Router → `import { useRouter } from "next/navigation"`. Pages Router → `import { useRouter } from "next/router"`. Don't mix them.

---

### Q25. What are Route Groups?

**Answer:** `(folder)` parentheses create route groups — organize routes without affecting the URL:
```
app/(marketing)/about/page.tsx → /about (not /(marketing)/about)
app/(marketing)/layout.tsx → shared layout for marketing pages
app/(shop)/products/page.tsx → /products
```

Each group can have its own layout. URL is unaffected.

**💡 Tip:** `(parentheses)` = **invisible folder** for organization only. Doesn't appear in URL. Use to group routes with shared layouts.

---

### Q26. What is `revalidatePath` and `revalidateTag`?

**Answer:** Cache invalidation methods:
- `revalidatePath("/posts")` — invalidates cache for a specific path
- `revalidateTag("posts")` — invalidates all fetches tagged with `"posts"`

```tsx
// In Server Action
"use server";
import { revalidateTag } from "next/cache";
await revalidateTag("posts"); // refreshes all post-related data
```

**💡 Tip:** `revalidatePath` = **invalidate by URL**. `revalidateTag` = **invalidate by tag** (more targeted). Use tags when multiple pages share the same data.

---

### Q27. How does Next.js handle CSS?

**Answer:**
- **Global CSS**: `app/globals.css` (imported in layout)
- **CSS Modules**: `component.module.css` (scoped, no conflicts)
- **Tailwind CSS**: built-in support, most popular choice
- **CSS-in-JS**: supported but requires configuration for SSR

**💡 Tip:** Most Next.js projects use **Tailwind CSS**. CSS Modules for component-scoped styles. Global CSS only for resets/fonts.

---

### Q28. What is `generateMetadata` vs static `metadata`?

**Answer:**
- Static: `export const metadata = { title: "Home" }` — same for every request
- Dynamic: `export async function generateMetadata({ params }) { ... }` — varies per route

Use static when metadata is fixed. Use `generateMetadata` when it depends on data (blog post title, product name).

**💡 Tip:** Static metadata = **same for all**. `generateMetadata` = **per-request**. Dynamic adds to response time — only use when needed.

---

### Q29. What is `next.config.js`?

**Answer:** Configuration file for Next.js:
```js
module.exports = {
  images: { domains: ["cdn.example.com"] },
  env: { API_URL: process.env.API_URL },
  redirects: async () => [{ source: "/old", destination: "/new", permanent: true }],
  headers: async () => [{ source: "/(.*)", headers: [{ key: "X-Frame-Options", value: "DENY" }] }],
};
```

**💡 Tip:** `next.config.js` = **settings for your Next.js app**. Common: image domains, env variables, redirects, headers, experimental features.

---

### Q30. What is `ImageResponse` in Next.js?

**Answer:** Generate dynamic images (OG images, social cards) using JSX:
```tsx
import { ImageResponse } from "next/og";
export async function GET() {
  return new ImageResponse(
    (<div style={{ fontSize: 60 }}>Hello, World!</div>),
    { width: 1200, height: 600 }
  );
}
```

Uses Satori under the hood — JSX to SVG to PNG. No canvas/headless browser needed.

**💡 Tip:** `ImageResponse` = **OG images from JSX**. Place in `app/opengraph-image.tsx` for automatic OG image generation. No design tools needed.

---

### Q31. What is Turbopack?

**Answer:** Turbopack is Next.js's new bundler (replacing Webpack), written in Rust:
- Up to 700x faster than Webpack for cold starts
- Incremental computation (only recompiles what changed)
- Default in `next dev` (Next.js 15+)

**💡 Tip:** Turbopack = **WebPack's faster successor**. Built in Rust. Currently for development — production still uses Webpack/Turbopack gradually.

---

### Q32. What is the `cookies()` function?

**Answer:** Server-side cookie access in Server Components and Route Handlers:
```tsx
import { cookies } from "next/headers";
const cookieStore = await cookies();
const token = cookieStore.get("auth-token");
cookieStore.set("theme", "dark");
```

Read-only in Server Components. Read/write in Server Actions and Route Handlers.

**💡 Tip:** `cookies()` = **server-side cookie jar**. Import from `next/headers`. Async in Next.js 15+. Use for auth tokens, user preferences.

---

### Q33. What is the `headers()` function?

**Answer:** Access request headers on the server:
```tsx
import { headers } from "next/headers";
const headersList = await headers();
const auth = headersList.get("authorization");
const ip = headersList.get("x-forwarded-for");
```

Read-only. Useful for auth, geo-location, request inspection.

**💡 Tip:** `headers()` = **server-side request reader**. Import from `next/headers`. Async in Next.js 15+. Use for auth and request context.

---

### Q34. What is the difference between `redirect` and `permanentRedirect`?

**Answer:**
- `redirect(path)` — 307 (temporary) redirect
- `permanentRedirect(path)` — 308 (permanent) redirect

Both can be used in Server Components, Route Handlers, and Server Actions.

**💡 Tip:** `redirect` = **temporary** (browser doesn't cache). `permanentRedirect` = **permanent** (browser caches). Use `redirect` for auth flows.

---

### Q35. What is static vs dynamic rendering?

**Answer:**
- **Static**: rendered at build time, cached, served instantly. Default when no dynamic data.
- **Dynamic**: rendered per request. Triggered by: `cookies()`, `headers()`, `searchParams`, `cache: "no-store"`, `unstable_noStore()`.

Next.js automatically detects which rendering to use based on your code.

**💡 Tip:** Static = **fast, cached** (default). Dynamic = **fresh, per-request** (triggered by dynamic APIs). Next.js auto-detects. Optimize for static when possible.

---

### Q36. What is `fetch` caching in Next.js?

**Answer:** Next.js extends the native `fetch` with caching:
```tsx
fetch(url);                                  // cached (default)
fetch(url, { cache: "force-cache" });        // cache like SSG
fetch(url, { cache: "no-store" });           // no cache (SSR)
fetch(url, { next: { revalidate: 3600 } });  // ISR (revalidate every hour)
fetch(url, { next: { tags: ["posts"] } });   // tag-based revalidation
```

**💡 Tip:** Default = **cached**. `no-store` = SSR. `revalidate` = ISR. `tags` = targeted invalidation. Next.js extends native fetch options.

---

### Q37. What is the App Router navigation behavior?

**Answer:** On navigation:
1. Server renders the new route's RSC payload
2. Only the changing segments re-render (shared layouts persist)
3. Client-side navigation (no full page reload)
4. Scroll position resets (can opt out with `scroll: false`)

```tsx
<Link href="/about" scroll={false}>About</Link>
router.push("/about", { scroll: false });
```

**💡 Tip:** Navigation = **partial update**. Shared layouts don't re-render. Scroll resets by default. Use `scroll={false}` to preserve position.

---

### Q38. What are the different rendering environments in Next.js?

**Answer:**
- **Node.js runtime**: default for Route Handlers, SSR, Server Actions. Full Node.js API access.
- **Edge runtime**: Middleware, some Route Handlers. Fast, limited APIs, no Node.js-specific modules.
- **Client runtime**: browser. Full DOM access, hooks, events.

Specify with `export const runtime = "edge"`.

**💡 Tip:** Default = **Node.js**. Edge = **faster but limited**. Choose based on needed APIs. Middleware always runs on Edge.

---

### Q39. What is `draftMode`?

**Answer:** Enables preview/draft mode for CMS content:
```tsx
import { draftMode } from "next/headers";
const { isEnabled } = await draftMode();
// Enable: draftMode().enable()
// Disable: draftMode().disable()
```

When enabled, bypasses static cache and renders fresh content. Used with CMS preview.

**💡 Tip:** `draftMode` = **"show me the unpublished version."** Used with headless CMS. Bypasses SSG cache for preview.

---

### Q40. What is `next.config.ts` (TypeScript config)?

**Answer:** Next.js 15+ supports TypeScript config:
```ts
import type { NextConfig } from "next";
const nextConfig: NextConfig = {
  images: { domains: ["cdn.example.com"] },
};
export default nextConfig;
```

Gets autocomplete and type checking for configuration options.

**💡 Tip:** `next.config.ts` > `next.config.js` — type safety for your config. Use TS version in Next.js 15+.

---

## Advanced Questions (Q41–Q50)

### Q41. How does Next.js handle code splitting?

**Answer:** Next.js automatically code-splits:
- **Page-level**: each page is a separate chunk
- **Component-level**: `React.lazy()` for dynamic imports
- **Route-level**: App Router splits per route segment
- **Third-party**: `next/dynamic` with `{ ssr: false }` for client-only libs

```tsx
import dynamic from "next/dynamic";
const Chart = dynamic(() => import("./Chart"), { ssr: false, loading: () => <Skeleton /> });
```

**💡 Tip:** Pages auto-split. Heavy components → `next/dynamic`. Client-only libs → `{ ssr: false }`. Don't manually code-split — Next.js handles it.

---

### Q42. What is Partial Prerendering (PPR)?

**Answer:** PPR combines static shell + dynamic holes in one HTTP request:
- Static parts (layout, navigation) cached and served instantly
- Dynamic parts (user-specific content) stream in via Suspense boundaries

```tsx
export const experimental_ppr = true;
// Static shell wraps Suspense boundaries for dynamic content
```

**💡 Tip:** PPR = **best of SSG + SSR**. Static frame loads instantly, dynamic content streams in. Experimental in Next.js 14, stable in 15.

---

### Q43. How to handle authentication in Next.js?

**Answer:** Common approaches:
- **Middleware**: check auth token before route loads (redirect unauthenticated)
- **Auth.js (NextAuth)**: popular library for OAuth, credentials, JWT
- **Clerk / Supabase Auth**: managed auth solutions
- **Custom**: JWT in cookies, verify in Server Components/Route Handlers

**💡 Tip:** Middleware = **first line of defense** (redirect). Server Components = **verify and render**. Store tokens in **httpOnly cookies**, not localStorage.

---

### Q44. What is `unstable_after`?

**Answer:** `after()` runs code after the response is sent (non-blocking):
```tsx
import { after } from "next/server";
export async function GET() {
  const data = await getData();
  after(async () => {
    await logRequest(); // runs after response, doesn't block user
  });
  return Response.json(data);
}
```

Perfect for analytics, logging, webhooks — tasks that shouldn't slow the response.

**💡 Tip:** `after()` = **"do this later, don't make the user wait."** Analytics, logging, cleanup. User gets instant response, tasks run in background.

---

### Q45. How does ISR (Incremental Static Regeneration) work?

**Answer:** ISR serves static pages and revalidates them in the background:
- `revalidate: 60` — revalidate every 60 seconds
- Time-based: revalidates after the specified time
- On-demand: `revalidatePath()` / `revalidateTag()` triggered by events

First request after expiry serves stale, triggers regeneration in background, next request gets fresh.

**💡 Tip:** ISR = **"serve stale, revalidate in background."** User never waits. Stale-while-revalidate pattern. Best for semi-dynamic content.

---

### Q46. What is the `useSearchParams` hook?

**Answer:** Reads URL search parameters in Client Components:
```tsx
"use client";
import { useSearchParams } from "next/navigation";
function SearchPage() {
  const searchParams = useSearchParams();
  const query = searchParams.get("q");
}
```

Wraps the component in Suspense (searchParams triggers client rendering). Server Components: use `searchParams` prop.

**💡 Tip:** Client: `useSearchParams()`. Server: `{ searchParams }` prop. SearchParams triggers dynamic rendering — not cached.

---

### Q47. What are instrumentation hooks?

**Answer:** `instrumentation.ts` runs setup code on server startup:
```ts
export async function register() {
  if (process.env.NEXT_RUNTIME === "nodejs") {
    // Setup OpenTelemetry, error tracking, etc.
  }
}
```

Runs once on server start. Use for: APM setup, error tracking (Sentry), custom server init.

**💡 Tip:** `instrumentation.ts` = **"run this when server starts."** Sentry, OpenTelemetry, custom init. Runs per runtime (Node.js + Edge).

---

### Q48. How does Next.js handle environment variables?

**Answer:**
- `NEXT_PUBLIC_*`: exposed to browser (embedded in JS bundle)
- Server-only: no prefix, only accessible in Server Components/API routes
- `.env.local`: local overrides (gitignored)
- `.env.production` / `.env.development`: environment-specific

**💡 Tip:** `NEXT_PUBLIC_` = **browser can see**. No prefix = **server only**. Never put secrets in `NEXT_PUBLIC_*` variables.

---

### Q49. What is the `unstable_noStore` function?

**Answer:** `unstable_noStore()` (now `connection()` in newer versions) opts a component out of caching:
```tsx
import { unstable_noStore as noStore } from "next/cache";
async function Page() {
  noStore(); // This page is always dynamic
  const data = await fetch("/api/data");
}
```

Equivalent to `cache: "no-store"` on every fetch. Makes the entire route dynamic.

**💡 Tip:** `noStore()` = **"never cache this page."** Simple opt-out of static rendering. Use for always-fresh pages (dashboards, admin).

---

### Q50. How to optimize Core Web Vitals in Next.js?

**Answer:**
- **LCP**: use `next/image`, server components, preload critical resources
- **INP**: minimize client JS, use server components, `useTransition` for heavy updates
- **CLS**: always set image dimensions, use `next/font` (no FOUT), avoid dynamic injected content
- `next/script` with correct strategy, dynamic imports for non-critical code

**💡 Tip:** Next.js handles most optimization **out of the box**. Use `next/image`, `next/font`, Server Components, proper caching. Check with Lighthouse.

---
