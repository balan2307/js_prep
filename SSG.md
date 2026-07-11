# Static Site Generation (SSG)

Static Site Generation (SSG) is a rendering strategy where HTML pages are generated **once during build time** (`npm run build`) and served as static files.

Unlike SSR, the server does **not** execute React on every request.

---

# Rendering Flow

```text
npm run build
      │
      ▼
React Components
      │
      ▼
Generate HTML Files
      │
      ▼
Deploy
      │
      ▼
User Request
      │
      ▼
Return Pre-generated HTML
```

---

# Advantages

- ✅ Extremely fast page load
- ✅ Excellent SEO
- ✅ Low server cost
- ✅ HTML served directly from CDN
- ✅ Better Core Web Vitals

---

# Limitations

- Data is fixed until the next build.
- Not suitable for frequently changing content unless using ISR.
- Rebuilding large websites can take time.

---

# Types of Static Site Generation

According to the rendering patterns, SSG can be divided into two categories.

```
Static Site Generation
│
├── With Data
│
└── Without Data
      │
      ├── No Data
      │
      └── Fetch Data on Client
```

---

# 1. SSG With Data

The data is fetched **during build time**.

The generated HTML already contains the data before deployment.

### Build Process

```text
npm run build
      │
      ▼
Fetch Products API
      │
      ▼
Generate HTML
      │
      ▼
Deploy
```

### Example

Suppose your API returns

```json
[
  {
    "id": 1,
    "name": "iPhone"
  },
  {
    "id": 2,
    "name": "MacBook"
  }
]
```

During build time, Next.js fetches this data.

```jsx
export async function getStaticProps() {
    const res = await fetch("https://api.example.com/products");
    const products = await res.json();

    return {
        props: {
            products
        }
    };
}

export default function Products({ products }) {
    return (
        <>
            {products.map(product => (
                <h2 key={product.id}>{product.name}</h2>
            ))}
        </>
    );
}
```

Generated HTML

```html
<h2>iPhone</h2>
<h2>MacBook</h2>
```

When a user visits the page, **no API request is made** because the HTML already contains the data.

### Best For

- Blog posts
- Product catalogs
- Documentation
- Marketing websites

---

# 2. SSG Without Data

Sometimes a page doesn't require data during build time.

This can be divided into two cases.

---

## Case A — No Data Required

These pages are completely static.

Examples

- About Page
- Contact Page
- Privacy Policy
- Terms & Conditions

Example

```jsx
export default function About() {
    return (
        <>
            <h1>About Us</h1>
            <p>We build amazing products.</p>
        </>
    );
}
```

Generated HTML

```html
<h1>About Us</h1>
<p>We build amazing products.</p>
```

No API calls.

No database.

Pure static HTML.

---

## Case B — Fetch Data on Client

The page itself is statically generated, but **dynamic data is fetched in the browser after the page loads**.

### Rendering Flow

```text
Build
    │
    ▼
Generate Static HTML
    │
    ▼
Deploy
    │
    ▼
User Opens Page
    │
    ▼
JavaScript Fetches API
    │
    ▼
Update UI
```

### Example

The page contains a weather widget.

```jsx
export default function Home() {
    const [weather, setWeather] = useState(null);

    useEffect(() => {
        fetch("/api/weather")
            .then(res => res.json())
            .then(setWeather);
    }, []);

    return (
        <>
            <h1>Weather App</h1>
            <p>{weather?.temperature}</p>
        </>
    );
}
```

Generated HTML

```html
<h1>Weather App</h1>
<p>Loading...</p>
```

After JavaScript loads

```text
API Request
      │
      ▼
Receive Weather Data
      │
      ▼
Update UI
```

### Best For

- Stock prices
- Weather
- Notifications
- Logged-in user information
- Live dashboards

---

# Summary

| Type | Data Source | HTML Generated | API Called in Browser |
|--------|------------|----------------|-----------------------|
| SSG With Data | Build Time | Contains Data | ❌ No |
| SSG Without Data (Static) | None | Static HTML | ❌ No |
| SSG Without Data (Client Fetch) | Client | Loading State | ✅ Yes |

---

# Interview Question

### Q. What is the difference between SSG with data and SSG without data?

**Answer:**

In **SSG with data**, the required data is fetched during the build process and embedded into the generated HTML. Users receive a fully populated page without additional API requests.

In **SSG without data**, either the page is completely static (such as an About page) or it loads dynamic data in the browser after JavaScript executes, making an API request on the client side.

---

# Real-World Examples

| Website/Page | Rendering Strategy |
|---------------|--------------------|
| About Page | SSG (No Data) |
| Privacy Policy | SSG (No Data) |
| Product Catalog | SSG (With Data) |
| Blog Posts | SSG (With Data) |
| Documentation | SSG (With Data) |
| Weather Widget | SSG + Client Fetch |
| Stock Prices | SSG + Client Fetch |
| GitHub Stars Counter | SSG + Client Fetch |

---

# Interview Tip ⭐

A common misconception is that **SSG pages cannot have dynamic content**.

This is false.

SSG generates the **initial HTML at build time**, but once the page loads, React can still fetch fresh data using `useEffect`, `fetch`, or libraries like SWR/React Query. This hybrid approach gives fast initial loading while supporting dynamic updates.
