# 📌 JavaScript Constructor Return Behavior

This example demonstrates how **constructors behave when returning values**, especially the difference between **primitive vs object return values**.

---

## 🧩 Example (with inline explanation)

```js
function Person(name) {

    // Step 1: Assign property to `this`
    this.name = name; // { name: 'Yuvraj' }

    // Step 2: Function declaration (hoisted, but never used)
    function person() {
        return {
            name: 'Virat'
        };
    }

    // Step 3: Overwrites `person` with an object
    var person = {
        name: 'Rohit'
    };

    // Step 4: Overwrites again with a primitive
    var person = false;

    // Step 5: Return primitive
    return person;
}

var instance = new Person('Yuvraj');

console.log(instance);

/*
Execution Flow:

1. `new Person('Yuvraj')` creates a new object:
   → this = {}

2. this.name = 'Yuvraj'
   → this = { name: 'Yuvraj' }

3. `person` is overwritten multiple times:
   function → object → false

4. return false (primitive)

5. ❗ Since constructors ignore primitive returns,
   JavaScript returns `this` instead

Final Output:
{ name: 'Yuvraj' }
*/
