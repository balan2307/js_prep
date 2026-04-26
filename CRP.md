## Browser Rendering Basics (CRP, Blocking, Reflow, Repaint)

### 🔹 Parsing vs Rendering

* **Parsing** = Browser reads HTML/CSS and builds structures

  * HTML → **DOM**
  * CSS → **CSSOM**
* **Rendering** = Converting DOM + CSSOM into pixels on screen

---

### 🔹 Parser Blocking vs Render Blocking

**Parser Blocking (e.g. normal `<script>`):**

* Stops HTML parsing
* DOM is not fully built
* Page may render partially

```html
<script src="app.js"></script>
```

---

**Render Blocking (e.g. CSS):**

* DOM is built but not shown
* Browser waits before painting UI

```html
<link rel="stylesheet" href="style.css">
```

---

### 🔹 defer Script Behavior

```html
<script src="app.js" defer></script>
```

* Does NOT block parsing
* Executes after HTML parsing is complete
* Runs **before `DOMContentLoaded`**
* Safe to access DOM

---

### 🔹 DOMContentLoaded

* Event fired when:

  * HTML is fully parsed
  * DOM is ready
  * All `defer` scripts executed
* Does NOT wait for images/CSS

---

### 🔹 Critical Rendering Path (CRP)

Steps browser follows to render UI:

1. HTML → DOM
2. CSS → CSSOM
3. DOM + CSSOM → Render Tree
4. **Layout (Reflow)**
5. **Paint (Repaint)**
6. Composite (final pixels)

---

### 🔴 Reflow (Layout)

* Recalculates size & position of elements
* Triggered by:

  * Resize
  * DOM changes
  * width/height changes

```js
div.style.width = "200px"; // triggers reflow
```

* Expensive (affects layout)

---

### 🟡 Repaint

* Updates visual styles only
* No layout change

```js
div.style.color = "blue"; // repaint only
```

* Cheaper than reflow

---

### 🔹 Page Resize Behavior

* Triggers **reflow → repaint → composite**
* Layout recalculated for all affected elements
* Can be expensive if frequent

---

### 🔹 Key Takeaways

* Parser-blocking → stops DOM building
* Render-blocking → delays UI display
* `defer` → best for DOM-safe scripts
* Reflow → layout change (expensive)
* Repaint → visual change (cheaper)

---
