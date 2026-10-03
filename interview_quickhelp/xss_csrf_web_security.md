# Web Security — XSS, CSRF, CSP

---

## XSS (Cross-Site Scripting)
Attacker injects malicious JS into your site that runs in other users' browsers — full access to cookies, localStorage, DOM.

### 3 Types
- **Stored** — script saved in DB, runs for every user who loads that data
- **Reflected** — script in URL, server reflects it back without sanitizing
- **DOM-based** — attacker manipulates DOM via URL, never touches server

### Prevention (3 layers)
```
Sanitization  →  escape user input, use DOMPurify for raw HTML rendering
CSP           →  even if injected, browser won't execute unauthorized scripts
httpOnly      →  even if executed, JS can't read session cookies
```
```javascript
// React escapes JSX automatically — never do this with user input
<div dangerouslySetInnerHTML={{ __html: userInput }} />

// Safe — use DOMPurify
element.innerHTML = DOMPurify.sanitize(userInput)
```
```
Set-Cookie: sessionId=abc123; httpOnly; Secure; SameSite=Strict
```

---

## CSRF (Cross-Site Request Forgery)
Attacker tricks your browser into making requests to a site you're logged into. Exploits automatic cookie attachment — browser sends cookies with every request regardless of origin.

### How it works
```html
<!-- evil.com — auto-submits on load -->
<form action="https://bank.com/transfer" method="POST">
  <input name="to" value="attacker" />
  <input name="amount" value="10000" />
</form>
<script>document.forms[0].submit()</script>
```
Browser submits → attaches bank.com cookie → bank thinks it's you → transfer happens.

### Prevention
**SameSite cookies** *(modern approach)* — browser won't attach cookie on cross-origin requests
```
Set-Cookie: sessionId=abc123; SameSite=Strict
```
- `Strict` → never sent cross-origin
- `Lax` → sent on top-level navigation, not on cross-origin forms/AJAX. Good default.

**CSRF Token** *(older approach, still common)* — secret token embedded in every request. Attacker can't read your page HTML (same-origin policy) so can never know the token.
```javascript
// forms — hidden field
<input type="hidden" name="csrf_token" value="x7f9k2m" />

// AJAX — custom header
fetch('/api/transfer', { headers: { 'X-CSRF-Token': 'x7f9k2m' } })
```
Server validates token on every POST, PUT, DELETE, PATCH — not just forms.

**Origin header check** — server rejects requests from unknown origins.

---

## CSP (Content Security Policy)
Browser whitelist — only execute scripts the server explicitly allows. Everything else blocked, even if injected onto the page.

```
Content-Security-Policy: script-src 'self'                          // same origin only
Content-Security-Policy: script-src 'self' https://cdn.example.com  // + trusted CDN
Content-Security-Policy: script-src 'nonce-abc123'                  // nonce (strongest)
```

**Nonce** — fresh random token every page load. Only scripts with matching nonce execute.
```html
<script nonce="abc123">// your code ✅</script>
<script>// attacker injected — no nonce → blocked ❌</script>
```
Visible in DevTools but single-use per request — useless to attacker.

**Common directives:** `script-src`, `style-src`, `img-src`, `connect-src`, `frame-src`, `default-src`

---

## CSRF Token vs CSP Nonce

|  | CSRF Token | CSP Nonce |
|---|---|---|
| Solves | CSRF | XSS |
| Lives in | Form field / request header | Script tag + CSP header |
| Changes | Per session | Per page load |
| Validated by | Server | Browser |
| Goal | Verify request origin | Verify script origin |

> **CSRF token** — "did this request come from my page?"
> **CSP nonce** — "did this script come from my server?"

---

## Interview One-liners

**XSS** — Injected JS in victim's browser. Fix: sanitize input + CSP blocks execution + httpOnly so cookies can't be stolen.

**CSRF** — Forged requests using automatic cookie attachment. Fix: SameSite cookies (modern) + CSRF tokens (older, validates all state-changing requests).

**CSP** — Response header whitelist. Browser blocks unauthorized scripts even if XSS injection succeeds. Strongest via nonce.

**XSS vs CSRF** — XSS runs code ON your site. CSRF makes requests TO your site using your identity.
