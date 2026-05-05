# JavaScript `this` Cheat Sheet

## 🔹 Core Rule
`this` is decided by **how a function is called**, not where it is written.

---

## 🔹 1. Normal Function (call-site based)

const obj = {
  name: 'Yuvraj',
  greet: function () {
    console.log(this.name);
  }
};

obj.greet(); // Yuvraj

const fn = obj.greet;
fn(); // undefined (this lost)

---

## 🔹 2. Arrow Function (lexical this)

const obj = {
  name: 'Yuvraj',
  greet() {
    const inner = () => {
      console.log(this.name);
    };
    inner();
  }
};

obj.greet(); // Yuvraj

---

## 🔹 3. Arrow vs Normal in Object

const obj = {
  name: 'Yuvraj',

  normal: function () {
    console.log(this.name);
  },

  arrow: () => {
    console.log(this.name);
  }
};

obj.normal(); // Yuvraj
obj.arrow();  // undefined

---

## 🔹 4. "this gets lost"

const obj = {
  name: 'Yuvraj',
  greet() {
    function inner() {
      console.log(this.name);
    }
    inner();
  }
};

obj.greet(); // undefined

---

## 🔹 5. Fix with bind

const obj = {
  name: 'Yuvraj',
  greet() {
    function inner() {
      console.log(this.name);
    }
    inner.bind(this)();
  }
};

obj.greet(); // Yuvraj

---

## 🔹 6. call / apply

function greet() {
  console.log(this.name);
}

const obj = { name: 'Yuvraj' };

greet.call(obj);   // Yuvraj
greet.apply(obj);  // Yuvraj

---

## 🔹 7. apply with args (EventEmitter concept)

function greet(a, b) {
  console.log(this.name, a, b);
}

const obj = { name: 'Yuvraj' };

greet.apply(obj, [1, 2]); // Yuvraj 1 2

---

## 🔹 8. Arrow ignores call/apply/bind

const obj = { name: 'Yuvraj' };

const fn = () => {
  console.log(this.name);
};

fn.call(obj); // undefined

---

## 🔥 Key Takeaways

- Normal function → `this` depends on how it's called
- Arrow function → `this` is inherited from parent scope
- `this` gets lost when a method is called without its object
- Use `bind`, `call`, or arrow functions to fix `this`

---
