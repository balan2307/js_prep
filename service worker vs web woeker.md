# Web Worker vs Service Worker

## Web Worker

A **Web Worker** runs JavaScript in a separate background thread so CPU-heavy tasks don't block the UI.

### Use Cases

- Heavy calculations
- Image/video processing
- Large data processing
- Parsing large files

### Example

```js
const worker = new Worker("worker.js");

worker.postMessage(data);

worker.onmessage = (event) => {
  console.log(event.data);
};
```

> **Web Worker → Background computation**

---

## Service Worker

A **Service Worker** runs in the background and sits between the web app and the network.

### Use Cases

- Caching
- Offline support
- Push notifications
- Background sync
- PWAs

### Registration

```js
navigator.serviceWorker.register("/sw.js");
```

### Lifecycle

```text
Register → Install → Waiting → Activate → Fetch
```

> **Service Worker → Network, caching & offline capabilities**

---

## Web Worker vs Service Worker

| Feature | Web Worker | Service Worker |
|---|---|---|
| Main purpose | Heavy computation | Network/background tasks |
| Separate thread | Yes | Yes |
| CPU-intensive tasks | Yes | No |
| Intercept requests | No | Yes |
| Caching | No | Yes |
| Offline support | No | Yes |
| Push notifications | No | Yes |
| DOM access | No | No |

## Easy Way to Remember

**Web Worker → "Do heavy work in the background."**

**Service Worker → "Handle network, cache and offline work."**
