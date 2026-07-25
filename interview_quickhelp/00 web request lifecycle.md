# Web Request Lifecycle: From URL to Rendered Page

A breakdown of what happens between typing a URL and seeing a rendered webpage — covering the network phase, TCP/TLS handshakes, and the browser's rendering pipeline. Useful as frontend interview prep.

---

## 1. Overview

The journey has two major phases:

1. **Network phase** — getting the bytes from server to browser
2. **Rendering phase** — turning those bytes into pixels on screen

---

## 2. Network Phase

### 2.1 URL Parsing
Browser parses the scheme, host, port, path, and query string from the input.

### 2.2 DNS Resolution
Hostname → IP address, checked in order:
- Browser cache
- OS cache
- Router / ISP resolver
- Recursive lookup (root → TLD → authoritative nameserver) if not cached

### 2.3 TCP Handshake (3-way)

TCP turns unreliable IP packets into a reliable, ordered, bidirectional byte stream.

| Step | Message | Purpose |
|------|---------|---------|
| 1 | **SYN** | Client proposes an Initial Sequence Number (ISN) |
| 2 | **SYN-ACK** | Server acknowledges client's ISN, sends its own ISN |
| 3 | **ACK** | Client acknowledges server's ISN — connection established |

**Why 3 steps, not 2?**
- Confirms **both directions** work (client→server *and* server→client), not just one.
- Agrees on **sequence numbers** for ordering, duplicate detection, and loss detection.
- Prevents servers from committing resources to unverified clients (mitigates SYN flood attacks).

Cost: **1 round trip** before any data is sent.

### 2.4 TLS Handshake (if HTTPS)

TCP guarantees reliable delivery — but not privacy, integrity, or identity. TLS adds:

- **Confidentiality** — encrypts payload so it can't be read in transit
- **Integrity** — a MAC detects tampering
- **Authentication** — certificate chain (verified against a trusted CA) proves the server is who it claims to be, preventing man-in-the-middle attacks

| Version | Round Trips | Notes |
|---------|-------------|-------|
| TLS 1.2 | 2 | ClientHello → ServerHello + cert → key exchange → Finished |
| TLS 1.3 | 1 | Client sends key share in first message; supports 0-RTT resumption |

### 2.5 HTTP Request/Response
- Browser sends method, headers (`Accept`, `Cookie`, `User-Agent`), possibly via a CDN edge node.
- Server (or cache) processes and returns status code, headers (`Content-Type`, `Cache-Control`), and body.

---

## 3. Rendering Phase (Critical Rendering Path)

| Step | What Happens |
|------|--------------|
| **HTML → DOM** | HTML is streamed and tokenized into the DOM tree |
| **CSS → CSSOM** | CSS is parsed into CSSOM. **Render-blocking** — nothing paints until this is ready |
| **JS execution** | Normal `<script>` tags are **parser-blocking**. `async` = parallel download, execute whenever ready. `defer` = parallel download, execute in order after parsing |
| **Render Tree** | DOM + CSSOM merged; only visible nodes included (`display:none` excluded, `visibility:hidden` included) |
| **Layout (Reflow)** | Computes exact position/size of every node |
| **Paint** | Fills in pixels — text, color, shadows |
| **Composite** | Compositor thread (often GPU) combines layers into final image. `transform`/`opacity` animations can skip layout & paint here |

**Lifecycle events:**
- `DOMContentLoaded` — fires once HTML/DOM is parsed
- `load` — fires once all resources (images, styles, etc.) finish loading

---

## 4. Performance Implications (Frontend-Relevant)

- Put CSS in `<head>`; use `defer`/`async` for scripts
- `preconnect` / `preload` for critical origins and resources
- Minimize layout thrashing — batch DOM reads/writes
- Prefer `transform`/`opacity` for animations (composite-only)
- Code-split and lazy-load to reduce initial JS payload
- Track Core Web Vitals: **LCP**, **CLS**, **INP**

---

## 5. Quick Interview Answer

> "A URL request goes through DNS resolution, a TCP handshake, and (for HTTPS) a TLS handshake — that's the network phase. Then the browser builds the DOM and CSSOM from the response, combines them into a render tree, computes layout, paints pixels, and composites layers onto the screen. As a frontend engineer, the parts I control are mostly in the rendering path and resource-loading strategy — that's where most real-world optimization happens."
