# JavaScript Polyfills

Polyfills are custom implementations of native JavaScript methods. They help understand how built-in methods work internally.

---

# 1. `Array.prototype.myMap()`

The `map()` method creates a **new array** by applying a callback function to every element of the original array.

## Polyfill

```javascript
Array.prototype.myMap = function (cb) {
    let arr = this;
    let res = [];

    for (let i = 0; i < arr.length; i++) {
        res.push(cb(arr[i], i, arr));
    }

    return res;
};
```

## Usage

```javascript
const arr = [1, 2, 3];

const result = arr.myMap((num) => num * 2);

console.log(result);
```

### Output

```text
[2, 4, 6]
```

### Callback Parameters

| Parameter | Description |
|-----------|-------------|
| `currentValue` | Current array element |
| `index` | Current index |
| `array` | Original array |

---

# 2. `Array.prototype.myFilter()`

The `filter()` method creates a **new array** containing only the elements that satisfy a given condition.

## Polyfill

```javascript
Array.prototype.myFilter = function (cb) {
    let arr = this;
    let res = [];

    for (let i = 0; i < arr.length; i++) {
        if (cb(arr[i], i, arr)) {
            res.push(arr[i]);
        }
    }

    return res;
};
```

## Usage

```javascript
const arr = [1, 2, 3, 4, 5];

const result = arr.myFilter((num) => num % 2 === 0);

console.log(result);
```

### Output

```text
[2, 4]
```

### Callback Parameters

| Parameter | Description |
|-----------|-------------|
| `currentValue` | Current array element |
| `index` | Current index |
| `array` | Original array |

---

# 3. `Array.prototype.myReduce()`

The `reduce()` method reduces an array into a **single value** by repeatedly applying a callback.

## Polyfill

```javascript
Array.prototype.myReduce = function (cb, init) {
    let arr = this;
    let res = init;

    for (let i = 0; i < arr.length; i++) {
        res = res !== undefined
            ? cb(res, arr[i], i, arr)
            : arr[i];
    }

    return res;
};
```

> **Note:** Use `res !== undefined` instead of `res ? ...` so values like `0`, `false`, or `""` are handled correctly.

## Usage

```javascript
const arr = [1, 2, 3];

const sum = arr.myReduce((acc, curr) => acc + curr);

console.log(sum);
```

### Output

```text
6
```

### Callback Parameters

| Parameter | Description |
|-----------|-------------|
| `accumulator` | Result from previous iteration |
| `currentValue` | Current array element |
| `index` | Current index |
| `array` | Original array |

---

# Difference Between `map()`, `filter()`, and `reduce()`

| Method | Returns | Purpose |
|--------|---------|---------|
| `map()` | New Array | Transform every element |
| `filter()` | New Array | Keep elements that satisfy a condition |
| `reduce()` | Single Value | Combine all elements into one value |

---

# Quick Revision

## `map()`

- Returns a **new array**
- Transforms every element
- Array length remains the same

## `filter()`

- Returns a **new array**
- Keeps only elements that satisfy the callback condition
- Output array may be smaller than the original

## `reduce()`

- Returns a **single value**
- Uses an **accumulator**
- Whatever the callback returns becomes the accumulator for the next iteration
- If no initial value is provided, the first array element becomes the accumulator
```
