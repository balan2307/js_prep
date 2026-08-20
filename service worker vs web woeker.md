# Web Worker vs Service Worker

## Web Worker

A Web Worker runs JavaScript in a separate background thread so CPU-intensive tasks don't block the main UI thread.

### Use cases
- Heavy calculations
- Image/video processing
- Large data processing
- Parsing large files

```js
const worker = new Worker("worker.js");

worker.postMessage(data);

worker.onmessage = (event) => {
  console.log(event.data);
};
