Error handling – try(), catch() and finally
Error handling is a way to manage exceptions (errors) that occur during code execution **without crashing the program**.
	Why use try-catch?
•	Prevents the program from stopping due to runtime errors.
•	Helps in debugging by showing custom error messages.
•	Improves user experience by handling failures smoothly
	How try-catch Works?
1.	The try block contains the code that might cause an error
2.	If an error occurs, throw Error(‘message’) is used to send the error to the catch() block
3.	The catch() block receives the error and handles it without stopping the whole program
4.	If no error occurs, the code inside try() runs normally, and catch is ignored


Example: 
function divide(a, b) {
  try { // Code that may cause an error
    if (b == 0) throw Error("Can't divide by 0!"); // Throwing an error
    console.log(a / b); // Code runs if no error
  } catch (error) { // Code that runs if an error occurs
    console.log(error); // Catching and displaying the error
  }
finally { // Code that always runs (optional)
    console.log("Division function executed.");
  }
}
divide(12, 0); 
// Caught Error: Can't divide by 0!
Division function executed.

`error parameter` in catch(error)
Error.name – Name/type of error (referenceError, typeError etc)
Error.message – Error description (what went wrong)
Error.stack – Tracing error location (line number + call path of the error code)

---
## Advance `object` Concepts

## Optional Chaining
`?.` Used to safely access **nested values** without getting an error if something is `undefined` or `null`

#### Key points
- Helps avoid `TypeError` when accessing deep (nested) properties.
- Returns `undefined` if value doesn't exist, instead of showing error.
- Works with objects, arrays, and functions.

#### Example
```js
const user = {
  name: "Alex",
  address: {
    city: "Mumbai"
  }
};
console.log(user.address.city);     // "Mumbai"
console.log(user.contact?.email);    // undefined (no error)
console.log(user.contact.email);     // ❌ Error
```

#### When to use?
- Accessing API data
- Handling **optional user inputs**
- Dealing with **DOM elements** that may no exist

## Object Destructuring
Object destructuring is a simple way to **extract values** from an object and store them in variables.

### ✅ Basic Syntax

```js
const person = {
  name: "Vimal",
  age: 19
};
const { name, age } = person;

console.log(name); // "Vimal"
console.log(age);  // 19
```

### Destructuring with Rest Operator
The `...rest` syntax collects the remaining properties into a new object.

```js
const object1 = {
  name: "Vimal",
  age: 19,
  country: "India"
};
const { name, ...rest } = object1;

console.log(name); // "Vimal"
console.log(rest); // { age: 19, country: "India" }
```

### Renaming Variables (Rare but useful)
We can rename the destructured variables:

```js
const { age: userAge } = object1;
console.log(userAge); // 19
```

### Nested Object Destructuring (Very Useful)
If an object has another object inside, we can directly extract inner values.

```js
const object1 = {
  completion: {
    html: "Full",
    css: "Full",
    js: "learning"
  }
};
const { completion: { html, css, js } } = object1; // Extracting values using `:{}`

console.log(completion) //Error
console.log(html); // "Full"
console.log(css);  // "Full"
console.log(js);   // "learning"
```
> **Note:**
> `completion` is not a `variable` here, only its properties (`html`, `css`, `js`) are extracted.


---
## Understanding `this` Keyword in JavaScript

### What is a Keyword?
A keyword is a **reserved word that has a special meaning in programming language.

### What is `this` in JavaScript?
`this` keyword value depends on **how a function is called**, not where it is defined. It means, the value of `this` keeps changing based on different conditions.

## Value of `this` in different conditions

### 1. Global Scope
- Calling outside `Function` or `object`
- Value of `this`: `window` object (in browser) or `global` object (in Node.js)
```js
console.log(this); // → Window object (in browser)
```
### 2. Function Scope (Regular Function)
- Calling inside a `Function`
- Value of `this`: `window` object (not bound to any `object`)
```js
function show() {
  console.log(this); // → Window object
}
show();
```

### 3. Method (Known as Implicit Binding)
- A method is a function that is either **written** as a key inside an **object** or defined inside a **class**
- Value of `this`: The `object` itself
```js
const user = {
  name: "Vimal",
  greet() {
    console.log(`Hi, I'm ${this.name}`); //Hi, I'm Vimal
  }
};
user.greet();
```

### 4. Function Inside Method (ES5)
- Value of `this`:
    - Method (function inside the object) gets `object` itself
    - Inner Method (Method inside Method) gets `window` object
```js
const obj = { // Object
  age: 25,
  fun: function () { //Object Method
    console.log(this); // → obj
    function inner() { // Method inside a Method 
      console.log(this); // → window
    }
    inner();
  }
};
obj.fun();
```

### 5. Arrow Function Inside Method (ES6)
- Value of `this`: The `object`
> **Note**:
> Arrow function inherits `this` from the parent, if its inside the method, then value of `this` is `object`, but if its outside the method, then value of `this` is `window` object
```js
const obj = {
  name: "Vimal",
  fun: function () {
    const arrow = () => {
      console.log(this.name); // → "Vimal"
    };
    arrow();
  }
};
obj.fun();
```

### 6. Constructor Function
- Value of `this`: A **new** blank `object` created by `new` keyword
```js
function Person(name, age) {
  this.name = name;
  this.age = age; // Assign values to the blank object
}
const newPerson = new Person("Vimal", 19); //Creates blank object
console.log(newPerson); // → {name: "Vimal", age: 19}
```



### 7. Event Listener
- Value of `this`: The **element** on which the event is triggered
```js
document.querySelector("button").addEventListener("click", function () {
  console.log(this); // → the button element
});
```

## Explicit Binding - `call()`, `apply()`, `bind()`
- These methods are built-in **methods** of all functions in JavaScript
- These methods are used when we want to **manually set** the value of `this`
- **Function Borrowing** is a use-case of `call`, `apply`, and `bind` where we borrow a method from one object to use with another
  
### 1. `call()`
- Calls function immediately
- let us pass arguments one by one.
- Change the value of `this` to a specific **object or function**

```js
function greet(city) {
  console.log(`${this.name} from ${city}`);
}
const person = { name: "Vimal" };
greet.call(person, "Delhi"); // → Vimal from Delhi
```

### 2. `apply()`
- Same as `call()` but passes arguments as **an array**.

```js
function greet(city) {
  console.log(`${this.name} from ${city}`);
}
const person = { name: "Vimal" };
greet.apply(person, ["Delhi"]); // → Vimal from Delhi
```

### 3. `bind()`
- Returns a new function instead of calling the function **immediately**

```js
const greetLater = greet.bind(person, "Delhi");
greetLater(); // → Vimal from Delhi
```
> **Note:**
>- **Method call** -> Implicit binding ->Automatically set `this`
>- **Function borrowing** -> Explicit binding ->Manually set `this`

---

## JavaScript Modules
JavaScript Modules help us split code into smaller, reusable files. This makes projects easier to manage, debug, and scale.

## Why Use Modules?

- Avoid messy, long single files.
- Reuse code in multiple files.
- Keep functions and variables in their own files.

## Types of Modules

### 1. ES Modules (Modern JavaScript Modules)

- Use `import` and `export` keywords.
- Supported in all modern browsers and Node.js (with `.js` or `.mjs` extension).
- Mostly used in frontend web development.

```js
// utils.js
export function greet(name) {
console.log(`Hello, ${name}`);
}
```
```js
// main.js
import { greet } from './utils.js';
greet("ThugZone");
```

### 2. CommonJS (Older Module System in Node.js)
- Uses `require()` and `module.exports`
- Stil used in many Node.js packages (especially backend tools)
- Not natively supported in browsers

```js
// utils.js
function greet(name) {
  console.log(`Hello, ${name}`);
}
module.exports = { greet };
```
```js
// main.js
const { greet } = require('./utils');
greet("ThugZone");
```

## Named Export and Default Export
### Named Export
- We can export **multiple functions or variables**
```js
// math.js
export const PI = 3.14;
export function square(x) { return x * x; }
```
```js
// main.js
import { PI, square } from './math.js';
```

### Default Export
- Export **only one value** (function, object,class)
```js
// greet.js
export default function(name) {
  console.log("Hi", name);
}
```
```js
// main.js
import greet from './greet.js';
greet("ThugZone");
```

## Dynamic Import (Lazy Loading)
- Loads Module **only when needed** (e.g on button click)
- Returns a Promise.
```js
// On demand loading
async function loadModule() {
  const module = await import('./utils.js');
  module.greet("ThugZone");
}
```
```js
// Using .then()
import('./utils.js')
  .then((mod) => mod.greet("ThugZone"))
  .catch(err => console.log("Failed to load", err));
```

## File Extensions in ES Modules
1. `.js` -> Default, works in both browser and Node.js
2. `.mjs` -> Used in Node.js if `type: module` is **not set** in package.json
3. `.ts`/`.mts` -> For TypeScript-Based modules
