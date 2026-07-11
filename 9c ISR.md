# Incremental Static Regeneration (ISR)

Incremental Static Regeneration (ISR) is a rendering strategy that combines the speed of **Static Site Generation (SSG)** with the ability to keep pages **up-to-date without rebuilding the entire application**.

Instead of regenerating all pages during every deployment, ISR allows **individual pages** to be regenerated either:

- Automatically after a specified time (**Auto Revalidation**)
- Manually whenever content changes (**On-Demand Revalidation**)

---

# Why Do We Need ISR?

Imagine an e-commerce website with **100,000 product pages**.

### Using SSG

Whenever a single product changes,

```text
Product Price Updated
        │
        ▼
Run npm build
        │
        ▼
Regenerate all 100,000 pages
        │
        ▼
Deploy Again
```

This is expensive and time-consuming.

---

### Using ISR

Only the affected page gets regenerated.

```text
Product Price Updated
        │
        ▼
Regenerate only Product Page
        │
        ▼
Serve Updated HTML
```

No full rebuild required.

---

# Rendering Flow

```text
User Request
      │
      ▼
Serve Existing Static HTML
      │
      ▼
Check Revalidation Rule
      │
      ▼
Regenerate Page (if needed)
      │
      ▼
Cache Updated HTML
```

---

# Types of ISR

```
Incremental Static Regeneration
│
├── Auto Revalidation
│
└── On-Demand Revalidation
```

---

# 1. Auto Revalidation

The page is regenerated **automatically after a specified time interval**.

Example:

```jsx
export async function getStaticProps() {
    const res = await fetch("https://api.example.com/products");
    const products = await res.json();

    return {
        props: {
            products,
        },
        revalidate: 60,
    };
}
```

Here,

```text
revalidate: 60
```

means:

- Generate page during build.
- Cache it.
- After **60 seconds**, the next request triggers regeneration.
- Users continue seeing the cached page while regeneration happens in the background.
- Once complete, future visitors receive the updated page.

---

## Auto Revalidation Flow

```text
Build
 │
 ▼
Generate HTML

 │
 ▼
User visits page

 │
 ▼
Serve Cached HTML

 │
 ▼
Has 60 seconds passed?

      │
 ┌────┴─────┐
 │          │
No         Yes
 │          │
 ▼          ▼
Serve      Regenerate
Cache      in Background
               │
               ▼
         Update Cache
```

---

## Real Example

A shopping website updates product prices every few minutes.

```text
Product Page

Price: ₹79,999
```

After 60 seconds,

```text
Database Price Changed

↓

ISR regenerates page

↓

Visitors now see

Price: ₹74,999
```

No deployment required.

---

## Best Use Cases

- Product catalog
- News articles
- Blog posts
- Documentation
- Cryptocurrency prices (updated periodically)

---

# 2. On-Demand Revalidation

Instead of waiting for a timer, the page is regenerated **immediately whenever content changes**.

For example,

A CMS (Content Management System) publishes a new blog.

Instead of waiting 60 seconds,

the CMS tells Next.js:

> "Regenerate this page now."

---

## Flow

```text
Editor Updates Blog

      │
      ▼
CMS Sends Webhook

      │
      ▼
Next.js Revalidates Page

      │
      ▼
New HTML Generated

      │
      ▼
Cache Updated
```

---

## Example

Suppose an admin edits a blog article.

```text
Old Title

Understanding React
```

Admin changes it to

```text
Mastering React 19
```

CMS triggers:

```text
POST /api/revalidate
```

Next.js regenerates only that page.

Visitors immediately see

```text
Mastering React 19
```

without rebuilding the website.

---

## Example API Route (Pages Router)

```jsx
export default async function handler(req, res) {
    await res.revalidate("/blog/react");

    return res.json({
        revalidated: true,
    });
}
```

---

## Best Use Cases

- CMS websites
- Blog platforms
- News portals
- Documentation websites
- E-commerce admin updates

---

# Auto Revalidation vs On-Demand Revalidation

| Feature | Auto Revalidation | On-Demand Revalidation |
|----------|-------------------|------------------------|
| Trigger | Time Interval | Manual Trigger/Webhook |
| Uses `revalidate` | ✅ Yes | ❌ No |
| Immediate Update | ❌ No | ✅ Yes |
| Best For | Frequently changing data | CMS/Admin updates |

---

# SSG vs ISR

| Feature | SSG | ISR |
|----------|-----|-----|
| HTML Generated | Build Time | Build Time + Later Regeneration |
| Requires Rebuild | ✅ Yes | ❌ No |
| Fresh Data | Only after deployment | Automatically updated |
| Performance | Excellent | Excellent |
| SEO | Excellent | Excellent |

---

# When Should You Use ISR?

Use ISR when:

- The page should remain fast like SSG.
- Content changes occasionally.
- You don't want to rebuild the entire website.
- SEO is important.
- Large websites contain thousands of pages.

---

# Real-World Examples

| Website/Page | Why ISR? |
|---------------|----------|
| Amazon Product Page | Price and stock change frequently |
| Flipkart Product Page | Inventory updates |
| Medium Articles | New edits should appear |
| Company Blog | New posts are published regularly |
| Documentation Website | Pages change occasionally |

---

# Interview Questions

### Q. Why use ISR instead of SSG?

**Answer:**

SSG requires rebuilding the entire application whenever content changes. ISR regenerates only the pages that need updating, making deployments much faster while keeping the content fresh.

---

### Q. Does ISR affect SEO?

**Answer:**

No.

ISR serves fully rendered HTML just like SSG, so search engines receive complete HTML pages. Therefore, ISR provides excellent SEO.

---

### Q. What happens during Auto Revalidation?

**Answer:**

When the revalidation interval expires, the **first request** after expiration still receives the cached page. In the background, Next.js regenerates the page. Once regeneration succeeds, the cache is updated, and all subsequent visitors receive the new HTML.

---

### Q. What is the difference between Auto Revalidation and On-Demand Revalidation?

| Auto Revalidation | On-Demand Revalidation |
|-------------------|------------------------|
| Runs after a fixed time | Runs immediately when triggered |
| Uses `revalidate` property | Uses API/Webhook |
| Suitable for periodic updates | Suitable for CMS/Admin updates |

---

# Interview Tip ⭐

Think of ISR as:

> **SSG + Automatic Page Refresh**

- **SSG** generates pages once during build.
- **ISR** generates pages once during build **and regenerates only the pages that need updating**, either after a time interval or when explicitly triggered.

This makes ISR one of the best rendering strategies for **large, SEO-friendly applications with changing content**.


# When Should You Use ISR?

Use ISR when:

- The page should remain fast like SSG.
- Content changes occasionally.
- You don't want to rebuild the entire website.
- SEO is important.
- Large websites contain thousands of pages.

### Why is ISR commonly used in E-commerce?

At first glance, it may seem that ISR isn't suitable for e-commerce because **it doesn't dynamically update pages already open in the user's browser**.

However, most product information changes **infrequently**, such as:

- Product Name
- Description
- Images
- Specifications

These are excellent candidates for ISR because they benefit from:

- Fast page loads
- Excellent SEO
- Low server and database load

Data that changes more frequently includes:

- Price
- Stock Availability
- Delivery Estimate
- Personalized Offers

For this type of data, e-commerce applications usually fetch fresh information on the client or use SSR when necessary.

### Typical Rendering Strategy

```text
Product Page
│
├── Product Name          → ISR
├── Images                → ISR
├── Description           → ISR
├── Specifications        → ISR
├── Price                 → Client Fetch / ISR
├── Stock Availability    → Client Fetch
├── Delivery Estimate     → Client Fetch
└── Recommended Products  → Client Fetch
```

This hybrid approach provides:

- ✅ Fast page loads
- ✅ Excellent SEO
- ✅ Reduced server and database load
- ✅ Fresh data where it actually matters

> **Note:** ISR updates the **server's cached HTML**, not the page already open in the user's browser. Users who already have the page open will continue seeing the existing content until they refresh, navigate again, or the application fetches fresh data on the client.
