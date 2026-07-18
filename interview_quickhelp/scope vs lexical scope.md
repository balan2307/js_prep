# JavaScript Scope & Lexical Scope

## What is Scope?

**Scope** determines **where a variable can be accessed** in your program.

### Types of Scope

### 1. Global Scope

Variables declared outside all functions and blocks.

```javascript
let a = 10;

function test() {
    console.log(a); // 10
}
```

---

### 2. Function Scope

Variables declared with `var` are accessible only inside the function.

```javascript
function test() {
    var x = 5;
}

console.log(x); // ❌ ReferenceError
```

---

### 3. Block Scope

Variables declared with `let` and `const` are accessible only inside the block.

```javascript
{
    let x = 5;
}

console.log(x); // ❌ ReferenceError
```

Blocks include:

- `if`
- `for`
- `while`
- `switch`
- `{}`

---

## What is Lexical Scope?

**Lexical Scope** means a function's scope is determined by **where it is defined (written)**, **not where it is called**.

### Example

```javascript
let city = "Mumbai";

function printCity() {
    console.log(city);
}

function another() {
    let city = "Delhi";
    printCity();
}

another();
```

**Output**

```text
Mumbai
```

`printCity()` was defined in the global scope, so it always looks for `city` in its lexical (surrounding) environment.

---

## Scope Chain

When JavaScript cannot find a variable in the current scope, it searches:

```text
Current Scope
      ↓
Parent Scope
      ↓
Global Scope
      ↓
ReferenceError
```

### Example

```javascript
let a = 1;

function outer() {
    let b = 2;

    function inner() {
        console.log(a);
        console.log(b);
    }

    inner();
}

outer();
```

**Output**

```text
1
2
```

`inner()` searches:
1. Its own scope
2. `outer()`'s scope
3. Global scope

---

## Key Differences

| Scope | Lexical Scope |
|--------|---------------|
| Defines where variables are accessible. | Determines **which scope a function belongs to**. |
| Created by global, function, and block. | Decided by **where the function is written**. |
| Controls variable visibility. | Controls how variables are resolved using the scope chain. |

---

## Quick Revision

- ✅ **Scope** → Where a variable can be accessed.
- ✅ **Lexical Scope** → Determined by where a function is defined.
- ✅ JavaScript uses **Lexical Scope**, **not Dynamic Scope**.
- ✅ Variable lookup happens through the **Scope Chain** (Current → Parent → Global).
