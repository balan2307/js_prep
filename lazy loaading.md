# Image Loading: `lazy` vs `eager`

The `loading` attribute on `<img>` tells the browser **when** to fetch an image relative to the viewport.

---

## The `loading` Attribute

```html
<img src="photo.jpg" loading="lazy" alt="..." />
<img src="hero.jpg" loading="eager" alt="..." />
```

Two possible values: `lazy` and `eager`.

---

## 1. `eager` (default)

```html
<img src="hero.jpg" loading="eager" alt="Hero image" />
```

**Behaviour:**

- Image is fetched **immediately**, regardless of whether it's visible.
- Browser doesn't wait — it starts the download as soon as the tag is parsed.
- This is the default behaviour even without the attribute.

**When to use:**

- Above-the-fold images (hero banners, logos, LCP images).
- Images critical to First Contentful Paint (FCP).

---

## 2. `lazy`

```html
<img src="photo.jpg" loading="lazy" alt="Photo" />
```

**Behaviour:**

- Fetch is **deferred** until image is near the viewport.
- If user never scrolls to it, it is **never downloaded**.

**When to use:**

- Below-the-fold images, galleries, product lists.

---

## Quick Comparison

| Feature | `eager` | `lazy` |
|---|---|---|
| Fetch timing | Immediately on parse | Near viewport on scroll |
| Default behaviour | ✅ Yes | ❌ No |
| Saves bandwidth | ❌ No | ✅ Yes |
| Good for above-the-fold | ✅ Yes | ❌ No |
| Good for below-the-fold | ❌ No | ✅ Yes |

---

## LCP Warning ⚠️

Never use `loading="lazy"` on your **LCP image** — it delays the metric and hurts Core Web Vitals.

```html
<!-- ✅ correct -->
<img src="hero.jpg" loading="eager" fetchpriority="high" alt="Hero" />

<!-- ✅ correct -->
<img src="photo.jpg" loading="lazy" alt="Gallery photo" />
```

---

## `fetchpriority` — bonus attribute

| Value | Effect |
|---|---|
| `high` | Fetch before other resources |
| `low` | Fetch after more important resources |
| `auto` | Browser decides (default) |

---

## Intersection Observer API

The **JavaScript way** to implement lazy loading manually — more control than the `loading` attribute.

**How it works:**

- Watches elements and fires a callback when they enter/exit the viewport.
- You swap a `data-src` placeholder with the real `src` only when visible.

```js
const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      const img = entry.target;
      img.src = img.dataset.src;   // swap placeholder → real src
      observer.unobserve(img);     // stop watching once loaded
    }
  });
});

document.querySelectorAll('img[data-src]').forEach(img => {
  observer.observe(img);
});
```

```html
<!-- placeholder until visible -->
<img data-src="photo.jpg" src="placeholder.jpg" alt="Photo" />
```

**Key options:**

```js
new IntersectionObserver(callback, {
  root: null,        // null = viewport
  rootMargin: '200px', // start loading 200px before entering view
  threshold: 0.1     // fire when 10% of element is visible
});
```

**Why use it over `loading="lazy"`:**

- Works for any element, not just `<img>` (divs, videos, components).
- Full control over threshold and timing.
- Can trigger animations, analytics, or any side effect on visibility.

---

## Content Visibility

CSS property that **skips rendering** of off-screen content entirely — the browser doesn't paint or layout what the user can't see yet.

```css
.card {
  content-visibility: auto;
  contain-intrinsic-size: 0 300px; /* estimated height to reserve space */
}
```

**Values:**

| Value | Effect |
|---|---|
| `visible` | Normal rendering (default) |
| `hidden` | Always skips rendering (like `display:none` but keeps DOM) |
| `auto` | Skips rendering when off-screen, resumes when near viewport |

**`contain-intrinsic-size`** — tells the browser how much space to reserve for the element before it renders, preventing layout shift.

**vs `loading="lazy"`:**

| | `loading="lazy"` | `content-visibility` |
|---|---|---|
| What it skips | Network fetch | Paint + layout |
| Works on | `<img>` only | Any element |
| Saves | Bandwidth | CPU / rendering time |
| Use for | Images | Long lists, cards, sections |

---

## Interview Cheatsheet

| Concept | One-liner |
|---|---|
| `loading="eager"` | Fetch immediately, use above the fold |
| `loading="lazy"` | Defer fetch until near viewport |
| `fetchpriority` | Hint browser on fetch priority order |
| Intersection Observer | JS API to watch element visibility |
| `content-visibility` | CSS to skip rendering off-screen elements |
| `contain-intrinsic-size` | Reserve space to prevent layout shift |
