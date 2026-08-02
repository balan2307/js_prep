# React Fiber Architecture

> **React Fiber is the new reconciliation algorithm introduced in React 16.**
>
> It **did not replace the Virtual DOM**. Instead, it replaced the **old Stack Reconciler** (the algorithm that reconciled the Virtual DOM).

---

# Before React 16

The update flow looked like this:

```text
JSX
   ↓
React Elements (Virtual DOM)
   ↓
Stack Reconciler
   ↓
Diff old & new trees
   ↓
Update DOM
```

The problem wasn't the Virtual DOM.

The problem was the **Stack Reconciler**.

It used recursive function calls and completed the entire reconciliation process **synchronously**.

```text
Compare App
   ↓
Compare Navbar
   ↓
Compare Sidebar
   ↓
Compare Feed
   ↓
Compare Post1
   ↓
Compare Post2
...
```

Once React started rendering, it couldn't stop until the whole tree was finished.

If reconciliation took **40ms**, the browser couldn't:

- Paint
- Handle clicks
- Process keyboard input
- Animate

The UI appeared frozen.

---

# What changed in React 16?

React introduced **Fiber**.

The new flow became:

```text
JSX
   ↓
React Elements
   ↓
Fiber Tree
   ↓
Fiber Reconciler
   ↓
Commit
```

Instead of using recursive function calls, React now represents every component as a **Fiber Node**.

This allows React to pause, resume and prioritize rendering work.

---

# What is a Fiber Node?

A Fiber Node is simply a **JavaScript object representing one component**.

For example,

```jsx
<App>
    <Navbar />
    <Feed />
</App>
```

becomes

```text
App Fiber
    │
    ├──────── Navbar Fiber
    │
    └──────── Feed Fiber
```

Each component has its own Fiber Node.

A Fiber Node stores much more information than a React Element.

For example,

```js
{
    type,
    pendingProps,
    memoizedProps,
    memoizedState,
    child,
    sibling,
    return,
    alternate,
    flags,
    stateNode
}
```

React uses these objects while rendering.

---

# What is a Fiber Tree?

A **Fiber Tree** is simply all the Fiber Nodes connected together.

Example:

```text
App Fiber
   │
   ├──── Navbar Fiber
   │
   ├──── Sidebar Fiber
   │
   └──── Feed Fiber
             │
             ├──── Post Fiber
             ├──── Post Fiber
             └──── Post Fiber
```

Think of it as React's internal representation of your UI.

---

# Virtual DOM vs Fiber Tree

This is one of the biggest misconceptions.

Many people think React replaced the Virtual DOM.

**It didn't.**

The Virtual DOM still exists.

Fiber is simply a richer data structure used internally by React.

### React Element (Virtual DOM)

React Elements are lightweight objects.

```jsx
<div>Hello</div>
```

becomes something similar to

```js
{
    type: "div",
    props: {
        children: "Hello"
    }
}
```

It only describes **what the UI should look like**.

---

### Fiber Node

A Fiber Node stores much more information.

```text
Type
Props
State
Hooks
Parent
Child
Sibling
Priority
Effect Flags
Alternate Fiber
DOM Reference
```

React needs all this information to efficiently render, pause, resume and update components.

---

# Virtual DOM vs Fiber

| Virtual DOM | Fiber |
|-------------|-------|
| Simple description of the UI | Rich object representing a component during rendering |
| Stores element type and props | Stores props, state, hooks, priority, effects, links to other fibers, DOM reference, etc. |
| Conceptual UI tree | Internal data structure used by React |
| Existed before React 16 | Introduced in React 16 |

A good way to think about it is:

> **React Elements describe _what_ to render.**
>
> **Fiber Nodes describe _how React should render and update it_.**

---

# React Update Lifecycle

React processes every update in **3 phases**.

```text
Trigger
   ↓
Render (Reconciliation)
   ↓
Commit
```

---

# 1. Trigger Phase

An update is scheduled when something changes.

Examples:

- `setState()`
- `dispatch()`
- Parent passes new props
- Context value changes

At this point React **does not update the DOM**.

It simply schedules work.

---

# 2. Render Phase (Reconciliation)

This is the phase that **Fiber improved**.

During this phase React:

- Executes function components.
- Runs hooks.
- Builds a new **Work-In-Progress Fiber Tree**.
- Compares it with the current Fiber Tree.
- Calculates what changed.
- Marks updates that should be committed.

**No DOM updates happen here.**

---

## Why can Render freeze the UI?

The render phase is **JavaScript execution**.

JavaScript runs on the browser's main thread.

The browser uses the same thread for:

- Painting
- Mouse events
- Keyboard events
- Animations

Before Fiber:

```text
React Rendering
██████████████████████

❌ No painting
❌ No clicks
❌ No scrolling
```

Even though React was only calculating changes, those calculations could take tens of milliseconds and block the browser.

---

# How Fiber solves this

Instead of rendering everything in one go,

Fiber breaks rendering into **small units of work**.

```text
Render some components
        ↓
Pause
        ↓
Browser paints
        ↓
Resume rendering
        ↓
Pause
        ↓
Handle user input
        ↓
Continue rendering
```

This keeps the application responsive.

---

# 3. Commit Phase

Once React knows exactly what changed, it performs all DOM updates.

During commit React:

- Updates the real DOM
- Attaches refs
- Runs `useLayoutEffect`
- Browser paints
- Runs `useEffect` (after paint)

The commit phase is always **synchronous** because partially updating the DOM would leave the UI in an inconsistent state.

---

# Chef Analogy 🍳

Imagine a chef who has three responsibilities:

- Cook food
- Answer customer questions
- Take payments

## Before Fiber

```text
Cook for 30 minutes
        ↓
Answer customers
        ↓
Take payments
```

Customers wait a long time because the chef refuses to stop cooking.

This is exactly how the old Stack Reconciler worked.

---

## With Fiber

```text
Cook for 2 minutes
        ↓
Answer a customer
        ↓
Cook for 2 minutes
        ↓
Take a payment
        ↓
Continue cooking
```

The total cooking time is almost the same, but the restaurant feels responsive because the chef periodically gives attention to customers.

Fiber does the same thing.

It performs some rendering work, yields control back to the browser, and later resumes rendering where it left off.

---

# Summary

```text
User Action
      │
      ▼
Trigger
(setState / props / context)
      │
      ▼
Render Phase (Interruptible)
────────────────────────────
✓ Run components
✓ Build Work-In-Progress Fiber Tree
✓ Compare current vs new Fiber Tree
✓ Calculate changes
(No DOM updates)
────────────────────────────
      │
      ▼
Commit Phase (Synchronous)
────────────────────────────
✓ Update DOM
✓ Attach refs
✓ Run useLayoutEffect
✓ Browser paints
✓ Run useEffect
────────────────────────────
```

## Key Takeaways

- React still uses the **Virtual DOM**.
- **Fiber did not replace the Virtual DOM; it replaced the old Stack Reconciler.**
- A **Fiber Node** is a JavaScript object representing one component.
- A **Fiber Tree** is React's internal tree of Fiber Nodes.
- Fiber allows React to pause, resume and prioritize rendering work.
- Only the **Render (Reconciliation)** phase became interruptible.
- The **Commit** phase is still synchronous.
- Fiber is the foundation for Concurrent Rendering, `startTransition`, Suspense, and other modern React features.


### Virtual DOM vs Fiber

Before **React 16**, the Virtual DOM was represented as a tree of **React Elements** (plain JavaScript objects containing `type` and `props`), and the **Stack Reconciler** recursively traversed this tree synchronously to determine what had changed.

With **React 16**, React introduced the **Fiber architecture**, which **did not replace the Virtual DOM**. Instead, it replaced the **Stack Reconciler** with the **Fiber Reconciler**. During reconciliation, React builds a **Fiber Tree**, where each **Fiber Node** is a richer JavaScript object representing a component. Along with `type` and `props`, a Fiber Node stores additional information such as **state, hooks, parent/child/sibling links, update priority, effect flags, and a reference to the DOM node**.

This richer data structure allows React to **pause, resume, prioritize, and schedule rendering work**, making features like Concurrent Rendering, `startTransition`, and Suspense possible while still using the Virtual DOM as the conceptual representation of the UI.
