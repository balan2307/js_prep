# Browser Rendering Basics (Interview Notes)

## Parsing vs Rendering

**Parsing** is the process where the browser reads the HTML and CSS files and builds internal data structures.

- HTML → **DOM (Document Object Model)**
- CSS → **CSSOM (CSS Object Model)**

**Rendering** is the process of taking the DOM and CSSOM and converting them into pixels displayed on the screen.

---

## Parser Blocking vs Render Blocking

### Parser Blocking

A normal `<script>` tag blocks the HTML parser because JavaScript can modify the DOM while it is being parsed.

```html
<script src="app.js"></script>
```

**Flow:**

1. Browser parses HTML.
2. Encounters `<script>`.
3. Stops parsing.
4. Downloads the script (if needed).
5. Executes the script.
6. Continues parsing HTML.

**Impact**

- Delays DOM creation.
- Delays `DOMContentLoaded`.
- Can delay first render.

---

### Render Blocking

CSS is render-blocking because the browser cannot correctly paint the page until it knows the final styles.

```html
<link rel="stylesheet" href="style.css">
```

**Flow:**

1. Browser continues parsing HTML.
2. Downloads CSS.
3. Builds CSSOM.
4. Waits to render until CSSOM is ready.

**Impact**

- DOM may already exist.
- UI is not painted until CSS is loaded.

> CSS does **not** stop HTML parsing, but it **does** delay rendering.

---

## defer Script

```html
<script src="app.js" defer></script>
```

`defer` scripts:

- Download in parallel with HTML parsing.
- Do **not** block parsing.
- Execute after the entire HTML is parsed.
- Execute before `DOMContentLoaded`.
- Execute in the order they appear.

This makes them ideal for most application JavaScript.

---

## DOMContentLoaded

`DOMContentLoaded` fires when:

- HTML has been fully parsed.
- DOM has been created.
- All deferred scripts have finished executing.

It **does not wait** for:

- Images
- Videos
- Fonts
- Most other external resources

---

## Critical Rendering Path (CRP)

The browser follows these steps to display a webpage:

```
HTML
   │
   ▼
 DOM

CSS
   │
   ▼
CSSOM

DOM + CSSOM
      │
      ▼
 Render Tree
      │
      ▼
 Layout (Reflow)
      │
      ▼
 Paint (Repaint)
      │
      ▼
 Composite
      │
      ▼
 Screen
```

### Step 1: DOM

Browser parses HTML into the DOM tree.

### Step 2: CSSOM

Browser parses CSS into the CSSOM.

### Step 3: Render Tree

Combines DOM and CSSOM into the Render Tree.

Only visible elements are included.

Example:

```html
<div>Hello</div>
<div style="display:none">Hidden</div>
```

Only the first `<div>` appears in the Render Tree.

### Step 4: Layout (Reflow)

The browser calculates:

- Position
- Width
- Height
- Margins
- Padding

for every visible element.

### Step 5: Paint (Repaint)

The browser paints:

- Colors
- Borders
- Shadows
- Backgrounds
- Text

onto different layers.

### Step 6: Composite

GPU combines layers together and displays the final pixels.

---

# Reflow (Layout)

Reflow means recalculating the size and position of elements.

### Common triggers

- Window resize
- Adding/removing DOM elements
- Changing width/height
- Font size changes
- Margin/Padding changes

Example:

```js
div.style.width = "200px";
```

Changing width affects surrounding elements, so layout must be recalculated.

**Cost:** Expensive because many elements may need repositioning.

---

# Repaint

Repaint updates only the visual appearance of elements.

Examples:

```js
div.style.color = "blue";
```

```js
div.style.background = "red";
```

Since layout doesn't change, only pixels need updating.

**Cost:** Cheaper than reflow.

---

## Page Resize

When the browser window is resized:

1. Layout (Reflow)
2. Paint (Repaint)
3. Composite

Large pages may perform many layout calculations, making resize operations expensive.

---

# Interview Summary (30–60 seconds)

**Q: Can you explain how a browser renders a webpage?**

> The browser first parses HTML into the DOM and CSS into the CSSOM. These are combined to create the Render Tree, which contains only visible elements. Next, the browser performs Layout (or Reflow) to calculate the size and position of each element, then Paint to draw colors, text, and borders. Finally, the Composite step combines painted layers and displays them on the screen.
>
> A normal `<script>` is parser-blocking because it pauses HTML parsing until the script executes. CSS is render-blocking because the browser waits for styles before painting the page. Using the `defer` attribute allows JavaScript to download in parallel without blocking parsing, and it executes after HTML parsing but before the `DOMContentLoaded` event.
>
> Reflow is more expensive than repaint because it recalculates layout, while repaint only updates visual styles without affecting layout.

---

# Quick Revision

| Concept | Meaning |
|---------|---------|
| Parsing | HTML → DOM, CSS → CSSOM |
| Rendering | Converts structures into pixels |
| Parser Blocking | Stops HTML parsing (normal `<script>`) |
| Render Blocking | Delays painting (CSS) |
| `defer` | Doesn't block parsing; executes before `DOMContentLoaded` |
| DOMContentLoaded | HTML parsed + deferred scripts executed |
| Render Tree | DOM + CSSOM (visible elements only) |
| Reflow | Recalculate layout (expensive) |
| Repaint | Update visuals only (cheaper) |
| Composite | GPU combines layers and displays the final page |
