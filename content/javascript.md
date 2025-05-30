◀️ [Home](../README.md)

# JavaScript

High-level, versatile programming language primarily used for building interactive and dynamic content on websites. It runs in the browser and supports event handling, DOM manipulation, and APIs, while also being widely used for server-side development with platforms like Node.js.


## Operators
### `...`
The `...` is called the spread operator in JavaScript. It's used to create a shallow copy of a structure, such as an array or an object.

Without spread operator (creates a reference):
```js
this.originalData = data; // Both variables point to the same array in memory
```

With spread operator (creates a copy):
```js
this.originalData = [...data]; // Creates a new array with copied values
```

## Arrow Functions

Provide a shorter syntax compared to regular functions, and they also have some important differences in behavior, particularly with how they handle the `this` keyword. The `=>` syntax in JavaScript is used to define **arrow functions**.

```jsx
const functionName = (parameter1, parameter2) => {
    // function body
};
```
> => is simply a shorthand syntax for writing functions (called arrow functions), and it helps with cleaner code, especially when working with things like array operations or event handlers.

For example:

```jsx
const add = (a, b) => {
    return a + b;
};

// is equivalent to:

const add = function(a, b) {
    return a + b;
};
```

## Modularity

<aside>
💡 Breaking your code into smaller, reusable, and maintainable pieces.
</aside>

## **ES Modules (ECMAScript Modules)**
Use export and import to define and use modules.
```jsx
// mathUtils.js
export function add(a, b) {
    return a + b;
}

// main.js
import { add } from './mathUtils.js';
console.log(add(2, 3));
```

## **Classes or Objects**

### **1. Named Export**
Encapsulate related functionality into a class or object and explicitly name the class while exporting it.
```jsx
// mathUtils.js
export class MathUtils {
    add(a, b) {
        return a + b;
    }
}

// main.js
import { MathUtils } from './mathUtils.js';

const mathUtils = new MathUtils();
console.log(mathUtils.add(2, 3));
```

### **2. Default Export**
Default exports are often used when a module exports only one main item.
```jsx
// File: MyClass.js
export default class MyClass {
    constructor(name) {
        this.name = name;
    }

    greet() {
        return `Hello, ${this.name}!`;
    }
}

// File: main.js
import MyClass from './MyClass.js'; // No curly braces needed for default imports

const instance = new MyClass('Alice');
console.log(instance.greet()); // Hello, Alice!
```

- Use **instance-level (constructor)** if the selections may change per instance or need to be customizable for different class objects.

- Use **static-level** if the selections are fixed and the same across all class instances.

### Notes

Without `type="module"` attribute in the HTML, the browser won’t process import statements.

```bash

<script type="module" src="{{ url_for('static', filename='app.js') }}"></script>
```
> It is a must to include the type="module" attribute when adding the .js file to the HTML file. Without it, the browser will treat app.js as a regular script and throw errors for import statements.

## `.then()`

Part of JavaScript’s **Promise** API, which is used to handle asynchronous operations, like fetching data or waiting for a task to complete.

**Basic Usage:**

1.	`then()` **is called when a Promise resolves** (success) or rejects (error).

2.	It allows you to specify **what to do** when the operation finishes successfully.

```jsx
someAsyncFunction()
  .then((result) => {
    // This block runs if the Promise resolves successfully
    console.log(result);
  })
  .catch((error) => {
    // This block runs if the Promise is rejected (an error occurs)
    console.log(error);
  });
```

For example:

```jsx
fetch("/api/get_config")
  .then((response) => {
    // Process response if the fetch is successful
    return response.json(); // This returns another Promise
  })
  .then((config) => {
    // Process the parsed JSON once the previous Promise resolves
    console.log(config);
  })
  .catch((error) => {
    // This block runs if there was an error at any point
    console.error(error);
  });
```
The first .then() handles the response from the server. The second .then() handles the parsed JSON data. .catch() handles any errors that occurred during either operation.

## `this`

`this` refers to the context in which a function is executed. Its value depends on **how** and **where** the function is called.

### **1. Global Scope**
In the global context (outside any function), this refers to the global object.
```jsx
console.log(this); // In a browser: Window, in Node.js: global
```

### **2. Inside an Object Method**
When a method is called on an object, this refers to the object the method is called on.
```jsx
const obj = {
    name: 'Alice',
    greet() {
        console.log(this.name); // 'Alice'
    }
};

obj.greet(); // 'Alice'
```

### **3. Inside a Regular Function**
In non-strict mode, this refers to the global object. In strict mode ('use strict';), this is undefined.
```jsx
function sayHello() {
    console.log(this); // Global object (non-strict) or undefined (strict)
}
sayHello();
```

### **4. Arrow Functions**
Arrow functions don’t have their own this. They inherit this from their surrounding scope (lexical scope).
```jsx
const obj = {
    name: 'Alice',
    greet: () => {
        console.log(this.name); // Undefined, because `this` is inherited from the global scope
    }
};

obj.greet();
```

To solve this, use a regular function:

```jsx
const obj = {
    name: 'Alice',
    greet() {
        console.log(this.name); // 'Alice'
    }
};

obj.greet();
```

### **5. In Classes**
Inside a class, this refers to the specific instance of the class.
```jsx
class Person {
    constructor(name) {
        this.name = name;
    }

    greet() {
        console.log(`Hello, ${this.name}`);
    }
}

const alice = new Person('Alice');
alice.greet(); // Hello, Alice
```

### **6. Explicit Binding**

You can control what this refers to using call, apply, or bind.

#### `call()`
Invoke a function with a specific this.
```jsx
function greet() {
    console.log(this.name);
}

const obj = { name: 'Alice' };
greet.call(obj); // 'Alice'
```

#### `apply()`
Same as call but passes arguments as an array.
```jsx
greet.apply(obj); // 'Alice'
```

#### `bind()`
Creates a new function with this bound to a specific value.
```jsx
const boundGreet = greet.bind(obj);
boundGreet(); // 'Alice'
```

### **7. Event Handlers**
In event handlers, this typically refers to the element that triggered the event.
```jsx
const button = document.querySelector('button');
button.addEventListener('click', function () {
    console.log(this); // The button element
});
```

To maintain this in callbacks, use arrow functions or bind.
```jsx
button.addEventListener('click', () => {
    console.log(this); // Inherited from parent scope
});
```