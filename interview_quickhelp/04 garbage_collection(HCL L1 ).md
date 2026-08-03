# JavaScript Memory Management & Garbage Collection

## Memory Allocation

JavaScript primarily uses **Stack** and **Heap** memory.

### Stack Memory

Stores:
- Primitive values (`number`, `string`, `boolean`, `null`, `undefined`, `symbol`, `bigint`)
- Function call frames
- References (memory addresses) to objects stored in the heap

Example:

```js
let age = 25;
let name = "John";
```

```text
Stack

age  -> 25
name -> "John"
```

---

### Heap Memory

Stores:
- Objects
- Arrays
- Functions
- Dates, Maps, Sets, etc.

Example:

```js
let user = {
  name: "John",
};
```

```text
Stack                     Heap

user ---------------> { name: "John" }
```

> **Note:** The variable `user` is stored in the stack, but the actual object lives in the heap.

---

# Garbage Collection (GC)

Garbage Collection is JavaScript's automatic memory management process. It frees memory occupied by objects that are **no longer reachable**.

---

## How GC Works (Mark-and-Sweep)

The garbage collector starts from **Root Objects**, such as:

- Global variables
- Local variables in the current call stack
- Internal browser/engine references

It then performs two phases:

### 1. Mark Phase

Starting from the roots, it marks every reachable object.

### 2. Sweep Phase

Any object that was **not marked** is considered unreachable and its memory is reclaimed.

---

## Example

```js
let a = {
  child: {
    child: {
      value: 1,
    },
  },
};
```

Memory:

```text
Global (Root)
   |
   a
   |
   A ---> B ---> C
```

GC starts from the root (`a`):

- ✅ Marks A
- ✅ Marks B
- ✅ Marks C

Since all are reachable, **nothing is garbage collected.**

---

## Unreachable Objects

```text
Global (Root)
   |
   a
   |
   A ---> B ---> C

D ---> E
```

Here:

- `A`, `B`, and `C` are connected to a root.
- `D` and `E` are **not connected to any root**.

During GC:

- ✅ A, B, C are marked.
- ❌ D, E remain unmarked.

Sweep Phase:

```text
Delete D
Delete E
```

---

## Circular References

Objects referencing each other **do not cause memory leaks**.

```js
let obj1 = {};
let obj2 = {};

obj1.ref = obj2;
obj2.ref = obj1;

obj1 = null;
obj2 = null;
```

Although the objects reference each other, **no root can reach them**, so both are garbage collected.

---

## Memory Leak

A memory leak happens when an object is **still reachable** even though the application no longer needs it.

Common causes:

- Global variables
- Unremoved event listeners
- Uncleared `setInterval`
- Growing caches
- Closures holding unnecessary references

---

## Interview One-Liner

> JavaScript stores primitive values and object references on the **stack**, while actual objects are stored in the **heap**. Garbage Collection uses the **Mark-and-Sweep** algorithm: it starts from root objects, marks everything reachable, and removes anything unreachable. An object is kept alive only if it is **reachable from a root**, not simply because another object references it.
