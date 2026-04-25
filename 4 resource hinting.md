# Resource Hinting

Resource hints tell the browser **what to fetch or connect to in advance** — before it actually needs it.

---

## 1. `preload`

Fetch this resource **now**, high priority — I will definitely need it very soon.

```html
<link rel="preload" href="font.woff2" as="font" crossorigin />
<link rel="preload" href="hero.jpg" as="image" />
<link rel="preload" href="app.js" as="script" />
```

- Forces early fetch in the **same navigation**
- Must use `as=""` to tell browser the resource type
- Does **not** execute — just downloads
- Use for: fonts, LCP images, critical JS/CSS

---

## 2. `prefetch`

Fetch this resource **when idle** — I will probably need it on the **next navigation**.

```html
<link rel="prefetch" href="next-page.js" />
<link rel="prefetch" href="dashboard.css" />
```

- Low priority, fetches in browser idle time
- Stored in cache for future use
- Use for: next page assets, route-based code splitting

---

## 3. `preconnect`

Open a connection to this origin early — **handshake now**, fetch later.

```html
<link rel="preconnect" href="https://fonts.googleapis.com" />
<link rel="preconnect" href="https://api.example.com" crossorigin />
```

- Does DNS lookup + TCP handshake + TLS negotiation upfront
- Saves ~100-500ms when you actually fetch from that origin
- Use for: third-party APIs, CDNs, font providers

---

## 4. `dns-prefetch`

Do the **DNS lookup only** — lighter version of preconnect.

```html
<link rel="dns-prefetch" href="https://fonts.googleapis.com" />
```

- Only resolves domain to IP address
- No TCP or TLS — very cheap
- Use as fallback for browsers that don't support `preconnect`
- Often used together with preconnect:

```html
<link rel="preconnect" href="https://fonts.googleapis.com" />
<link rel="dns-prefetch" href="https://fonts.googleapis.com" />
```

---

## 5. `prerender`

Fetch **and render** the entire next page in the background.

```html
<link rel="prerender" href="https://example.com/next-page" />
```

- Most aggressive hint — full page render in hidden tab
- Instant navigation if user actually goes there
- Very expensive — use only when highly confident user will navigate there
- Poor browser support, mostly Chrome

---

## 6. `modulepreload`

Like `preload` but specifically for **ES modules** — also parses and compiles the module.

```html
<link rel="modulepreload" href="app.js" />
<link rel="modulepreload" href="utils.js" />
```

- Fetches + parses + compiles the module early
- Also preloads the module's dependencies
- Use for: critical ES modules in modern apps

---

## Quick Comparison

| Hint | What it does | Priority | When to use |
|---|---|---|---|
| `preload` | Fetch now, current page | High | LCP image, critical font, key JS |
| `prefetch` | Fetch idle, next page | Low | Next route assets |
| `preconnect` | DNS + TCP + TLS now | Medium | Known third-party origins |
| `dns-prefetch` | DNS only | Very low | Fallback for preconnect |
| `prerender` | Full page render | Very high cost | Near-certain next page |
| `modulepreload` | Fetch + compile ES module | High | Critical ES modules |

---

## Interview Cheatsheet

| Hint | One-liner |
|---|---|
| `preload` | I need this now on current page — fetch immediately |
| `prefetch` | I'll need this on next page — fetch when idle |
| `preconnect` | I'll talk to this server soon — warm up the connection |
| `dns-prefetch` | Resolve this domain early — DNS only, very cheap |
| `prerender` | Render the whole next page now — expensive but instant nav |
| `modulepreload` | Preload + precompile this ES module and its deps |
