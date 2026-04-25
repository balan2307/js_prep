# Script Loading: `sync` vs `async` vs `defer`

When a browser encounters a `<script>` tag, it must decide when to **fetch** and **execute** it relative to HTML parsing. The three strategies behave very differently.

---

## 1. Sync (default)

```html
<script src="script.js"></script>
```

**Behaviour:**
- HTML parsing **pauses** immediately when the tag is encountered.
- Browser fetches and executes the script.
- Parsing resumes only after execution completes.

**Timeline:**

```
[HTML parse] → ⛔ [Fetch + Execute] → [HTML parse] → DOMContentLoaded
```

**When to use:** Almost never for external scripts. Acceptable for inline critical scripts at the top of `<head>` that must run before anything else.

---

## 2. `async`

```html
<script src="script.js" async></script>
```

**Behaviour:**
- HTML parsing **continues** while the script is fetched in parallel.
- Once the fetch completes, parsing **pauses** and the script executes immediately.
- Multiple `async` scripts execute in **fetch-completion order**, not document order.

**Timeline:**

```
[HTML parse ──────────────]
[fetch in parallel ───]
                      [Execute] → [parse] → DOMContentLoaded
```

> ⚠️ Execution timing is **unpredictable** — depends on network speed and file size.

**When to use:** Independent scripts with no DOM dependencies and no ordering requirements — e.g. analytics, ads, third-party widgets.

---

## 3. `defer`

```html
<script src="script.js" defer></script>
```

**Behaviour:**
- HTML parsing **continues** while the script is fetched in parallel.
- Script executes only **after** HTML parsing is fully complete.
- Multiple `defer` scripts execute in **document order**, guaranteed.
- Runs just before `DOMContentLoaded` fires.

**Timeline:**

```
[HTML parse ─────────────────────]
[fetch in parallel ─────────]
                              [Execute] → DOMContentLoaded
```

**When to use:** Most external scripts that interact with the DOM. This is the safest default for most use cases.

---

## Quick Comparison

| Feature | Sync | `async` | `defer` |
|---|---|---|---|
| Blocks HTML parsing | ✅ Yes | ⚠️ At execution | ❌ Never |
| Fetched in parallel | ❌ No | ✅ Yes | ✅ Yes |
| Execution order guaranteed | ✅ Yes | ❌ No | ✅ Yes |
| Runs before DOM ready | ✅ Yes | ⚠️ Maybe | ❌ No |
| Safe for DOM manipulation | ⚠️ Risky | ❌ No | ✅ Yes |

---

## Decision Guide

```
Does your script depend on the DOM?
├── Yes → use defer (or move <script> to bottom of <body>)
└── No → Is script order important?
         ├── Yes → use defer
         └── No  → use async (analytics, widgets, ads)
```

---

## Placement Tip

`defer` and `async` are only meaningful for **external scripts** (`src="..."`). On **inline scripts**, both attributes are ignored.

```html
<!-- ✅ Works as expected -->
<script src="app.js" defer></script>

<!-- ❌ defer has no effect here -->
<script defer>
  console.log('this runs synchronously anyway');
</script>
```

---

## In a Nutshell

- **Sync** → blocks everything, use sparingly
- **`async`** → fast but chaotic, for isolated scripts
- **`defer`** → fast and orderly, the safe default for most scripts
