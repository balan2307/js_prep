# CSS Optimization

---

## Why CSS Blocks Rendering

CSS is render-blocking — browser won't paint anything until ALL linked CSS files are downloaded AND parsed.

```
base.css       ██ done (0.1s)
components.css ████████ done (0.5s)
animations.css ████████████████████ done (2s)
                                    ↓ PAINT (blank for 2s)
```

With multiple CSS files, the **slowest one holds everything hostage**.

Two steps that block rendering:
- **Download** → get bytes from server (network — slow)
- **Parse** → browser reads every CSS rule (CPU — usually too fast to notice)

Both must complete before browser paints a single pixel.

---

## 1. Critical CSS — Most Important Fix

Inline only the CSS needed for above-the-fold content. Browser paints immediately — no waiting.

```html
<head>
  <style>
    /* Only styles user sees without scrolling */
    body { margin: 0; font-family: sans-serif; }
    nav  { display: flex; background: #fff; }
    h1   { font-size: 2rem; color: #111; }
    .hero { height: 100vh; display: grid; place-items: center; }
  </style>
</head>
```

The more you cover here → less flash when full CSS loads later.

Tools to auto-extract critical CSS:
```bash
npm install -g critical
critical index.html --inline --base ./ > index-optimized.html
```

---

## 2. Preload + onload Swap — Best Modern Approach

Downloads CSS at high priority without blocking paint. Applies it once downloaded.

```html
<link rel="preload" href="style.css" as="style"
      onload="this.onload=null; this.rel='stylesheet'">

<noscript><link rel="stylesheet" href="style.css"></noscript>
```

How each part works:

| Part | What it does |
|---|---|
| `rel="preload"` | Downloads file immediately at high priority but does NOT apply it |
| `as="style"` | Tells browser what type of file — ensures correct priority |
| `onload` fires | Download done → switches `rel` to `stylesheet` → CSS applies |
| `this.onload=null` | Prevents infinite loop (changing rel would retrigger onload) |
| `<noscript>` | Fallback for users with JS disabled |

Timeline:
```
Without preload:
[blank screen while CSS downloads] → paint

With preload + onload:
[inline critical CSS paints immediately] ✅
[full CSS downloads in background]
[onload fires → full CSS applies] ✅
```

Does it cause a flash? Yes — but it's a **style enhancement flash**, not a broken layout flash. Inline critical CSS already covers the visible area so user never sees blank or broken content.

---

## 3. Media Attribute — Non-Blocking by Mismatch

If `media` doesn't match current condition → browser downloads at **low priority**, doesn't block paint.

```html
<!-- Never blocks normal browsing — only applies when printing -->
<link href="print.css" rel="stylesheet" media="print">

<!-- Only blocks if device is actually in portrait -->
<link href="portrait.css" rel="stylesheet" media="(orientation:portrait)">

<!-- Trick: force ANY css to load non-blocking -->
<link href="main.css" rel="stylesheet" media="print"
      onload="this.media='all'">
```

The trick — deliberately set a `media` value that won't match current condition so browser downloads silently. Then `onload` switches it to `all` to apply everywhere.

**Why preload is better than media trick:**
- `media="print"` → downloads at LOW priority → might load late
- `rel="preload"` → downloads at HIGH priority → loads faster

---

## 4. JS Dynamic Injection — Legacy Approach

Create `<link>` tag with JS at bottom of body. By then page is already painted.

```html
<!-- At bottom of <body> -->
<script>
  (function() {
    var link = document.createElement('link');
    link.rel  = 'stylesheet';
    link.href = '/css/main.css';
    document.head.appendChild(link);
  })();
</script>
```

Why it works — script at bottom runs after HTML is parsed and initial paint is done. CSS loads without blocking first render.

**Problem** — causes FOUC (Flash of Unstyled Content). Page shows unstyled first, then styles pop in. Use preload+onload instead.

---

## Comparison

| Technique | Blocks Paint? | Priority | FOUC? | Use |
|---|---|---|---|---|
| Normal `<link>` | ✅ Yes | High | No | ❌ Avoid |
| Inline critical CSS | No | Instant | No | ✅ Always |
| Preload + onload | No | High | Slight | ✅ Best |
| Media mismatch trick | No | Low | Slight | ⚠️ OK |
| JS injection | No | Low | Yes | ❌ Legacy |

---

## Full Optimized CSS Setup

```html
<head>
  <!-- Step 1: inline critical → instant paint, no blank screen -->
  <style>
    body { margin: 0; font-family: sans-serif; }
    nav  { display: flex; background: #fff; }
    h1   { font-size: 2rem; }
    .hero { height: 100vh; display: grid; place-items: center; }
  </style>

  <!-- Step 2: full CSS loads async → no blocking -->
  <link rel="preload" href="main.css" as="style"
        onload="this.onload=null;this.rel='stylesheet'">

  <!-- Step 3: context specific → non blocking -->
  <link href="print.css" rel="stylesheet" media="print">

  <!-- Step 4: fallback if JS disabled -->
  <noscript><link rel="stylesheet" href="main.css"></noscript>
</head>
```

What each step solves:
```
Step 1 → no blank screen (user sees content immediately)
Step 2 → full styles load without blocking (best of both worlds)
Step 3 → print/portrait CSS loads silently (no wasted priority)
Step 4 → safety net for JS disabled users
```
