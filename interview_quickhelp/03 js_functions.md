# JavaScript Functions

---

# 1. Function Declaration

A function declared using the `function` keyword.

```javascript
function greet() {
    console.log("Hello");
}

greet();
```

### Characteristics

- Hoisted completely.
- Can be called before its declaration.

```javascript
greet();

function greet() {
    console.log("Hello");
}
```

---

# 2. Function Expression

A function assigned to a variable.

```javascript
const greet = function () {
    console.log("Hello");
};

greet();
```

### Characteristics

- Not fully hoisted.
- Only the variable is hoisted.
- Cannot be called before initialization.

```javascript
greet();

const greet = function () {};
```

Output

```text
ReferenceError
```

---

# Function Declaration vs Function Expression

| Function Declaration | Function Expression |
|----------------------|---------------------|
| Fully hoisted | Not fully hoisted |
| Can be called before declaration | Cannot be called before initialization |
| Has its own name | Usually anonymous |

---

# Parameters vs Arguments

## Parameters

Variables defined while declaring a function.

```javascript
function add(a, b) {
    return a + b;
}
```

Here,

```text
a, b
```

are **parameters**.

---

## Arguments

Actual values passed during function invocation.

```javascript
add(10, 20);
```

Here,

```text
10, 20
```

are **arguments**.

---

# Interview Question (Screenshot)

```javascript
var x = 21;

var fun = function () {
    console.log(x);
    var x = 20;
};

fun();
```

## Output

```text
undefined
```

### Why?

Inside the function,

```javascript
var x = 20;
```

is **hoisted**.

The function is internally interpreted as:

```javascript
var fun = function () {

    var x;          // Hoisted

    console.log(x);

    x = 20;
};
```

Execution:

```text
var x = undefined

↓

console.log(undefined)

↓

x = 20
```

Hence,

```text
undefined
```

The global `x = 21` is **shadowed** by the local `var x`.

---

# Spread Operator (`...`)

Expands an iterable into individual elements.

## Arrays

```javascript
const arr = [1, 2, 3];

console.log(...arr);
```

Output

```text
1 2 3
```

---

## Copy Array

```javascript
const arr1 = [1, 2];

const arr2 = [...arr1];
```

---

## Merge Arrays

```javascript
const a = [1, 2];
const b = [3, 4];

const c = [...a, ...b];
```

Output

```text
[1,2,3,4]
```

---

## Objects

```javascript
const user = {
    name: "John"
};

const updated = {
    ...user,
    age: 25
};
```

---

# Rest Operator (`...`)

Collects multiple values into a single array.

## Function Parameters

```javascript
function sum(...nums) {
    console.log(nums);
}

sum(1, 2, 3, 4);
```

Output

```text
[1,2,3,4]
```

---

## Destructuring

```javascript
const [a, ...rest] = [1, 2, 3, 4];
```

Output

```text
a = 1

rest = [2,3,4]
```

---

# Spread vs Rest

| Spread | Rest |
|---------|------|
| Expands values | Collects values |
| Used while calling functions or copying arrays/objects | Used in function parameters and destructuring |
| One array → Many values | Many values → One array |

---

## Example

### Spread

```javascript
const arr = [1, 2, 3];

console.log(...arr);
```

Output

```text
1 2 3
```

---

### Rest

```javascript
function show(...args) {
    console.log(args);
}

show(1, 2, 3);
```

Output

```text
[1,2,3]
```

---

# Quick Revision

### Function Declaration

- Fully hoisted
- Callable before declaration

### Function Expression

- Not fully hoisted
- Cannot be called before initialization

### Parameters

Variables in function definition.

### Arguments

Actual values passed to the function.

### Spread (`...`)

- Expands arrays/objects
- Copy arrays
- Merge arrays
- Pass array elements as arguments

### Rest (`...`)

- Collects values
- Used in function parameters
- Used in destructuring
