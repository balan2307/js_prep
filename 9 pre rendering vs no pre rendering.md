# 🚀 Pre-rendering vs No Pre-rendering (Client-Side Rendering)

One of the most frequently asked React/Next.js interview questions.

---

# What is Pre-rendering?

**Pre-rendering** is the process of generating the HTML **before** it reaches the browser.

Instead of sending an empty HTML page, the server sends a fully rendered page so users can immediately see the content.

### Example

Instead of sending:

```html
<body>
    <div id="root"></div>
</body>
```

The server sends:

```html
<body>
    <h1>Products</h1>
    <p>Welcome to our website</p>
</body>
```

React then **hydrates** this HTML to make it interactive.

---

# What is No Pre-rendering (Client-Side Rendering - CSR)?

In **Client-Side Rendering (CSR)**, the server sends an almost empty HTML page.

```html
<body>
    <div id="root"></div>
</body>
```

The browser then:

1. Downloads the JavaScript bundle.
2. Executes React.
3. React generates the HTML.
4. Finally, the user sees the page.

---

# Rendering Flow

## Client-Side Rendering (CSR)

```text
Request
   │
   ▼
Server

<div id="root"></div>

   │
   ▼
Browser downloads JavaScript

   │
   ▼
React executes

   │
   ▼
Creates HTML

   │
   ▼
User sees the page
```

---

## Server-Side Rendering (SSR)

```text
Request
   │
   ▼
Server executes React

   │
   ▼
Creates HTML

   │
   ▼
Browser immediately displays content

   │
   ▼
Downloads JavaScript

   │
   ▼
Hydration

   │
   ▼
Interactive page
```

---

# Types of Pre-rendering

## 1. Static Site Generation (SSG)

HTML is generated **once during build time**.

```text
npm run build
        │
        ▼
React Components
        │
        ▼
Static HTML Files Generated
        │
        ▼
Deployed
```

Whenever a user visits the page, the server simply returns the already-generated HTML.

### Best For

- Blogs
- Documentation
- Portfolio websites
- Marketing pages

---

## 2. Server-Side Rendering (SSR)

HTML is generated **on every incoming request**.

```text
User Request
      │
      ▼
Server renders React
      │
      ▼
Creates HTML
      │
      ▼
Returns HTML
```

Since the page is generated for each request, it can contain fresh or personalized data.

### Best For

- E-commerce
- News websites
- Dashboards with SEO requirements
- Personalized pages

---

# What is Hydration?

Hydration is the process where React attaches JavaScript and event handlers to the already-rendered HTML.

### Before Hydration

```html
<button>Like</button>
```

The button is visible but clicking it does nothing.

### After Hydration

```jsx
<button onClick={handleLike}>
    Like
</button>
```

Now the button responds to clicks because React has attached the event handlers.

> **Hydration = Making pre-rendered HTML interactive.**

---

# CSR vs Pre-rendering

| Feature | Client-Side Rendering (CSR) | Pre-rendering (SSR/SSG) |
|----------|-----------------------------|--------------------------|
| HTML from Server | Empty | Fully Rendered |
| First Paint | Slower | Faster |
| SEO | Poor | Excellent |
| Initial User Experience | Blank screen until JS loads | Content visible immediately |
| JavaScript Required to See Content | Yes | No |
| Interactivity | After JS loads | After Hydration |
| Server Load | Low | Higher for SSR (SSG has no per-request rendering) |

---

# When Should You Use What?

## Use CSR When

- Building admin dashboards
- Internal company tools
- Highly interactive applications
- SEO is not important

---

## Use SSG When

- Blogs
- Documentation
- Portfolio websites
- Landing pages

---

## Use SSR When

- E-commerce websites
- News websites
- Product pages
- SEO is important
- Personalized pages

---

# Advantages of Pre-rendering

- ✅ Better SEO
- ✅ Faster First Contentful Paint (FCP)
- ✅ Better user experience
- ✅ Content is immediately visible
- ✅ Better Core Web Vitals

---

# Advantages of CSR

- ✅ Less work for the server
- ✅ Great for highly interactive apps
- ✅ Simpler backend architecture
- ✅ Once loaded, navigation is very fast

---

# Quick Interview Answer (30 Seconds)

> **Pre-rendering** means generating the HTML before it reaches the browser. It can happen during **build time (SSG)** or **on every request (SSR)**. Since the browser receives fully rendered HTML, users see content immediately and search engines can index the page easily. React then performs **hydration**, attaching JavaScript event handlers to make the page interactive.
>
> In **Client-Side Rendering (CSR)**, the server sends an almost empty HTML file. The browser downloads JavaScript, React renders the UI, and only then does the user see the page. CSR is ideal for dashboards and highly interactive applications but provides slower initial rendering and weaker SEO.

---

# Interview Tip ⭐

If asked:

> **"Does SSR remove the need for JavaScript?"**

**Answer:**

No.

SSR only renders the initial HTML on the server. JavaScript is still required to **hydrate** the page and make it interactive.

Without JavaScript:
- The content is visible.
- Buttons, forms, and event handlers will not work.
