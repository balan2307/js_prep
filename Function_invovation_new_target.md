# 📌 Understanding `new.target` in JavaScript

JavaScript provides a powerful but lesser-known feature called **`new.target`**, which helps control how functions are invoked — especially useful when writing **constructor-like functions**.

---

## 🧩 Example (with inline explanation)

```js
function MyAwesomeLibrary(config) {

    // If function is called WITHOUT `new`
    // new.target will be undefined
    if (!new.target) {
        console.log("in"); // logs once

        // Recursively call the function WITH `new`
        return new MyAwesomeLibrary(config);
    }

    // When called WITH `new`, execution comes here
    // `this` refers to a newly created object
    this.there = true;
}

// Calling WITHOUT new
var awesomeObject = MyAwesomeLibrary({ data: 'Bestest Library Ever!' });

// Final object returned
console.log(awesomeObject);

/*
Execution Flow:

1. MyAwesomeLibrary({...}) → called WITHOUT new
   → new.target === undefined
   → logs "in"
   → calls new MyAwesomeLibrary(...)

2. new MyAwesomeLibrary({...}) → called WITH new
   → new.target is defined
   → sets this.there = true
   → returns { there: true }

Final Output:
in
{ there: true }
*/
