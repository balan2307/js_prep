# Lazy Loading CSS (Serving Critical CSS)

---

## The Problem

By default, CSS **blocks rendering** — the browser won't show anything until all CSS is downloaded. For large stylesheets this kills performance.

---

## The Solution — Split CSS into Two Parts

| Part | What it contains | How to load |
|---|---|---|
| **Critical CSS** | Above-the-fold styles only | Normally (blocking is fine, it's tiny) |
| **Full CSS** | Everything else | Lazily — don't block initial render |

---

## The Trick

```html
<!-- Load critical CSS synchronously — blocks render, but it's tiny -->
<link rel="stylesheet" href="critical.css" />

<!-- Load full CSS asynchronously with low priority -->
<link rel="stylesheet" href="full.css"
  media="print"
  onload="this.media='all'" />
```

**How it works step by step:**

1. `media="print"` — browser thinks this is print-only, so it **does not block rendering**
2. CSS downloads silently in the background
3. `onload="this.media='all'"` — once downloaded, flips it to apply to all screens
4. Full CSS activates without ever having blocked the page

---

## Above the Fold vs Below the Fold

```
┌─────────────────────────┐
│                         │  ← critical.css handles this
│   ABOVE THE FOLD        │     (loaded synchronously)
│   (visible on load)     │
│                         │
├ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ┤  ← fold line
│                         │
│   BELOW THE FOLD        │  ← full.css handles this
│   (visible on scroll)   │     (loaded lazily)
│                         │
└─────────────────────────┘
```

---

## Why `media="print"` Works

Browsers download all stylesheets but only **block rendering** for stylesheets that match the current media. Since the page is not being printed:

- `media="print"` → low priority, non-blocking
- `media="all"` or no media → high priority, **blocks render**

Flipping `media` via `onload` is the key trick.

---

## Interview Cheatsheet

| Concept | One-liner |
|---|---|
| Render blocking CSS | Default behaviour — browser waits for all CSS before painting |
| Critical CSS | Only above-fold styles, loaded first, small and fast |
| `media="print"` trick | Marks CSS as non-blocking since page is not in print mode |
| `onload="this.media='all'"` | Activates the stylesheet for all screens after download |
| Below-the-fold CSS | Loads in background, applies after page is already visible |

---

## One-liner to Remember

> Critical CSS loads first and blocks intentionally — full CSS loads in the background using the `media="print"` trick and activates only after download.
