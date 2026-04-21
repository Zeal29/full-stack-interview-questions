# CSS Interview Questions — Top 50

> **Difficulty Split:** Basic (Q1–Q20) | Intermediate (Q21–Q40) | Advanced (Q41–Q50)

---

## Basic Questions (Q1–Q20)

### Q1. What is CSS?

**Answer:** CSS (Cascading Style Sheets) is a stylesheet language used to describe the visual presentation of HTML elements — controlling colors, fonts, layout, spacing, and responsive behavior. It separates content (HTML) from presentation.

**💡 Tip:** HTML = **structure**, CSS = **style**, JS = **behavior**. Three pillars of the web.

---

### Q2. What are the different ways to include CSS in HTML?

**Answer:**
- **Inline**: `style` attribute on element — highest priority, least maintainable
- **Internal/Embedded**: `<style>` tag in `<head>` — page-specific
- **External**: `<link>` to `.css` file — best practice, reusable, cacheable

**💡 Tip:** Priority: Inline > Internal > External. Always prefer **External** for maintainability.

---

### Q3. What is the CSS Box Model?

**Answer:** Every element is a box with four layers (inside-out): **Content** → **Padding** → **Border** → **Margin**. By default, `width` only applies to content. `box-sizing: border-box` makes width include padding + border.

**💡 Tip:** Remember **C-P-B-M**. Use `box-sizing: border-box` everywhere — makes sizing predictable.

---

### Q4. What is the difference between `id` and `class` selectors?

**Answer:** `#id` selects a unique element (high specificity). `.class` selects multiple elements (lower specificity). IDs should be unique per page; classes can be reused. Prefer classes for styling.

**💡 Tip:** **ID** = one element (like your CNIC). **Class** = group of elements (like school class). Prefer **class** for styling.

---

### Q5. What is specificity in CSS?

**Answer:** Specificity determines which CSS rule applies when multiple rules target the same element. Ranking (low to high): `element` (1) → `.class` (10) → `#id` (100) → `inline style` (1000) → `!important` (overrides all).

**💡 Tip:** Specificity score: **(inline, id, class, element)**. `!important` breaks the cascade — avoid it.

---

### Q6. What is the `display` property and its common values?

**Answer:**
- `block` — full width, new line (`<div>`, `<p>`)
- `inline` — no line break, width/height ignored (`<span>`, `<a>`)
- `inline-block` — inline but respects width/height
- `none` — hidden, removed from layout
- `flex` — flexbox container
- `grid` — grid container

**💡 Tip:** Block = **full row**. Inline = **in the flow**. Inline-block = **best of both**.

---

### Q7. What is the difference between `position: relative`, `absolute`, `fixed`, and `sticky`?

**Answer:**
- **relative**: offset from its normal position (leaves a gap)
- **absolute**: positioned relative to nearest positioned ancestor (removed from flow)
- **fixed**: positioned relative to viewport (stays on scroll)
- **sticky**: toggles between relative and fixed based on scroll position

**💡 Tip:** Relative = **shift from self**. Absolute = **relative to parent**. Fixed = **relative to screen**. Sticky = **smart hybrid**.

---

### Q8. What is Flexbox?

**Answer:** Flexbox is a one-dimensional layout method for arranging items in rows or columns. Key properties: `display: flex`, `justify-content` (main axis), `align-items` (cross axis), `flex-direction`, `flex-wrap`, `gap`.

**💡 Tip:** Flexbox = **one direction** (row OR column). Remember: `justify-content` = main axis, `align-items` = cross axis.

---

### Q9. What is CSS Grid?

**Answer:** CSS Grid is a two-dimensional layout system for creating rows AND columns simultaneously. Key properties: `display: grid`, `grid-template-columns`, `grid-template-rows`, `gap`, `grid-area`. Best for overall page layouts.

**💡 Tip:** Flexbox = **1D** (row OR column). Grid = **2D** (rows AND columns). Use Grid for page layout, Flexbox for component layout.

---

### Q10. What is the difference between `rem`, `em`, `px`, and `%`?

**Answer:**
- `px` — fixed pixel value, doesn't scale
- `em` — relative to parent's font-size (cascades)
- `rem` — relative to root (`<html>`) font-size (consistent)
- `%` — relative to parent element's property value

**💡 Tip:** Prefer **`rem`** for scalable, consistent sizing. `em` compounds (can surprise you). `px` is rigid.

---

### Q11. What are CSS selectors and their types?

**Answer:**
- **Element**: `p { }`
- **Class**: `.card { }`
- **ID**: `#header { }`
- **Descendant**: `div p { }` (any nested p)
- **Child**: `div > p { }` (direct child only)
- **Attribute**: `input[type="text"] { }`
- **Pseudo-class**: `:hover`, `:first-child`
- **Pseudo-element**: `::before`, `::after`

**💡 Tip:** Single colon `:` = pseudo-**class** (state). Double colon `::` = pseudo-**element** (create content).

---

### Q12. What is the `z-index` property?

**Answer:** `z-index` controls the stacking order of positioned elements (must have `position` other than `static`). Higher value = appears on top. Default is `auto` (0). Creates stacking contexts.

**💡 Tip:** Z-index only works on **positioned** elements. Don't go crazy with values like 999999 — use a scale like 1, 10, 100.

---

### Q13. What are pseudo-classes and pseudo-elements?

**Answer:**
- **Pseudo-classes** (`:`) target element **states**: `:hover`, `:focus`, `:first-child`, `:nth-child(n)`, `:checked`
- **Pseudo-elements** (`::`) target **virtual elements**: `::before`, `::after`, `::first-line`, `::placeholder`

**💡 Tip:** One colon `:` = **state** (hover, focus). Two colons `::` = **fake element** (before, after).

---

### Q14. What is media query in CSS?

**Answer:** Media queries apply styles based on device conditions (screen width, orientation, resolution). Syntax: `@media (max-width: 768px) { ... }`. Essential for responsive design — Mobile-first approach: start with mobile styles, add breakpoints for larger screens.

**💡 Tip:** **Mobile-first**: use `min-width` (styles grow). **Desktop-first**: use `max-width` (styles shrink). Prefer mobile-first.

---

### Q15. What is the `float` property?

**Answer:** `float` moves an element to the left or right of its container, allowing inline content to wrap around it. Originally for images, now largely replaced by Flexbox/Grid. Requires `clear` to stop wrapping.

**💡 Tip:** Float is **legacy** for layout. Use Flexbox/Grid instead. Float is still useful for wrapping text around images.

---

### Q16. What is the `overflow` property?

**Answer:** `overflow` controls what happens when content overflows its container:
- `visible` (default) — content overflows visibly
- `hidden` — clipped, no scrollbar
- `scroll` — always shows scrollbar
- `auto` — shows scrollbar only when needed

**💡 Tip:** Use `overflow: auto` for **smart scrollbars** (only when content overflows).

---

### Q17. What is the `transition` property?

**Answer:** `transition` creates smooth animations between CSS property changes. Syntax: `transition: property duration timing-function delay`. Example: `transition: background-color 0.3s ease`.

**💡 Tip:** Transition = **auto-pilot animation** between two states (hover → normal). Use `ease` for natural feel.

---

### Q18. What is the `transform` property?

**Answer:** `transform` applies 2D/3D transformations: `translate()`, `rotate()`, `scale()`, `skew()`. Doesn't affect document flow. Hardware-accelerated (GPU), so it's performant for animations.

**💡 Tip:** `transform` + `opacity` = **GPU-accelerated** animations. Always prefer these for performance over animating `top/left/width`.

---

### Q19. What is the `box-sizing` property?

**Answer:** `box-sizing` controls how width/height are calculated:
- `content-box` (default): width = content only (padding + border added outside)
- `border-box`: width = content + padding + border (total fits within specified width)

**💡 Tip:** Always use `* { box-sizing: border-box; }` — makes sizing predictable and is industry standard.

---

### Q20. What are CSS variables (custom properties)?

**Answer:** CSS variables store reusable values: `--primary: #3498db;` and use with `var(--primary)`. Defined on `:root` for global scope. Can be changed per-element, overridden, and updated with JavaScript.

**💡 Tip:** Think of CSS variables as **constants** — define once, use everywhere. Change in one place = updates everywhere.

---

## Intermediate Questions (Q21–Q40)

### Q21. What is the CSS cascade?

**Answer:** The cascade determines which styles apply when multiple rules target the same element. Order of priority: (1) Origin & importance, (2) Specificity, (3) Source order. Later rules win when specificity is equal.

**💡 Tip:** Cascade = **competition**. Winner determined by: `!important` > inline > `#id` > `.class` > element > last one wins.

---

### Q22. What is CSS inheritance?

**Answer:** Some CSS properties automatically pass from parent to child: `color`, `font-size`, `font-family`, `line-height`, `text-align`. Layout properties (`margin`, `padding`, `border`, `width`) are NOT inherited. Use `inherit` keyword to force inheritance.

**💡 Tip:** **Text/font** properties inherit. **Box/layout** properties don't. Use `inherit` to override.

---

### Q23. What is BEM (Block Element Modifier)?

**Answer:** BEM is a CSS naming convention:
- **Block**: `.card` (standalone component)
- **Element**: `.card__title` (part of block, double underscore)
- **Modifier**: `.card--dark` (variation, double dash)

Prevents specificity wars and makes CSS maintainable.

**💡 Tip:** BEM = **Block__Element--Modifier**. Keeps specificity flat (all at class level).

---

### Q24. What is the `calc()` function?

**Answer:** `calc()` performs calculations in CSS: `width: calc(100% - 60px);`. Mixes different units (%, px, em, vw). Spaces required around `+` and `-` operators. Useful for responsive layouts.

**💡 Tip:** `calc()` = **CSS calculator**. Mix units freely: `calc(100% - 2rem)`. Remember **spaces** around `+` and `-`.

---

### Q25. What is the difference between `visibility: hidden` and `display: none`?

**Answer:**
- `display: none` — removes element from layout (no space occupied)
- `visibility: hidden` — hides element but keeps its space in layout
- `opacity: 0` — invisible but still interactive (can be clicked)

**💡 Tip:** `display:none` = **gone** (no space). `visibility:hidden` = **invisible ghost** (still occupies space). `opacity:0` = **transparent** (still interactive).

---

### Q26. What is the `@keyframes` rule?

**Answer:** `@keyframes` defines multi-step animations:
```css
@keyframes slide {
  from { transform: translateX(0); }
  to { transform: translateX(100px); }
}
/* Usage */
.element { animation: slide 2s ease infinite; }
```

**💡 Tip:** `@keyframes` = **animation script** (from/to or 0%/50%/100%). `animation` property = **plays it**.

---

### Q27. What is CSS reset vs normalize?

**Answer:**
- **CSS Reset**: strips ALL default browser styles (margins, paddings, font sizes) — start from zero
- **Normalize.css**: makes default styles **consistent** across browsers while keeping useful defaults

**💡 Tip:** Reset = **nuclear option** (remove everything). Normalize = **diplomatic** (make consistent, keep useful). Most projects use normalize or a light reset.

---

### Q28. What is the `vh`, `vw`, `vmin`, `vmax` units?

**Answer:**
- `vw` — 1% of viewport width
- `vh` — 1% of viewport height
- `vmin` — 1% of the smaller viewport dimension
- `vmax` — 1% of the larger viewport dimension

**💡 Tip:** Viewport units = **screen-relative sizing**. `100vh` = full screen height. Great for hero sections.

---

### Q29. What is the CSS `clip-path` property?

**Answer:** `clip-path` defines a clipping region — only content inside the region is visible. Create shapes: `circle()`, `polygon()`, `ellipse()`, `inset()`. Used for creative UI shapes without images.

**💡 Tip:** `clip-path` = **cookie cutter** for elements. Use Clippy (online tool) to generate values visually.

---

### Q30. What is the `object-fit` property?

**Answer:** `object-fit` controls how images/videos fit inside their container:
- `fill` (default) — stretches to fill
- `contain` — fits entirely, may letterbox
- `cover` — fills container, may crop
- `none` — original size
- `scale-down` — picks smaller of none or contain

**💡 Tip:** Use `object-fit: cover` for **profile pics/hero images** (fills without distortion). Like `background-size: cover` but for `<img>`.

---

### Q31. What is CSS `grid-template-areas`?

**Answer:** Named grid areas for intuitive layout:
```css
grid-template-areas:
  "header header header"
  "sidebar main main"
  "footer footer footer";
```

**💡 Tip:** Grid areas = **visual layout in code**. Draw your layout with text — easy to read and modify.

---

### Q32. What is the `currentColor` keyword?

**Answer:** `currentColor` inherits the current `color` property value. Useful for SVG icons, borders matching text color: `border: 2px solid currentColor;`. Changes automatically when `color` changes.

**💡 Tip:** `currentColor` = **CSS variable before CSS variables existed**. Automatically tracks the element's text color.

---

### Q33. What is the `will-change` property?

**Answer:** `will-change` hints to the browser which properties will animate, allowing it to optimize in advance: `will-change: transform, opacity;`. Overuse causes performance problems — only apply before animations, remove after.

**💡 Tip:** `will-change` = **heads-up to browser** ("hey, I'm about to animate this"). Use sparingly — too many hints wastes memory.

---

### Q34. What is CSS `@supports` (feature query)?

**Answer:** `@supports` checks if the browser supports a CSS property before applying styles:
```css
@supports (display: grid) {
  .container { display: grid; }
}
```

**💡 Tip:** `@supports` = **CSS feature detection** — progressive enhancement without JavaScript.

---

### Q35. What are CSS `min()`, `max()`, and `clamp()` functions?

**Answer:**
- `min(100%, 500px)` — picks the smaller value
- `max(100%, 500px)` — picks the larger value
- `clamp(min, preferred, max)` — fluid sizing between bounds

Example: `font-size: clamp(1rem, 2.5vw, 2rem)` — scales between 1rem and 2rem.

**💡 Tip:** `clamp()` = **responsive without media queries**. The modern way to do fluid typography/sizing.

---

### Q36. What is the `aspect-ratio` property?

**Answer:** `aspect-ratio` sets the width-to-height ratio of an element: `aspect-ratio: 16 / 9;`. Eliminates the need for the padding-bottom hack for responsive videos/images.

**💡 Tip:** `aspect-ratio` = **shape lock**. `16/9` for video, `1/1` for squares. Modern CSS makes the padding hack obsolete.

---

### Q37. What is the difference between Flexbox `justify-content` and `align-items`?

**Answer:**
- `justify-content` — aligns items along the **main axis** (default: horizontal)
- `align-items` — aligns items along the **cross axis** (default: vertical)
- Switch with `flex-direction: column` — axes swap

**💡 Tip:** Justify = **main** axis. Align = **cross** axis. Default row = justify is horizontal, align is vertical.

---

### Q38. What is CSS `:root` and why is it used?

**Answer:** `:root` targets the document's root element (`<html>`). It's the highest-level selector and the standard place to define CSS custom properties (variables). Has higher specificity than `html` selector.

**💡 Tip:** Always define CSS variables on `:root` — it's the **global scope** for your design tokens.

---

### Q39. What is the `backdrop-filter` property?

**Answer:** `backdrop-filter` applies effects (blur, brightness) to the area **behind** an element. Used for frosted glass/blur effects: `backdrop-filter: blur(10px);`. Common in modal overlays and glassmorphism UI.

**💡 Tip:** `backdrop-filter: blur()` = **frosted glass effect**. Requires semi-transparent background to see the effect.

---

### Q40. What is CSS specificity hierarchy?

**Answer:** Specificity ranking (low to high):
1. `*`, `+`, `>`, `~` (combinators) — 0
2. Element/pseudo-element (`p`, `::before`) — 1
3. Class/attribute/pseudo-class (`.card`, `:hover`) — 10
4. ID (`#header`) — 100
5. Inline styles — 1000
6. `!important` — overrides all

**💡 Tip:** Specificity is a **4-digit number**: (inline, id, class, element). `0,1,0,0` beats `0,0,10,0`. Avoid `!important`.

---

## Advanced Questions (Q41–Q50)

### Q41. What is a stacking context?

**Answer:** A stacking context is a three-dimensional layering of elements. Created by: `position` + `z-index`, `opacity < 1`, `transform`, `filter`, `will-change`, `isolation: isolate`. Elements inside a context can't escape their parent's z-index ordering.

**💡 Tip:** Stacking context = **grouped z-index bubble**. Child elements compete within their parent's context, not globally.

---

### Q42. What is the CSS `contain` property?

**Answer:** `contain` tells the browser an element is independent, enabling performance optimizations: `layout`, `paint`, `size`, `inline-size`, `strict` (all), `content` (safe subset). Limits style/layout recalculations to the contained element.

**💡 Tip:** `contain: layout paint` on repeated list items = **performance boost**. Browser skips recalculating outside the container.

---

### Q43. What are CSS `@layer` rules?

**Answer:** `@layer` creates cascade layers to control specificity across groups of styles:
```css
@layer base, components, utilities;
@layer base { h1 { color: black; } }
@layer components { h1 { color: blue; } }
```

Later layers win regardless of specificity within each layer.

**💡 Tip:** `@layer` = **organized cascade**. Third-party CSS can't override your styles if you layer correctly.

---

### Q44. What is the CSS `:has()` selector (parent selector)?

**Answer:** `:has()` selects a parent based on its children — CSS's first parent selector: `div:has(p) { }` selects divs containing a `<p>`. `card:has(img)` selects cards with images. Called the "family selector."

**💡 Tip:** `:has()` = **CSS parent selector** (long-awaited). `a:has(> img)` = "select links that directly contain an image."

---

### Q45. What is CSS Houdini?

**Answer:** CSS Houdini is a set of low-level APIs that let developers extend CSS directly: Paint API (custom visual effects), Layout API (custom layouts), Properties & Values API (typed custom properties), Animation Worklet (scroll-linked animations). Gives direct access to the CSS engine.

**💡 Tip:** Houdini = **extend CSS itself** (not just use it). Still gaining browser support, but Typed OM and Properties API are usable now.

---

### Q46. What is the `content-visibility` property?

**Answer:** `content-visibility: auto` skips rendering off-screen elements, dramatically improving initial page load performance. `contain-intrinsic-size` provides a placeholder size estimate. The browser renders content only when it's about to enter the viewport.

**💡 Tip:** `content-visibility: auto` = **lazy rendering for CSS**. Huge perf win for long pages — browser skips off-screen sections.

---

### Q47. What is the CSS `:is()` and `:where()` selector?

**Answer:**
- `:is(header, main, footer) p { }` — matches p inside header, main, or footer. Takes specificity of most specific argument.
- `:where(header, main, footer) p { }` — same matching but **zero specificity** (easily overridable)

**💡 Tip:** `:is()` = **DRY selector** with real specificity. `:where()` = **zero specificity** (can't override anything). Use `:where()` for resets/utilities.

---

### Q48. How does CSS container query work?

**Answer:** Container queries apply styles based on a **parent container's size** (not viewport like media queries):
```css
@container (min-width: 400px) { .card { flex-direction: row; } }
```
The parent must have `container-type: inline-size`. More component-friendly than media queries.

**💡 Tip:** Media query = **viewport** size. Container query = **parent** size. CQ = truly reusable components.

---

### Q49. What is CSS logical properties?

**Answer:** Logical properties replace physical directions with flow-relative ones: `margin-inline-start` (not `margin-left`), `padding-block-end` (not `padding-bottom`), `border-inline` (not `border-left/right`). Essential for RTL (right-to-left) language support.

**💡 Tip:** Logical = **direction-independent**. Works for both LTR and RTL. `inline` = text direction axis, `block` = perpendicular axis.

---

### Q50. What are CSS `scroll-snap` properties?

**Answer:** `scroll-snap-type` creates snap points during scrolling:
```css
.container { scroll-snap-type: x mandatory; }
.child { scroll-snap-align: start; }
```
- `mandatory` — always snaps
- `proximity` — snaps only if close
- Used for carousels, full-page sections, image galleries

**💡 Tip:** Scroll snap = **CSS-only carousel**. No JavaScript needed for basic snapping behavior.

---
