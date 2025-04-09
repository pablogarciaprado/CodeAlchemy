

High-level, versatile programming language primarily used for building interactive and dynamic content on websites. It runs in the browser and supports event handling, DOM manipulation, and APIs, while also being widely used for server-side development with platforms like Node.js.

# Arrow Functions

Provide a shorter syntax compared to regular functions, and they also have some important differences in behavior, particularly with how they handle the `this` keyword. The `=>` syntax in JavaScript is used to define **arrow functions**.

```jsx
const functionName = (parameter1, parameter2) => {
    // function body
};
```

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

# Modularity

<aside>
💡

Breaking your code into smaller, reusable, and maintainable pieces.

</aside>

## **ES Modules (ECMAScript Modules)**

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

•	Use **instance-level (constructor)** if the selections may change per instance or need to be customizable for different class objects.

•	Use **static-level** if the selections are fixed and the same across all class instances.

### Notes

Without `type="module"` attribute in the HTML, the browser won’t process import statements.

```bash

<script type="module" src="{{ url_for('static', filename='app.js') }}"></script>
```

# `.then()`

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

# `this`

`this` refers to the context in which a function is executed. Its value depends on **how** and **where** the function is called.

## **1. Global Scope**

```jsx
console.log(this); // In a browser: Window, in Node.js: global
```

## **2. Inside an Object Method**

```jsx
const obj = {
    name: 'Alice',
    greet() {
        console.log(this.name); // 'Alice'
    }
};

obj.greet(); // 'Alice'
```

## **3. Inside a Regular Function**

```jsx
function sayHello() {
    console.log(this); // Global object (non-strict) or undefined (strict)
}
sayHello();
```

## **4. Arrow Functions**

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

## **5. In Classes**

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

## **6. Explicit Binding**

You can control what this refers to using call, apply, or bind.

```jsx
function greet() {
    console.log(this.name);
}

const obj = { name: 'Alice' };
greet.call(obj); // 'Alice'
```

```jsx
greet.apply(obj); // 'Alice'
```

```jsx
const boundGreet = greet.bind(obj);
boundGreet(); // 'Alice'
```

## **7. Event Handlers**

```jsx
const button = document.querySelector('button');
button.addEventListener('click', function () {
    console.log(this); // The button element
});
```

```jsx
button.addEventListener('click', () => {
    console.log(this); // Inherited from parent scope
});
```