# WeakMap in JavaScript

## What is WeakMap?

A `WeakMap` is similar to a `Map`, but its **keys are held using weak references**.

If an object key has **no other strong references**, it becomes eligible for **Garbage Collection (GC)**, and the corresponding `WeakMap` entry is removed automatically.

---

## Why not use Map?

```js
const map = new Map();

let user = { id: 1 };

map.set(user, "Premium");

user = null;
```

The object is **NOT** garbage collected because `Map` still has a **strong reference** to it.

```text
Map
 |
 ▼
Object
```

---

## WeakMap

```js
const weakMap = new WeakMap();

let user = { id: 1 };

weakMap.set(user, "Premium");

user = null;
```

Now the object has **no strong references**.

GC removes:

- ✅ The object
- ✅ Its `WeakMap` entry

---

## WeakMap Restrictions

- Keys must be **objects**.
- Not iterable (`keys()`, `values()`, `entries()` don't exist).
- No `size` property.

---

## Common Use Cases

- Store metadata for DOM elements.
- Implement private object data.
- Cache data without causing memory leaks.

Example:

```js
const metadata = new WeakMap();

const div = document.createElement("div");

metadata.set(div, { clicked: true });
```

When `div` is garbage collected, its metadata is automatically removed.

---

## Map vs WeakMap

| Feature | Map | WeakMap |
|---------|------|----------|
| Keys | Any value | Objects only |
| Prevents GC | ✅ Yes | ❌ No |
| Iterable | ✅ Yes | ❌ No |
| `size` | ✅ Yes | ❌ No |

---

## Interview One-Liner

> A `WeakMap` stores object keys using **weak references**, allowing them to be garbage collected when no other strong references exist. It's useful for associating data with objects without causing memory leaks.
