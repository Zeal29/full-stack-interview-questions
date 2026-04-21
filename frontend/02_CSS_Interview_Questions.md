# 🎨 CSS Interview Questions — Top 50 Most Asked (2026)

> **Difficulty Split:** ~40% Basic (Q1–20) | ~45% Intermediate (Q21–42) | ~15% Advanced (Q43–50)
> **Audience:** Backend/Full-Stack Engineers who need frontend knowledge
> **Sources:** GeeksforGeeks, InterviewBit, CSS-Tricks, GreatFrontEnd, UpGrad, FinalRoundAI

---

## 🟢 BASIC (Questions 1–20)

---

### Q1. What is CSS?

**Answer:** CSS (Cascading Style Sheets) is a stylesheet language used to describe the visual presentation of HTML documents. It controls layout, colors, fonts, spacing, and responsiveness. CSS separates content (HTML) from presentation, making maintenance easier.

**Memory Tip:** HTML = **structure**. CSS = **style**. JS = **behavior**.

---

### Q2. What are the three ways to include CSS?

**Answer:**
1. **Inline CSS:** `style` attribute directly on element — highest priority
2. **Internal CSS:** `<style>` tag inside `<head>` — page-specific
3. **External CSS:** Linked `.css` file via `<link>` — best practice, reusable

Priority order: Inline > Internal > External.

**Memory Tip:** Proximity wins — the closer the CSS is to the element, the higher its priority.

---

### Q3. What is the CSS Box Model?

**Answer:** Every HTML element is a rectangular box with four layers (inside → out):
1. **Content:** Text, images (affected by `width`/`height`)
2. **Padding:** Space between content and border
3. **Border:** Around the padding
4. **Margin:** Space between border and adjacent elements

```
| Margin |
|  Border  |
|   Padding  |
|    Content   |
```

**Memory Tip:** **M-B-P-C** from outside-in: **M**argin → **B**order → **P**adding → **C**ontent.

---

### Q4. What is the difference between `margin` and `padding`?

**Answer:**
- **Padding:** Space **inside** the border, between content and border. Background color fills padding.
- **Margin:** Space **outside** the border, between the element and neighbors. Background does NOT fill margin. Margins can collapse (merge) vertically.

**Memory Tip:** **P**adding is **p**ersonal space (inside). **M**argin is **m**eeting distance (outside).

---

### Q5. What are CSS selectors?

**Answer:** Selectors target HTML elements for styling:
- **Element:** `p { }` — selects all `<p>` tags
- **Class:** `.highlight { }` — selects elements with `class="highlight"`
- **ID:** `#header { }` — selects the element with `id="header"`
- **Universal:** `* { }` — selects everything
- **Attribute:** `input[type="text"] { }` — selects by attribute
- **Pseudo-class:** `a:hover { }` — selects by state
- **Pseudo-element:** `p::first-line { }` — selects part of element

**Memory Tip:** Selectors = **targeting mechanism**. Specificity increases: element < class < ID.

---

### Q6. What is CSS specificity?

**Answer:** Specificity determines which CSS rule wins when multiple rules target the same element. Calculated as a score:
- **Inline styles:** 1000
- **IDs:** 100
- **Classes, attributes, pseudo-classes:** 10
- **Elements, pseudo-elements:** 1

`#nav .menu li` = 1-1-1 (111) beats `.header .menu li` = 0-2-1 (21). Only `!important` overrides specificity.

**Memory Tip:** **ICE** — **I**D (100) > **C**lass (10) > **E**lement (1). Inline = 1000.

---

### Q7. What is the `box-sizing` property?

**Answer:**
- `content-box` (default): `width`/`height` apply to content only. Padding and border are **added on top**.
- `border-box`: `width`/`height` include content + padding + border. More predictable sizing.

```css
*, *::before, *::after {
  box-sizing: border-box; /* Universal best practice */
}
```

**Memory Tip:** `border-box` = what you **see is what you get**. Always use it globally.

---

### Q8. What is the difference between `display: none` and `visibility: hidden`?

**Answer:**
| Property | Space | Events | Screen Reader |
|----------|-------|--------|---------------|
| `display: none` | Removed from flow | No | No |
| `visibility: hidden` | Still occupies space | No | No |
| `opacity: 0` | Still occupies space | Yes | Yes |

**Memory Tip:** `display:none` = **vanishes completely**. `visibility:hidden` = **invisible ghost**. `opacity:0` = **transparent glass**.

---

### Q9. What are CSS units? Explain `px`, `em`, `rem`, `%`, `vh`, `vw`.

**Answer:**
- **px:** Absolute pixels (fixed size)
- **em:** Relative to parent's font-size (cascading, can compound)
- **rem:** Relative to root (`<html>`) font-size (predictable, preferred)
- **%:** Relative to parent element
- **vh/vw:** Viewport height/width (1vh = 1% of viewport)
- **fr:** Fraction unit for CSS Grid

**Memory Tip:** **R**em = **R**oot-based (predictable). **E**m = **E**lement-based (cascading). Prefer `rem` for fonts, `em` for component-relative sizing.

---

### Q10. What are pseudo-classes? Give examples.

**Answer:** Pseudo-classes select elements based on their **state**:
- `:hover` — mouse over element
- `:focus` — element has focus
- `:active` — element being clicked
- `:visited` — link already visited
- `:first-child` — first child of parent
- `:nth-child(2n)` — every even child
- `:not(.class)` — negation

```css
a:hover { color: red; }
li:nth-child(odd) { background: #f0f0f0; }
```

**Memory Tip:** Pseudo-classes = **state detectors** (single colon `:`).

---

### Q11. What are pseudo-elements? Give examples.

**Answer:** Pseudo-elements style **specific parts** of an element:
- `::before` — insert content before element
- `::after` — insert content after element
- `::first-line` — style first line of text
- `::first-letter` — style first letter
- `::placeholder` — style input placeholder
- `::selection` — style selected (highlighted) text

```css
.quote::before { content: """; }
p::first-letter { font-size: 2em; }
```

**Memory Tip:** Pseudo-elements = **virtual elements** (double colon `::`). Need `content` property for `::before`/`::after`.

---

### Q12. What are CSS combinators?

**Answer:** Combinators define relationships between selectors:
- **Descendant** (space): `div p` — any `<p>` inside `<div>` (any depth)
- **Child** (`>`): `div > p` — direct child only
- **Adjacent sibling** (`+`): `h2 + p` — `<p>` immediately after `<h2>`
- **General sibling** (`~`): `h2 ~ p` — all `<p>` siblings after `<h2>`

**Memory Tip:** Space = **any depth**. `>` = **direct child**. `+` = **next door neighbor**. `~` = **all siblings after**.

---

### Q13. What is the CSS `position` property?

**Answer:**
- **static** (default): Normal document flow
- **relative:** Normal flow + offset from its normal position (with `top`/`left`/etc.)
- **absolute:** Removed from flow, positioned relative to nearest positioned ancestor
- **fixed:** Positioned relative to viewport (stays on scroll)
- **sticky:** Switches between relative and fixed based on scroll position

**Memory Tip:** **S-R-A-F-S**: Static (normal) → Relative (shift from self) → Absolute (in ancestor) → Fixed (on screen) → Sticky (smart switch).

---

### Q14. What is the CSS `float` property?

**Answer:** `float` pushes an element to the left or right of its container, allowing inline content to wrap around it. Commonly used for text wrapping around images. Modern layouts use Flexbox/Grid instead of floats.

```css
img { float: left; margin-right: 1rem; }
```
Clear floats with `clear: both` or the clearfix hack.

**Memory Tip:** Float = **text wraps around** like a river around a rock. Legacy layout tool.

---

### Q15. What is the `z-index` property?

**Answer:** `z-index` controls the stacking order of **positioned** elements (not `static`). Higher values appear on top. Only works on elements with `position: relative`, `absolute`, `fixed`, or `sticky`. Stacking contexts can limit z-index scope.

**Memory Tip:** z-index = **layers** like Photoshop layers. Higher = closer to you. But only works if element is **positioned**.

---

### Q16. What is the `overflow` property?

**Answer:** Controls what happens when content overflows its container:
- `visible` (default): Content overflows visibly
- `hidden`: Overflow is clipped
- `scroll`: Always shows scrollbars
- `auto`: Shows scrollbars only when needed
- `overflow-x` / `overflow-y`: Control horizontal/vertical independently

**Memory Tip:** Overflow = **what happens when content is too big** for its box.

---

### Q17. How do you center a `div` horizontally?

**Answer:**
```css
/* Method 1: Auto margins (block element with fixed width) */
.container { width: 200px; margin: 0 auto; }

/* Method 2: Flexbox */
.parent { display: flex; justify-content: center; }

/* Method 3: Grid */
.parent { display: grid; place-items: center; }
```

**Memory Tip:** `margin: 0 auto` is the **classic** centering trick. Flexbox/Grid are the **modern** ways.

---

### Q18. How do you center a `div` vertically and horizontally?

**Answer:**
```css
/* Method 1: Flexbox */
.parent { display: flex; justify-content: center; align-items: center; }

/* Method 2: Grid */
.parent { display: grid; place-items: center; }

/* Method 3: Position + Transform */
.child {
  position: absolute;
  top: 50%; left: 50%;
  transform: translate(-50%, -50%);
}
```

**Memory Tip:** **Flexbox**: justify (X) + align (Y) = center. **Grid**: `place-items: center` = one-liner.

---

### Q19. What is the CSS `cascade`?

**Answer:** The cascade is the algorithm that determines which CSS rules apply when multiple rules target the same element. Order of priority:
1. Origin and importance (`!important` wins)
2. Specificity (ID > class > element)
3. Source order (last defined wins among equal specificity)

**Memory Tip:** **ISO** — **I**mportance → **S**pecificity → **O**rder.

---

### Q20. What is CSS inheritance?

**Answer:** Some CSS properties automatically pass from parent to child: `color`, `font-family`, `font-size`, `line-height`, `text-align`, `visibility`. Layout properties (`margin`, `padding`, `border`, `width`, `height`, `display`) do NOT inherit. Use `inherit` keyword to force inheritance.

**Memory Tip:** **Text/styling** properties inherit. **Box/layout** properties don't.

---

## 🟡 INTERMEDIATE (Questions 21–42)

---

### Q21. What is Flexbox?

**Answer:** Flexbox is a one-dimensional layout model for arranging items in rows or columns.
```css
.container {
  display: flex;
  justify-content: center;  /* main axis alignment */
  align-items: center;      /* cross axis alignment */
  gap: 1rem;               /* space between items */
}
```
Key container properties: `flex-direction`, `justify-content`, `align-items`, `flex-wrap`, `gap`.
Key item properties: `flex-grow`, `flex-shrink`, `flex-basis`, `align-self`.

**Memory Tip:** Flexbox = **one direction** (row OR column). Think: **content dictates layout**.

---

### Q22. What is CSS Grid?

**Answer:** Grid is a two-dimensional layout system handling rows AND columns simultaneously:
```css
.grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 1rem;
}
.item { grid-column: span 2; }
```
Key properties: `grid-template-columns/rows`, `grid-column/row`, `grid-area`, `gap`, `fr` unit.

**Memory Tip:** Grid = **two directions** (rows AND columns). Think: **layout dictates content placement**.

---

### Q23. What is the difference between Flexbox and Grid?

**Answer:**
| Feature | Flexbox | Grid |
|---------|---------|------|
| Dimension | 1D (row OR column) | 2D (rows AND columns) |
| Approach | Content-first | Layout-first |
| Best for | Navigation, toolbars, card rows | Page layouts, dashboards, galleries |
| Wrapping | `flex-wrap` | Implicit grid |
| Alignment | `justify-content`, `align-items` | `justify-items`, `align-items`, `place-items` |

**Memory Tip:** Flexbox = **flexible line**. Grid = **structured grid**. Use both together!

---

### Q24. What is a media query?

**Answer:** Media queries apply CSS based on device characteristics:
```css
/* Mobile-first approach */
.container { width: 100%; }

@media (min-width: 768px) {
  .container { width: 750px; margin: 0 auto; }
}
@media (min-width: 1024px) {
  .container { width: 980px; }
}
```
Used for responsive design. Can target: width, height, orientation, resolution, color scheme (`prefers-color-scheme`).

**Memory Tip:** Media queries = **CSS if-statements**. Mobile-first = `min-width`. Desktop-first = `max-width`.

---

### Q25. What is responsive web design (RWD)?

**Answer:** RWD makes websites adapt to different screen sizes using:
- **Fluid grids** (percentages, `fr` units)
- **Flexible images** (`max-width: 100%`)
- **Media queries** (breakpoint-specific styles)
- **Mobile-first approach** (design for small screens, enhance for larger)

```css
img { max-width: 100%; height: auto; }
```

**Memory Tip:** Responsive = **one website, all devices**. Not separate mobile/desktop sites.

---

### Q26. What is the difference between `rem` and `em` units?

**Answer:**
- **em:** Relative to the **parent element's** font-size. Can compound (nested ems multiply).
- **rem:** Relative to the **root element's** (`<html>`) font-size. Always consistent.

```css
html { font-size: 16px; }
.parent { font-size: 1.5em; }    /* 24px */
.child  { font-size: 1.5em; }    /* 36px (24 × 1.5) */
.sibling { font-size: 1.5rem; }  /* 24px (always based on root) */
```

**Memory Tip:** **R**em = **R**oot (consistent). **E**m = **E**nvironment (cascading). Prefer `rem` for predictability.

---

### Q27. What are CSS custom properties (variables)?

**Answer:**
```css
:root {
  --primary-color: #3498db;
  --font-size-base: 1rem;
}
.button {
  background: var(--primary-color);
  font-size: var(--font-size-base);
}
```
CSS variables cascade and can be overridden per-scope. Unlike preprocessor variables, they're **runtime** and can change dynamically (e.g., via JS or media queries).

**Memory Tip:** CSS variables = `--name` + `var(--name)`. Runtime, cascading, overrideable.

---

### Q28. What are CSS preprocessors?

**Answer:** Preprocessors like **Sass/SCSS**, **LESS**, and **Stylus** extend CSS with:
- Variables (`$color` in Sass)
- Nesting (child selectors inside parents)
- Mixins (reusable style blocks)
- Functions and math operations
- `@import` for modular files

They compile to standard CSS. Modern CSS is catching up with native variables, nesting, and `@layer`.

**Memory Tip:** Preprocessors = **CSS with superpowers** that compile down to plain CSS.

---

### Q29. What is the CSS `transition` property?

**Answer:** Transitions smoothly animate CSS property changes:
```css
.button {
  background: blue;
  transition: background 0.3s ease, transform 0.2s;
}
.button:hover {
  background: darkblue;
  transform: scale(1.05);
}
```
Shorthand: `transition: property duration timing-function delay;`
Timing functions: `ease`, `linear`, `ease-in`, `ease-out`, `ease-in-out`.

**Memory Tip:** Transition = **smooth gear shift**. Needs a trigger (hover, focus, class change).

---

### Q30. What are CSS animations and `@keyframes`?

**Answer:**
```css
@keyframes slideIn {
  from { transform: translateX(-100%); opacity: 0; }
  to   { transform: translateX(0); opacity: 1; }
}
.element {
  animation: slideIn 0.5s ease forwards;
}
```
Animations run automatically (no trigger needed). Properties: `animation-name`, `animation-duration`, `animation-timing-function`, `animation-delay`, `animation-iteration-count`, `animation-direction`.

**Memory Tip:** Transitions = **A to B** (trigger-based). Animations = **multi-step movie** (automatic).

---

### Q31. What are CSS transforms?

**Answer:** Transforms modify element shape/position without affecting layout:
```css
.element {
  transform: translate(50px, 30px);  /* move */
  transform: rotate(45deg);           /* rotate */
  transform: scale(1.5);              /* grow/shrink */
  transform: skew(10deg);             /* tilt */
}
```
3D transforms: `rotateX()`, `rotateY()`, `rotateZ()`, `perspective()`. Transforms are GPU-accelerated — great for animations.

**Memory Tip:** Transform = **TRSS** — Translate, Rotate, Scale, Skew. GPU-friendly.

---

### Q32. What is the `opacity` property?

**Answer:** Sets transparency from 0 (invisible) to 1 (fully visible):
```css
.element { opacity: 0.5; } /* 50% transparent */
```
Unlike `visibility: hidden`, opacity-0 elements still occupy space, receive events, and are accessible to screen readers.

**Memory Tip:** Opacity = **ghost slider**. 0 = invisible ghost, 1 = solid, 0.5 = translucent.

---

### Q33. What are CSS gradients?

**Answer:**
```css
/* Linear gradient */
background: linear-gradient(to right, #ff0000, #0000ff);

/* Radial gradient */
background: radial-gradient(circle, #ff0000, #0000ff);

/* Conic gradient */
background: conic-gradient(red, yellow, green, red);
```
You can use angles, multiple color stops, and repeating gradients (`repeating-linear-gradient`).

**Memory Tip:** Linear = **line** (direction). Radial = **radiate** (center outward). Conic = **cone** (rotation).

---

### Q34. What is the `calc()` function?

**Answer:** Performs calculations with mixed units:
```css
.sidebar { width: calc(100% - 250px); }
.element { font-size: calc(1rem + 2vw); }
```
Operators: `+`, `-`, `*`, `/`. Must have spaces around `+` and `-`.

**Memory Tip:** `calc()` = **calculator in CSS**. Mix units freely (`%` + `px` + `vw`).

---

### Q35. What is the `clip-path` property?

**Answer:** Creates clipping regions to show only parts of an element:
```css
.circle { clip-path: circle(50%); }
.triangle { clip-path: polygon(50% 0%, 0% 100%, 100% 100%); }
```
Useful for creative shapes without image editors. Can be animated for transitions.

**Memory Tip:** `clip-path` = **cookie cutter** for your elements.

---

### Q36. What is the difference between `absolute` and `relative` positioning?

**Answer:**
- **relative:** Stays in normal flow, offsets from its **own original position**. Doesn't affect siblings.
- **absolute:** Removed from normal flow entirely, positioned relative to its **nearest positioned ancestor** (any non-static parent). If none exists, relative to viewport.

`absolute` elements need a positioned parent (`relative` is commonly used as anchor).

**Memory Tip:** Relative = **shift from self**. Absolute = **escape flow, find positioned parent**.

---

### Q37. What are stacking contexts?

**Answer:** A stacking context is an isolated z-index world. Elements inside only compete with each other, not with elements outside. Created by:
- `position` (absolute/relative/fixed) + `z-index`
- `opacity` < 1
- `transform`
- `filter`
- `will-change`
- `isolation: isolate`

A child with `z-index: 9999` inside a parent with `z-index: 1` will still appear below a sibling of the parent with `z-index: 2`.

**Memory Tip:** Stacking context = **z-index sandbox**. Children can't escape their parent's z-index level.

---

### Q38. What is the CSS `filter` property?

**Answer:** Applies visual effects like image editing:
```css
.image {
  filter: blur(5px);
  filter: brightness(1.2);
  filter: contrast(1.5);
  filter: grayscale(100%);
  filter: drop-shadow(5px 5px 10px black);
}
```
Filters can be chained: `filter: blur(2px) brightness(1.1) saturate(1.5);`. Often GPU-accelerated.

**Memory Tip:** `filter` = **Instagram for CSS** (blur, brightness, contrast, grayscale, sepia, drop-shadow).

---

### Q39. What is BEM naming convention?

**Answer:** BEM (Block Element Modifier) organizes CSS class names:
```css
/* Block */
.card { }
/* Element */
.card__title { }
.card__body { }
/* Modifier */
.card--featured { }
.card__title--large { }
```
```html
<div class="card card--featured">
  <h2 class="card__title card__title--large">Title</h2>
</div>
```
Benefits: flat specificity (no nesting), self-documenting, prevents conflicts.

**Memory Tip:** BEM = **B**lock **E**lement **M**odifier. Double dash `--` = variant. Double underscore `__` = child part.

---

### Q40. What is the difference between CSS Reset and Normalize?

**Answer:**
- **CSS Reset:** Strips ALL default browser styles to a blank slate (margins, paddings, heading sizes all zeroed). You rebuild everything.
- **Normalize.css:** Preserves useful defaults and only fixes cross-browser inconsistencies. More gradual approach.

Modern preference: Start with a small reset, then add custom base styles.

**Memory Tip:** Reset = **nuclear option** (wipe everything). Normalize = **diplomat** (fix differences, keep useful defaults).

---

### Q41. How do you create a CSS-only dropdown menu?

**Answer:**
```css
.dropdown { position: relative; }
.dropdown-content { display: none; position: absolute; }
.dropdown:hover .dropdown-content { display: block; }
```
Modern approach uses `visibility` + `opacity` for smooth transitions rather than `display: none` (which can't be animated).

**Memory Tip:** Hover on parent → show child. Position absolute for overlay.

---

### Q42. What is the `currentColor` keyword?

**Answer:** `currentColor` inherits the current `color` property value:
```css
.button {
  color: blue;
  border: 2px solid currentColor; /* blue border */
  box-shadow: 0 0 5px currentColor; /* blue shadow */
}
```
Useful for SVG icons and consistent theming without extra variables.

**Memory Tip:** `currentColor` = **"use whatever color I already am"**.

---

## 🔴 ADVANCED (Questions 43–50)

---

### Q43. What are CSS Grid `auto-fill` vs `auto-fit`?

**Answer:**
```css
/* auto-fill: Creates empty columns to fill space */
grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));

/* auto-fit: Collapses empty columns, stretches items */
grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
```
Both create responsive grids without media queries. `auto-fill` keeps empty tracks; `auto-fit` collapses them, stretching existing items.

**Memory Tip:** `auto-fill` = **fill space with tracks** (may have gaps). `auto-fit` = **fit items** (stretches to fill).

---

### Q44. What are CSS Container Queries?

**Answer:** Container queries style elements based on their **container's size**, not the viewport:
```css
.card-container {
  container-type: inline-size;
}
@container (min-width: 400px) {
  .card { display: flex; }
}
```
Unlike media queries (viewport-based), container queries adapt components regardless of where they're placed. Major step for truly reusable components.

**Memory Tip:** Media queries = **room size**. Container queries = **box size** (component-level responsiveness).

---

### Q45. What is the `:has()` selector (parent selector)?

**Answer:** `:has()` selects elements based on their children — the long-requested "parent selector":
```css
/* Style card differently if it contains an image */
.card:has(img) { grid-template-columns: 1fr 2fr; }

/* Style form group when input is invalid */
.form-group:has(input:invalid) { border-color: red; }

/* Dark mode toggle */
body:has(.dark-toggle:checked) { background: #111; }
```
Supported in all modern browsers since 2024.

**Memory Tip:** `:has()` = **"parent that contains"**. CSS finally got its parent selector!

---

### Q46. What are `@layer` cascade layers?

**Answer:**
```css
@layer base, components, utilities;

@layer base {
  h1 { font-size: 2rem; }
}
@layer components {
  .card { padding: 1rem; }
}
@layer utilities {
  .hidden { display: none; }
}
```
Layers control priority — later layers override earlier ones regardless of specificity. Prevents specificity wars in large codebases.

**Memory Tip:** `@layer` = **organized drawers**. Later drawer = higher priority. Specificity only matters within a layer.

---

### Q47. What is the `clamp()` function?

**Answer:** Sets a value with min, preferred, and max bounds:
```css
h1 {
  font-size: clamp(1.5rem, 4vw, 3rem);
  /* min: 1.5rem, preferred: 4vw, max: 3rem */
}
.container {
  width: clamp(300px, 80%, 1200px);
}
```
Replaces multiple media queries for fluid typography and sizing.

**Memory Tip:** `clamp(min, preferred, max)` = **never below min, never above max, prefer middle**.

---

### Q48. What is the `will-change` property?

**Answer:** Hints to the browser about which properties will animate, allowing optimization:
```css
.element {
  will-change: transform, opacity;
}
```
Creates a compositing layer for smoother animations. **Don't overuse** — too many layers waste memory. Apply only before animation, remove after.

**Memory Tip:** `will-change` = **"heads up, browser — I'm about to animate this."** Use sparingly.

---

### Q49. How does CSS `contain` property improve performance?

**Answer:** `contain` tells the browser an element is independent, enabling rendering optimizations:
```css
.widget {
  contain: layout paint;
  /* or: contain: strict; / contain: content; */
}
```
Values: `layout` (no effect on outside layout), `paint` (nothing renders outside bounds), `size` (size doesn't depend on children), `style` (counters, quotes), `inline-size`.

`content-visibility: auto` goes further — skips rendering off-screen elements entirely.

**Memory Tip:** `contain` = **"this element is self-contained"** — browser can optimize rendering.

---

### Q50. How do you debug CSS issues?

**Answer:** Key debugging techniques:
- **Browser DevTools:** Inspect element, view computed styles, box model diagram
- **Toggle properties:** Disable/enable CSS rules in real-time
- **Outline elements:** `* { outline: 1px solid red; }` to visualize layout
- **Check specificity:** DevTools shows which rule wins and which are crossed out
- **Responsive mode:** Test different viewport sizes
- **Console:** `$0` references selected element, `getComputedStyle($0)` shows final values

**Memory Tip:** DevTools is your **X-ray vision** for CSS. Select element → see everything.

---

## 📊 Quick Reference Table

| Difficulty | Questions | Focus |
|-----------|-----------|-------|
| 🟢 Basic | Q1–Q20 | Selectors, box model, units, positioning, centering |
| 🟡 Intermediate | Q21–Q42 | Flexbox, Grid, media queries, animations, transforms |
| 🔴 Advanced | Q43–Q50 | Container queries, `:has()`, `@layer`, `clamp()`, performance |

---

*Compiled from: GeeksforGeeks, InterviewBit, CSS-Tricks, GreatFrontEnd, UpGrad, FinalRoundAI (2024–2026)*
