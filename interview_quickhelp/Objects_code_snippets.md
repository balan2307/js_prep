# JavaScript Interview Snippets

A collection of common JavaScript interview questions with explanations.

---

# 1. Pass-by-Value vs Object References

### Code

```js
function changeAgeAndReference(person) {
  person.age = 25;

  person = {
    name: "John",
    age: 50,
  };

  return person;
}

const personObj1 = {
  name: "Alex",
  age: 30,
};

const personObj2 = changeAgeAndReference(personObj1);

console.log(personObj1);
console.log(personObj2);
```

### Output

```js
personObj1
// { name: "Alex", age: 25 }

personObj2
// { name: "John", age: 50 }
```

### Explanation

JavaScript is **pass-by-value**.

For objects, the value being passed is the **reference**.

```js
person.age = 25;
```

Mutates the original object because both `person` and `personObj1` point to the same object.

```js
person = {
  name: "John",
  age: 50,
};
```

Creates a **new object** and makes only the local variable `person` point to it.

`personObj1` still points to the original object.

### Key Takeaways

- JavaScript is **not pass-by-reference**.
- Objects are passed **by value**, where the value is a reference.
- Mutating an object affects all references.
- Reassigning a parameter only changes the local reference.

---

# 2. Default Parameters & Spread Operator

### Code

```js
const value = { number: 10 };

const multiply = (x = { ...value }) => {
  console.log((x.number *= 2));
};

multiply();
multiply(value);
multiply(value);
```

### Output

```js
20
20
40
```

### Explanation

First call:

```js
multiply();
```

Uses

```js
x = { ...value };
```

which creates a **new object**, so the original object is not modified.

Second call:

```js
multiply(value);
```

`x` points to the same object as `value`, so it becomes:

```js
value.number = 20;
```

Third call doubles it again:

```js
20 → 40
```

### Key Takeaways

- `{ ...obj }` creates a **shallow copy**.
- Passing an object shares the same reference.
- Default parameters are evaluated only when an argument isn't provided.

---

# 3. Objects as Property Keys

### Code

```js
const a = {};
const b = { key: "b" };
const c = { key: "c" };

a[b] = 123;
a[c] = 456;

console.log(a[b]);
```

### Output

```js
456
```

### Explanation

Objects cannot be property keys of normal JavaScript objects.

They are converted to strings:

```js
String(b);
// "[object Object]"

String(c);
// "[object Object]"
```

So JavaScript actually executes:

```js
a["[object Object]"] = 123;
a["[object Object]"] = 456;
```

The second assignment overwrites the first.

### Key Takeaways

- Object keys in normal objects are converted to strings.
- Only **strings** and **symbols** are valid property keys.
- Use **Map** when object identity should be preserved.

Example:

```js
const map = new Map();

map.set(b, 123);
map.set(c, 456);

console.log(map.get(b)); // 123
```

---

# 4. Rest Parameters & Spread Operator

### Code

```js
function getItems(fruitList, favoriteFruit, ...args) {
  return [...fruitList, ...args, favoriteFruit];
}

console.log(
  getItems(["banana", "apple"], "pear", "orange")
);
```

### Output

```js
["banana", "apple", "orange", "pear"]
```

### Explanation

Arguments are assigned from left to right.

```js
fruitList = ["banana", "apple"];
favoriteFruit = "pear";
args = ["orange"];
```

The returned array becomes:

```js
[
  ...fruitList,
  ...args,
  favoriteFruit
]
```

### Key Takeaways

- `...args` (Rest Parameter) collects remaining arguments.
- `...array` (Spread Operator) expands an iterable.
- Rest parameter must be the last parameter.
- Spread and rest use the same syntax (`...`) but serve different purposes.

Example:

```js
function demo(a, b, ...rest) {
  console.log(a);
  console.log(b);
  console.log(rest);
}

demo(1, 2, 3, 4, 5);
```

Output:

```js
1
2
[3, 4, 5]
```

---

# Summary

| Concept | Key Point |
|----------|-----------|
| Pass-by-Value | Objects are passed by value (the value is a reference). |
| Object Mutation | Mutating affects all references pointing to the object. |
| Reassignment | Reassigning a parameter doesn't affect the original object. |
| Spread Operator | Creates a shallow copy or expands iterables. |
| Default Parameters | Used only when an argument isn't supplied. |
| Object Keys | Objects become string keys (`"[object Object]"`). |
| Map | Preserves object identity as keys. |
| Rest Parameters | Collect remaining arguments into an array. |
| Spread vs Rest | Same syntax (`...`), different purposes. |
