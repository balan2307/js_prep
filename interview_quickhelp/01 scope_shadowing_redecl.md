# JavaScript: `var`, `let`, `const`, Scope, Shadowing & Redeclaration

One of the most important JavaScript interview topics. Understanding these concepts will help you answer many scope-related interview questions.

---

# 1. `var` vs `let` vs `const`

| Feature | `var` | `let` | `const` |
|----------|--------|--------|----------|
| Scope | Function Scoped | Block Scoped | Block Scoped |
| Redeclaration | ✅ Allowed | ❌ Not Allowed | ❌ Not Allowed |
| Reassignment | ✅ Allowed | ✅ Allowed | ❌ Not Allowed |
| Hoisted | ✅ Yes (`undefined`) | ✅ Yes (TDZ) | ✅ Yes (TDZ) |

## Example

```javascript
var a = 10;
var a = 20; // ✅ Allowed

let b = 10;
// let b = 20; // ❌ Error

const c = 10;
// c = 20; // ❌ Error
```

> **Note:** `const` prevents **reassignment**, not **mutation**.

```javascript
const user = {
  name: "Esakki"
};

user.name = "John"; // ✅ Allowed
```

---

# 2. Scope

Scope determines **where a variable can be accessed**.

## Global Scope

```javascript
let a = 10;

function test() {
    console.log(a);
}

test(); // 10
```

---

## Function Scope

Only `var` is function scoped.

```javascript
function test() {
    var a = 10;
}

console.log(a); // ❌ Error
```

---

## Block Scope

`let` and `const` are block scoped.

```javascript
{
    let a = 10;
}

console.log(a); // ❌ Error
```

Examples of blocks:

```javascript
if () {}

for () {}

while () {}

switch () {}

{}
```

### `var` ignores block scope

```javascript
if (true) {
    var a = 10;
}

console.log(a); // 10
```

---

# 3. Shadowing

Shadowing occurs when an inner variable has the same name as an outer variable.

The inner variable **hides** the outer one.

```javascript
let a = 100;

{
    let a = 10;
    console.log(a); // 10
}

console.log(a); // 100
```

---

Another example:

```javascript
var a = 100;

function test() {
    var a = 10;
    console.log(a);
}

test(); // 10
console.log(a); // 100
```

---

# 4. Illegal Shadowing

Illegal shadowing happens when a `var` declaration attempts to shadow a `let` or `const` in the same scope.

## ❌ Not Allowed

```javascript
let a = 10;

{
    var a = 20;
}
```

**Reason:**

`var` is function scoped, so it tries to declare itself in the surrounding function/global scope where `let a` already exists.

This results in a **SyntaxError**.

---

## ✅ Allowed

```javascript
var a = 10;

{
    let a = 20;
}

console.log(a); // 10
```

Here, `let` remains inside its block.

---

## Interview Rule

```text
✅ var can be shadowed by let

❌ let cannot be shadowed by var

❌ const cannot be shadowed by var
```

---

# 5. Redeclaration

Redeclaration means declaring the same variable **again in the same scope**.

## `var`

```javascript
var a = 10;
var a = 20;

console.log(a); // 20
```

✅ Allowed

---

## `let`

```javascript
let a = 10;
let a = 20;
```

❌ Error

---

## `const`

```javascript
const a = 10;
const a = 20;
```

❌ Error

---

## Different Scope = Allowed

```javascript
let a = 10;

{
    let a = 20;
}
```

No error because they belong to different scopes.

---

# 6. Reassignment vs Redeclaration

These two terms are commonly confused.

## Reassignment

Changing the value of an existing variable.

```javascript
let a = 10;

a = 20;
```

---

## Redeclaration

Declaring the variable again.

```javascript
let a = 10;

let a = 20;
```

---

| Operation | `var` | `let` | `const` |
|-----------|--------|--------|----------|
| Reassign | ✅ | ✅ | ❌ |
| Redeclare | ✅ | ❌ | ❌ |

---

# Interview Questions

## Question 1

Predict the output:

```javascript
var a = 10;

{
    var a = 20;
}

console.log(a);
```

### Output

```javascript
20
```

**Reason:** `var` is **not block scoped**.

---

## Question 2

Predict the output:

```javascript
let a = 10;

{
    let a = 20;
}

console.log(a);
```

### Output

```javascript
10
```

**Reason:** The inner `a` is a different variable because `let` is block scoped.

---

# Modern JavaScript Best Practices

- ✅ Use `const` by default.
- ✅ Use `let` only if the value needs to change.
- ❌ Avoid `var` in modern JavaScript because it can lead to unexpected scope-related bugs.

---

# Quick Revision

| Feature | `var` | `let` | `const` |
|----------|--------|--------|----------|
| Function Scoped | ✅ | ❌ | ❌ |
| Block Scoped | ❌ | ✅ | ✅ |
| Reassign | ✅ | ✅ | ❌ |
| Redeclare | ✅ | ❌ | ❌ |
| Hoisted | ✅ (`undefined`) | ✅ (TDZ) | ✅ (TDZ) |

---

# Shadowing Summary

## ✅ Valid Shadowing

```javascript
var -> var
let -> let
const -> const
var -> let
var -> const
```

---

## ❌ Illegal Shadowing

```javascript
let -> var
const -> var
```

---

# Cheat Sheet

```text
var
✓ Function Scoped
✓ Reassign
✓ Redeclare

let
✓ Block Scoped
✓ Reassign
✗ Redeclare

const
✓ Block Scoped
✗ Reassign
✗ Redeclare

Shadowing
✓ let -> let
✓ const -> const
✓ var -> var
✓ var shadowed by let
✓ var shadowed by const

Illegal Shadowing
✗ let shadowed by var
✗ const shadowed by var
```

---

# What's Next?

After mastering these concepts, learn:

1. Hoisting
2. Temporal Dead Zone (TDZ)
3. Execution Context
4. Lexical Environment
5. Scope Chain

These topics build directly on `var`, `let`, and `const` and are frequently asked in JavaScript interviews.
