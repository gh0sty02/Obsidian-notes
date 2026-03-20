---

---
# Complete JavaScript Interview Guide

## Table of Contents

1. [Core JavaScript Concepts](about:blank#core-javascript-concepts)
2. [Advanced Concepts](about:blank#advanced-concepts)
3. [Asynchronous JavaScript](about:blank#asynchronous-javascript)
4. [Object-Oriented Programming](about:blank#object-oriented-programming)
5. [Execution & Performance](about:blank#execution-performance)
6. [Polyfills](about:blank#polyfills)

---

# Core JavaScript Concepts

## 1. Scope in JavaScript (Global, Functional, Block)

Scope determines the accessibility or visibility of variables in different parts of your code.

### Types of Scope:

**Global Scope:**
- Variables declared outside any function or block
- Accessible from anywhere in the code
- In browsers, global variables become properties of the `window` object

**Function Scope:**
- Variables declared with `var` inside a function
- Accessible anywhere within that function
- Not accessible outside the function

**Block Scope:**
- Introduced in ES6 with `let` and `const`
- Variables are only accessible within the block `{...}` they’re defined in
- Works with if statements, loops, and any code block

**Example:**

```javascript
// Global Scope
const globalVar = "I'm global";

function outerFunction() {
  // Function Scope
  var functionVar = "I'm function-scoped";

  if (true) {
    // Block Scope
    let blockVar = "I'm block-scoped";
    const anotherBlockVar = "I'm also block-scoped";
    var notBlockScoped = "I'm function-scoped, not block-scoped!";

    console.log(blockVar); // Accessible
  }

  console.log(functionVar); // Accessible
  console.log(notBlockScoped); // Accessible (var is function-scoped)
  // console.log(blockVar); // ReferenceError: blockVar is not defined
}

console.log(globalVar); // Accessible
// console.log(functionVar); // ReferenceError
```

---

## 2. Scope Chaining

Scope chaining is the mechanism JavaScript uses to resolve variable references. When you access a variable, JavaScript first looks in the current scope. If it doesn’t find it, it moves up to the outer (parent) scope, and continues up the chain until it reaches the global scope.

This chain is established based on where functions are **defined** in the code (lexical scoping), not where they are called.

**Example:**

```javascript
const globalVar = "I'm global";

function outerFunction() {
  const outerVar = "I'm outer";

  function middleFunction() {
    const middleVar = "I'm middle";

    function innerFunction() {
      const innerVar = "I'm inner";

      // Scope chain: inner -> middle -> outer -> global
      console.log(innerVar);   // Found in local scope
      console.log(middleVar);  // Found in middle scope
      console.log(outerVar);   // Found in outer scope
      console.log(globalVar);  // Found in global scope
    }

    innerFunction();
  }

  middleFunction();
}

outerFunction();

// Real-world example: Counter with private variables
function createCounter() {
  let count = 0; // Private variable in outer scope

  return {
    increment() {
      count++; // Accesses 'count' via scope chain
      return count;
    },
    decrement() {
      count--; // Also accesses 'count' via scope chain
      return count;
    },
    getCount() {
      return count;
    }
  };
}

const counter = createCounter();
console.log(counter.increment()); // 1
console.log(counter.increment()); // 2
console.log(counter.getCount());  // 2
// console.log(count); // ReferenceError: count is not defined
```

---

## 3. Primitive vs. Non-Primitive Types

JavaScript data types are categorized into two groups:

### Primitive Types (Immutable, Stored by Value):

- **string** - Text data (`"hello"`)
- **number** - Numeric data (`42`, `3.14`)
- **boolean** - True or false (`true`, `false`)
- **null** - Intentional absence of value
- **undefined** - Variable declared but not assigned
- **symbol** - Unique identifier (ES6)
- **bigint** - Large integers (ES2020)

### Non-Primitive (Reference) Types (Mutable, Stored by Reference):

- **object** - Collections of key-value pairs
- **array** - Ordered collections
- **function** - Callable objects
- **date**, **regexp**, and other built-in objects

### Key Differences:

**Storage:**
- Primitives: Value is stored directly in the variable
- Non-primitives: Variable stores a reference (memory address)

**Comparison:**
- Primitives: Compared by value
- Non-primitives: Compared by reference

**Mutability:**
- Primitives: Immutable (cannot be changed, only reassigned)
- Non-primitives: Mutable (can be modified)

**Example:**

```javascript
// Primitives - Pass by Value
let a = 10;
let b = a; // Copy of value
b = 20;
console.log(a); // 10 (unchanged)
console.log(b); // 20

let str1 = "hello";
let str2 = str1;
str2 = "world";
console.log(str1); // "hello" (unchanged)

// Non-Primitives - Pass by Reference
let obj1 = { value: 10 };
let obj2 = obj1; // Copy of reference, not the object
obj2.value = 20;
console.log(obj1.value); // 20 (both reference same object)
console.log(obj2.value); // 20

let arr1 = [1, 2, 3];
let arr2 = arr1;
arr2.push(4);
console.log(arr1); // [1, 2, 3, 4] (modified through arr2)

// Comparison
console.log(10 === 10); // true (same value)
console.log("hello" === "hello"); // true (same value)

console.log([1, 2] === [1, 2]); // false (different references)
console.log({a: 1} === {a: 1}); // false (different references)

let obj3 = { a: 1 };
let obj4 = obj3;
console.log(obj3 === obj4); // true (same reference)
```

---

## 4. Var, Let, and Const

Three ways to declare variables in JavaScript, each with distinct behaviors:

| Feature | `var` | `let` | `const` |
| --- | --- | --- | --- |
| **Scope** | Function-scoped | Block-scoped | Block-scoped |
| **Hoisting** | Yes (initialized with `undefined`) | Yes (but in TDZ) | Yes (but in TDZ) |
| **Re-declaration** | Allowed | Not allowed | Not allowed |
| **Re-assignment** | Allowed | Allowed | Not allowed* |
| **Temporal Dead Zone** | No | Yes | Yes |
| **Global Object Property** | Yes (creates property on window) | No | No |

- Note: `const` prevents re-assignment of the variable, but object properties can still be modified.

**Example:**

```javascript
// var - Function Scoped
function testVar() {
  console.log(x); // undefined (hoisted)
  var x = 10;

  if (true) {
    var x = 20; // Same variable (function-scoped)
    console.log(x); // 20
  }

  console.log(x); // 20 (modified in if block)
}

// let - Block Scoped
function testLet() {
  // console.log(y); // ReferenceError: Cannot access 'y' before initialization
  let y = 10;

  if (true) {
    let y = 20; // Different variable (block-scoped)
    console.log(y); // 20
  }

  console.log(y); // 10 (unchanged)
}

// const - Block Scoped, Cannot Reassign
function testConst() {
  const z = 10;
  // z = 20; // TypeError: Assignment to constant variable

  // But object properties can be modified
  const obj = { name: "John" };
  obj.name = "Jane"; // Allowed
  obj.age = 30; // Allowed
  // obj = {}; // TypeError: Assignment to constant variable

  // Arrays too
  const arr = [1, 2, 3];
  arr.push(4); // Allowed
  arr[0] = 10; // Allowed
  // arr = []; // TypeError: Assignment to constant variable
}

// Global object property
var globalVar = "I'm on window";
let globalLet = "I'm not on window";
const globalConst = "I'm not on window";

console.log(window.globalVar); // "I'm on window"
console.log(window.globalLet); // undefined
console.log(window.globalConst); // undefined

// Best Practices:
// 1. Use const by default
// 2. Use let when you need to reassign
// 3. Avoid var in modern JavaScript
```

---

## 5. Temporal Dead Zone (TDZ)

The TDZ is the period between entering a scope and the actual declaration of a `let` or `const` variable. During this time, the variable exists but is not accessible. Attempting to access it results in a `ReferenceError`.

### Why it exists:

- Catches errors by preventing usage before declaration
- Enforces better coding practices
- Makes variable lifecycle more predictable

**Example:**

```javascript
// var - No TDZ (initialized with undefined)
console.log(myVar); // undefined
var myVar = 5;

// let and const - Have TDZ
function example() {
  // TDZ for 'x' starts here

  console.log(typeof x); // ReferenceError (in TDZ)

  let x = 5; // TDZ ends here

  console.log(x); // 5
}

// TDZ in different scopes
function demo() {
  console.log(a); // ReferenceError

  if (true) {
    // TDZ for 'b' starts here
    console.log(b); // ReferenceError
    let b = 20; // TDZ for 'b' ends here
    console.log(b); // 20
  }

  let a = 10; // TDZ for 'a' ends here
  console.log(a); // 10
}

// TDZ with function parameters
function testParams(a = b, b = 2) {
  // 'a' tries to use 'b' before 'b' is initialized
  return [a, b];
}

// console.log(testParams()); // ReferenceError: Cannot access 'b' before initialization
console.log(testParams(1, 2)); // [1, 2] - Works fine

// TDZ doesn't exist for var
function noTDZ() {
  console.log(x); // undefined (no error)
  var x = 10;
}

// Practical implication
let x = 'outer';

function test() {
  console.log(x); // ReferenceError (not 'outer')
  // Even though 'x' exists in outer scope,
  // the local 'let x' creates a TDZ
  let x = 'inner';
}
```

---

## 6. Hoisting

Hoisting is JavaScript’s behavior of moving declarations to the top of their containing scope during the compilation phase, before code execution. However, the behavior differs based on the declaration type.

### What gets hoisted:

- **Function declarations:** Fully hoisted (name + body)
- **var declarations:** Declaration hoisted, initialized with `undefined`
- **let/const declarations:** Hoisted but not initialized (TDZ)
- **Function expressions:** Only variable declaration hoisted
- **Class declarations:** Hoisted but not initialized (TDZ)

**Example:**

```javascript
// Function declarations - Fully hoisted
greet(); // "Hello!" (works before declaration)

function greet() {
  console.log("Hello!");
}

// var declarations - Hoisted and initialized with undefined
console.log(myVar); // undefined (not ReferenceError)
var myVar = 5;
console.log(myVar); // 5

// Equivalent to:
// var myVar = undefined;
// console.log(myVar);
// myVar = 5;
// console.log(myVar);

// let and const - Hoisted but in TDZ
// console.log(myLet); // ReferenceError: Cannot access before initialization
let myLet = 10;

// console.log(myConst); // ReferenceError: Cannot access before initialization
const myConst = 20;

// Function expressions - Only variable hoisted
console.log(myFunc); // undefined
// myFunc(); // TypeError: myFunc is not a function

var myFunc = function() {
  console.log("Hello from expression");
};

myFunc(); // Works now

// Arrow functions - Same as function expressions
console.log(arrowFunc); // undefined
// arrowFunc(); // TypeError

var arrowFunc = () => {
  console.log("Arrow function");
};

// Class declarations - Hoisted but in TDZ
// const obj = new MyClass(); // ReferenceError

class MyClass {
  constructor(name) {
    this.name = name;
  }
}

const obj = new MyClass("John"); // Works after declaration

// Complex example showing precedence
var name = "Global";

function showName() {
  console.log(name); // undefined (not "Global")
  var name = "Local";
  console.log(name); // "Local"
}

showName();

// Equivalent to:
function showName() {
  var name; // Hoisted to top of function
  console.log(name); // undefined
  name = "Local";
  console.log(name); // "Local"
}
```

---

## 7. Prototypes in JavaScript

Prototypes are the mechanism by which JavaScript objects inherit features from one another. Every JavaScript object has an internal property called `[[Prototype]]` which is a reference to another object.

### Key Concepts:

- Every object has a prototype (except the base `Object.prototype`)
- Prototypes form a chain up to `Object.prototype`
- Prototypes enable inheritance and code reuse
- You can access an object’s prototype via `Object.getPrototypeOf()` or `__proto__`

**Example:**

```javascript
// Every object has a prototype
const obj = {};
console.log(Object.getPrototypeOf(obj) === Object.prototype); // true

// Arrays have Array.prototype
const arr = [];
console.log(Object.getPrototypeOf(arr) === Array.prototype); // true

// Functions have Function.prototype
function myFunc() {}
console.log(Object.getPrototypeOf(myFunc) === Function.prototype); // true

// Creating objects with specific prototypes
const animal = {
  eats: true,
  walk() {
    console.log("Animal walks");
  }
};

// Create object with animal as prototype
const rabbit = Object.create(animal);
rabbit.jumps = true;

console.log(rabbit.eats); // true (inherited from animal)
console.log(rabbit.jumps); // true (own property)
rabbit.walk(); // "Animal walks" (inherited method)

// Checking properties
console.log(rabbit.hasOwnProperty('jumps')); // true
console.log(rabbit.hasOwnProperty('eats')); // false (inherited)

// Constructor function and prototype
function Person(name, age) {
  this.name = name;
  this.age = age;
}

// Adding methods to prototype
Person.prototype.greet = function() {
  console.log(`Hi, I'm${this.name}`);
};

Person.prototype.species = "Homo sapiens";

const john = new Person("John", 30);
const jane = new Person("Jane", 25);

john.greet(); // "Hi, I'm John"
jane.greet(); // "Hi, I'm Jane"

// Both share the same method (memory efficient)
console.log(john.greet === jane.greet); // true

// ES6 Class syntax (uses prototypes under the hood)
class Animal {
  constructor(name) {
    this.name = name;
  }

  speak() {
    console.log(`${this.name} makes a sound`);
  }
}

class Dog extends Animal {
  speak() {
    console.log(`${this.name} barks`);
  }
}

const dog = new Dog("Buddy");
dog.speak(); // "Buddy barks"

// Prototype chain
console.log(Object.getPrototypeOf(dog) === Dog.prototype); // true
console.log(Object.getPrototypeOf(Dog.prototype) === Animal.prototype); // true
console.log(Object.getPrototypeOf(Animal.prototype) === Object.prototype); // true
```

---

## 8. Prototype Object

The `prototype` object is a property that exists on constructor functions (and classes). When you create an object using a constructor function, the newly created object’s internal `[[Prototype]]` is set to reference the constructor’s `prototype` object.

### Key Points:

- Every function has a `prototype` property (except arrow functions)
- The `prototype` property is an object
- Properties/methods added to `prototype` are shared across all instances
- This is JavaScript’s way of implementing inheritance

**Example:**

```javascript
// Constructor function
function Person(name, age) {
  // Instance properties
  this.name = name;
  this.age = age;
}

// Adding method to prototype (shared across instances)
Person.prototype.greet = function() {
  console.log(`Hello, I'm${this.name} and I'm${this.age} years old`);
};

Person.prototype.species = "Homo sapiens";

// Creating instances
const person1 = new Person("Alice", 30);
const person2 = new Person("Bob", 25);

// Both instances share the same prototype methods
console.log(person1.greet === person2.greet); // true (same function reference)

person1.greet(); // "Hello, I'm Alice and I'm 30 years old"
person2.greet(); // "Hello, I'm Bob and I'm 25 years old"

// Instance properties vs Prototype properties
console.log(person1.hasOwnProperty('name')); // true (instance property)
console.log(person1.hasOwnProperty('greet')); // false (prototype property)

console.log('greet' in person1); // true (checks prototype chain)

// Modifying prototype affects all instances
Person.prototype.sayGoodbye = function() {
  console.log(`Goodbye from${this.name}`);
};

person1.sayGoodbye(); // Works immediately
person2.sayGoodbye(); // Works immediately

// Instance property shadows prototype property
person1.species = "Human"; // Creates instance property
console.log(person1.species); // "Human" (instance property)
console.log(person2.species); // "Homo sapiens" (prototype property)

// Prototype constructor property
console.log(Person.prototype.constructor === Person); // true

// The relationship
// person1.__proto__ === Person.prototype
// Person.prototype.constructor === Person
console.log(Object.getPrototypeOf(person1) === Person.prototype); // true

// Visualizing the structure
/*
person1 {
  name: "Alice",
  age: 30,
  __proto__: Person.prototype {
    greet: function() {...},
    species: "Homo sapiens",
    sayGoodbye: function() {...},
    constructor: Person,
    __proto__: Object.prototype
  }
}
*/

// Array prototype example
Array.prototype.first = function() {
  return this[0];
};

const numbers = [1, 2, 3];
console.log(numbers.first()); // 1

// All arrays now have this method
const letters = ['a', 'b', 'c'];
console.log(letters.first()); // 'a'
```

---

## 9. Prototype Chaining

Prototype chaining is the mechanism through which JavaScript implements inheritance. When you try to access a property or method on an object, JavaScript first looks at the object itself. If not found, it looks at the object’s prototype, then that prototype’s prototype, and so on, until it reaches `Object.prototype` (which has a prototype of `null`).

**Example:**

```javascript
// Base Animal constructor
function Animal(name) {
  this.name = name;
}

Animal.prototype.eat = function() {
  console.log(`${this.name} is eating`);
};

Animal.prototype.sleep = function() {
  console.log(`${this.name} is sleeping`);
};

// Dog constructor
function Dog(name, breed) {
  Animal.call(this, name); // Call parent constructor
  this.breed = breed;
}

// Set up prototype chain: Dog -> Animal
Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.constructor = Dog;

// Add Dog-specific methods
Dog.prototype.bark = function() {
  console.log(`${this.name} says woof!`);
};

// Create instance
const myDog = new Dog("Buddy", "Golden Retriever");

// Property lookup through prototype chain
myDog.bark();  // Found on Dog.prototype
myDog.eat();   // Found on Animal.prototype (through chain)
myDog.sleep(); // Found on Animal.prototype (through chain)

console.log(myDog.toString()); // Found on Object.prototype (through chain)

// Visualizing the prototype chain:
// myDog -> Dog.prototype -> Animal.prototype -> Object.prototype -> null

console.log(Object.getPrototypeOf(myDog) === Dog.prototype); // true
console.log(Object.getPrototypeOf(Dog.prototype) === Animal.prototype); // true
console.log(Object.getPrototypeOf(Animal.prototype) === Object.prototype); // true
console.log(Object.getPrototypeOf(Object.prototype) === null); // true

// instanceof checks the prototype chain
console.log(myDog instanceof Dog); // true
console.log(myDog instanceof Animal); // true
console.log(myDog instanceof Object); // true

// Property shadowing in the chain
myDog.eat = function() {
  console.log(`${this.name} is eating dog food`);
};

myDog.eat(); // Uses myDog's own method (shadows Animal.prototype.eat)

// Another dog still uses the prototype method
const anotherDog = new Dog("Max", "Labrador");
anotherDog.eat(); // "Max is eating" (from Animal.prototype)

// ES6 Class syntax (still uses prototype chaining under the hood)
class Vehicle {
  constructor(type) {
    this.type = type;
  }

  start() {
    console.log(`${this.type} is starting`);
  }
}

class Car extends Vehicle {
  constructor(brand, model) {
    super('Car');
    this.brand = brand;
    this.model = model;
  }

  honk() {
    console.log(`${this.brand}${this.model} is honking`);
  }
}

const myCar = new Car('Toyota', 'Camry');
myCar.start(); // Inherited from Vehicle
myCar.honk();  // Defined in Car

// Prototype chain: myCar -> Car.prototype -> Vehicle.prototype -> Object.prototype -> null

// Performance consideration
// Properties found early in the chain are faster to access
// Deep prototype chains can impact performance
```

---

## 10. Closures

A closure is the combination of a function and the lexical environment within which that function was declared. In simpler terms, a closure gives you access to an outer function’s scope from an inner function, even after the outer function has finished executing.

### Key Characteristics:

- Inner function has access to outer function’s variables
- Variables are “remembered” even after outer function returns
- Creates private variables (data encapsulation)
- Commonly used in callbacks, event handlers, and module patterns

**Example:**

```javascript
// Basic closure
function createCounter() {
  let count = 0; // Private variable

  return function() {
    count++;
    return count;
  };
}

const counter1 = createCounter();
const counter2 = createCounter();

console.log(counter1()); // 1
console.log(counter1()); // 2
console.log(counter1()); // 3

console.log(counter2()); // 1 (separate closure, separate count)
console.log(counter2()); // 2

// Closure with multiple functions
function createPerson(name) {
  let age = 0; // Private variable

  return {
    getName() {
      return name;
    },
    getAge() {
      return age;
    },
    setAge(newAge) {
      if (newAge >= 0) {
        age = newAge;
      }
    },
    birthday() {
      age++;
    }
  };
}

const john = createPerson("John");
console.log(john.getName()); // "John"
john.setAge(30);
console.log(john.getAge()); // 30
john.birthday();
console.log(john.getAge()); // 31
// console.log(john.age); // undefined (private variable)

// Closure in loops (common interview question)
// Problem: All functions share the same 'i'
for (var i = 1; i <= 3; i++) {
  setTimeout(function() {
    console.log(i); // Prints 4, 4, 4 (not 1, 2, 3)
  }, i * 1000);
}

// Solution 1: Use let (block-scoped)
for (let i = 1; i <= 3; i++) {
  setTimeout(function() {
    console.log(i); // Prints 1, 2, 3 (separate closure for each iteration)
  }, i * 1000);
}

// Solution 2: Use IIFE to create closure
for (var i = 1; i <= 3; i++) {
  (function(j) {
    setTimeout(function() {
      console.log(j); // Prints 1, 2, 3
    }, j * 1000);
  })(i);
}

// Real-world example: Module pattern
const Calculator = (function() {
  let result = 0; // Private variable

  return {
    add(num) {
      result += num;
      return this;
    },
    subtract(num) {
      result -= num;
      return this;
    },
    multiply(num) {
      result *= num;
      return this;
    },
    divide(num) {
      if (num !== 0) {
        result /= num;
      }
      return this;
    },
    getResult() {
      return result;
    },
    reset() {
      result = 0;
      return this;
    }
  };
})();

Calculator.add(10).multiply(2).subtract(5);
console.log(Calculator.getResult()); // 15

// Function factory with closures
function multiplyBy(factor) {
  return function(number) {
    return number * factor;
  };
}

const double = multiplyBy(2);
const triple = multiplyBy(3);

console.log(double(5)); // 10
console.log(triple(5)); // 15

// Closure memory consideration
function createHugeArray() {
  const hugeArray = new Array(1000000).fill('data');

  return function() {
    return hugeArray.length;
  };
}

const getLength = createHugeArray();
// hugeArray is kept in memory because of the closure
// Be careful with large objects in closures
```

---

## 11. Pass by Reference vs. Pass by Value

JavaScript is **always pass by value**, but the confusion arises because the “value” for objects is the reference to the object.

### Primitive Types: Pass by Value

- A copy of the actual value is passed
- Changes inside function don’t affect original

### Objects: Pass by Reference Value

- A copy of the reference (memory address) is passed
- Modifying object properties affects the original
- Reassigning the parameter doesn’t affect the original reference

**Example:**

```javascript
// Primitives - Pass by Value
function changePrimitive(num) {
  num = 100; // Changes local copy only
  console.log("Inside function:", num); // 100
}

let x = 10;
changePrimitive(x);
console.log("Outside function:", x); // 10 (unchanged)

// String example
function changeString(str) {
  str = "changed";
  console.log("Inside:", str); // "changed"
}

let myString = "original";
changeString(myString);
console.log("Outside:", myString); // "original" (unchanged)

// Objects - Reference Value Passed
function changeObject(obj) {
  obj.name = "Changed"; // Modifies the original object
  console.log("Inside:", obj.name); // "Changed"
}

let person = { name: "John", age: 30 };
changeObject(person);
console.log("Outside:", person.name); // "Changed" (modified)

// But reassigning doesn't affect original
function reassignObject(obj) {
  obj = { name: "New Object" }; // Creates new object, local reference only
  obj.age = 25;
  console.log("Inside:", obj); // { name: "New Object", age: 25 }
}

let person2 = { name: "Jane", age: 30 };
reassignObject(person2);
console.log("Outside:", person2); // { name: "Jane", age: 30 } (unchanged)

// Array example
function modifyArray(arr) {
  arr.push(4); // Modifies original array
  console.log("Inside after push:", arr); // [1, 2, 3, 4]

  arr = [10, 20, 30]; // Reassignment doesn't affect original
  console.log("Inside after reassign:", arr); // [10, 20, 30]
}

let numbers = [1, 2, 3];
modifyArray(numbers);
console.log("Outside:", numbers); // [1, 2, 3, 4] (push affected it, reassign didn't)

// Practical example: Why we return new objects
function addProperty(obj, key, value) {
  // Bad: Mutates original
  obj[key] = value;
  return obj;
}

function addPropertyImmutable(obj, key, value) {
  // Good: Returns new object (immutable pattern)
  return { ...obj, [key]: value };
}

const original = { name: "John" };

const modified1 = addProperty(original, "age", 30);
console.log(original); // { name: "John", age: 30 } (mutated!)
console.log(modified1 === original); // true (same reference)

const original2 = { name: "Jane" };
const modified2 = addPropertyImmutable(original2, "age", 25);
console.log(original2); // { name: "Jane" } (unchanged)
console.log(modified2); // { name: "Jane", age: 25 }
console.log(modified2 === original2); // false (different objects)
```

---

## 12. Currying in JavaScript

Currying is the technique of transforming a function that takes multiple arguments into a sequence of nested functions, each taking a single argument. It allows you to create specialized, reusable functions by “pre-loading” a function with some of its arguments.

### Benefits:

- Creates specialized functions from general ones
- Enables function composition
- Improves code reusability
- Makes functions more flexible

**Example:**

```javascript
// Non-curried function
function add(a, b, c) {
  return a + b + c;
}

console.log(add(1, 2, 3)); // 6

// Curried version
function curriedAdd(a) {
  return function(b) {
    return function(c) {
      return a + b + c;
    };
  };
}

console.log(curriedAdd(1)(2)(3)); // 6

// ES6 arrow function syntax (cleaner)
const curriedAddArrow = a => b => c => a + b + c;

console.log(curriedAddArrow(1)(2)(3)); // 6

// Practical example: Creating specialized functions
const multiply = a => b => a * b;

const double = multiply(2); // Partially applied
const triple = multiply(3);
const quadruple = multiply(4);

console.log(double(5)); // 10
console.log(triple(5)); // 15
console.log(quadruple(5)); // 20

// Real-world example: Logger with context
const logger = level => message => timestamp => {
  console.log(`[${timestamp}] [${level}]${message}`);
};

const errorLogger = logger('ERROR');
const warnLogger = logger('WARN');
const infoLogger = logger('INFO');

const logError = errorLogger('Database connection failed');
const logWarn = warnLogger('High memory usage');

logError(new Date().toISOString());
logWarn(new Date().toISOString());

// Currying with DOM manipulation
const setStyle = element => property => value => {
  element.style[property] = value;
  return element;
};

const button = document.createElement('button');
const setButtonStyle = setStyle(button);
const setButtonColor = setButtonStyle('color');
const setButtonBg = setButtonStyle('backgroundColor');

setButtonColor('white');
setButtonBg('blue');

// Generic curry function
function curry(fn) {
  return function curried(...args) {
    if (args.length >= fn.length) {
      return fn.apply(this, args);
    } else {
      return function(...nextArgs) {
        return curried.apply(this, args.concat(nextArgs));
      };
    }
  };
}

// Using the curry function
function sum(a, b, c, d) {
  return a + b + c + d;
}

const curriedSum = curry(sum);

console.log(curriedSum(1)(2)(3)(4)); // 10
console.log(curriedSum(1, 2)(3, 4)); // 10
console.log(curriedSum(1, 2, 3)(4)); // 10

// Practical use case: API request builder
const makeRequest = method => url => headers => body => {
  return fetch(url, {
    method,
    headers,
    body: JSON.stringify(body)
  });
};

const get = makeRequest('GET');
const post = makeRequest('POST');

const postToAPI = post('https://api.example.com');
const postWithAuth = postToAPI({ 'Authorization': 'Bearer token' });

postWithAuth({ data: 'some data' })
  .then(response => response.json())
  .then(data => console.log(data));
```

---

## 13. Infinite Currying Problem

Infinite currying is an advanced currying technique where a function can be called with an indefinite number of arguments. The challenge is to create a function that keeps returning a function until some termination condition is met.

**Example:**

```javascript
// Infinite currying: sum(1)(2)(3)...(n)()
function sum(a) {
  return function(b) {
    if (b !== undefined) {
      return sum(a + b); // Keep currying
    }
    return a; // Termination: return accumulated sum
  };
}

console.log(sum(1)(2)(3)(4)()); // 10
console.log(sum(5)(10)()); // 15
console.log(sum(1)(2)(3)(4)(5)(6)()); // 21

// Version using toString for implicit conversion
function add(a) {
  let sum = a;

  function inner(b) {
    if (b === undefined) {
      return sum;
    }
    sum += b;
    return inner;
  }

  // Custom valueOf/toString for console.log
  inner.valueOf = function() {
    return sum;
  };

  inner.toString = function() {
    return sum.toString();
  };

  return inner;
}

console.log(add(1)(2)(3)(4).valueOf()); // 10
console.log(+add(1)(2)(3)(4)); // 10 (implicit conversion)

// Multiply version
function multiply(a) {
  return function(b) {
    if (b !== undefined) {
      return multiply(a * b);
    }
    return a;
  };
}

console.log(multiply(2)(3)(4)()); // 24

// Generic infinite currying with operator
function infiniteCurry(fn, initial) {
  let result = initial;

  return function inner(value) {
    if (value === undefined) {
      return result;
    }
    result = fn(result, value);
    return inner;
  };
}

const infiniteSum = infiniteCurry((a, b) => a + b, 0);
console.log(infiniteSum(1)(2)(3)(4)()); // 10

const infiniteMultiply = infiniteCurry((a, b) => a * b, 1);
console.log(infiniteMultiply(2)(3)(4)()); // 24

// Advanced: Infinite currying with method chaining
class Calculator {
  constructor(value = 0) {
    this.value = value;
  }

  add(num) {
    if (num === undefined) return this.value;
    return new Calculator(this.value + num);
  }

  multiply(num) {
    if (num === undefined) return this.value;
    return new Calculator(this.value * num);
  }

  subtract(num) {
    if (num === undefined) return this.value;
    return new Calculator(this.value - num);
  }

  valueOf() {
    return this.value;
  }
}

const calc = new Calculator(10);
console.log(+calc.add(5).multiply(2).subtract(10)); // 20
```

---

## 14. Memoization in JavaScript

Memoization is an optimization technique where you cache the results of expensive function calls and return the cached result when the same inputs occur again. This trades memory for speed.

### Use Cases:

- Expensive calculations (Fibonacci, factorial)
- API calls with same parameters
- Recursive functions
- Pure functions with predictable outputs

**Example:**

```javascript
// Basic memoization
function memoize(fn) {
  const cache = new Map();

  return function(...args) {
    const key = JSON.stringify(args);

    if (cache.has(key)) {
      console.log('Returning from cache');
      return cache.get(key);
    }

    console.log('Computing result');
    const result = fn.apply(this, args);
    cache.set(key, result);

    return result;
  };
}

// Example: Expensive calculation
const expensiveSum = (n) => {
  let sum = 0;
  for (let i = 0; i < n * 1000000; i++) {
    sum += i;
  }
  return sum;
};

const memoizedSum = memoize(expensiveSum);

console.time('First call');
console.log(memoizedSum(100)); // "Computing result"
console.timeEnd('First call'); // ~2-3 seconds

console.time('Second call');
console.log(memoizedSum(100)); // "Returning from cache"
console.timeEnd('Second call'); // ~0.001 seconds

// Fibonacci without memoization (exponential time)
function fibonacciSlow(n) {
  if (n <= 1) return n;
  return fibonacciSlow(n - 1) + fibonacciSlow(n - 2);
}

console.time('Fibonacci slow');
console.log(fibonacciSlow(35)); // Takes several seconds
console.timeEnd('Fibonacci slow');

// Fibonacci with memoization (linear time)
const fibonacci = memoize(function(n) {
  if (n <= 1) return n;
  return fibonacci(n - 1) + fibonacci(n - 2);
});

console.time('Fibonacci fast');
console.log(fibonacci(35)); // Nearly instant
console.timeEnd('Fibonacci fast');

console.log(fibonacci(100)); // Would be impossible without memoization

// Memoization with LRU (Least Recently Used) cache
function memoizeLRU(fn, maxSize = 100) {
  const cache = new Map();

  return function(...args) {
    const key = JSON.stringify(args);

    if (cache.has(key)) {
      // Move to end (most recently used)
      const value = cache.get(key);
      cache.delete(key);
      cache.set(key, value);
      return value;
    }

    const result = fn.apply(this, args);

    // Remove oldest if at capacity
    if (cache.size >= maxSize) {
      const firstKey = cache.keys().next().value;
      cache.delete(firstKey);
    }

    cache.set(key, result);
    return result;
  };
}

// Memoization with TTL (Time To Live)
function memoizeWithTTL(fn, ttl = 5000) {
  const cache = new Map();

  return function(...args) {
    const key = JSON.stringify(args);
    const cached = cache.get(key);

    if (cached && Date.now() - cached.timestamp < ttl) {
      return cached.value;
    }

    const result = fn.apply(this, args);

    cache.set(key, {
      value: result,
      timestamp: Date.now()
    });

    return result;
  };
}

// Usage with API calls
const fetchUser = memoizeWithTTL(async (id) => {
  const response = await fetch(`https://api.example.com/users/${id}`);
  return response.json();
}, 60000); // Cache for 1 minute

// Advanced: Memoization class
class Memoizer {
  constructor(fn, options = {}) {
    this.fn = fn;
    this.cache = new Map();
    this.maxSize = options.maxSize || Infinity;
    this.ttl = options.ttl || Infinity;
  }

  call(...args) {
    const key = JSON.stringify(args);
    const cached = this.cache.get(key);

    // Check if cached and not expired
    if (cached && Date.now() - cached.timestamp < this.ttl) {
      // LRU: Move to end
      this.cache.delete(key);
      this.cache.set(key, cached);
      return cached.value;
    }

    const result = this.fn(...args);

    // Remove oldest if at capacity
    if (this.cache.size >= this.maxSize) {
      const firstKey = this.cache.keys().next().value;
      this.cache.delete(firstKey);
    }

    this.cache.set(key, {
      value: result,
      timestamp: Date.now()
    });

    return result;
  }

  clear() {
    this.cache.clear();
  }

  has(...args) {
    const key = JSON.stringify(args);
    return this.cache.has(key);
  }
}

const memoizer = new Memoizer(expensiveSum, { maxSize: 10, ttl: 60000 });
console.log(memoizer.call(100));
console.log(memoizer.has(100)); // true
memoizer.clear();
```

---

## 15. Rest Parameter

The rest parameter syntax (`...`) allows you to represent an indefinite number of arguments as an array. It must be the last parameter in the function signature.

**Example:**

```javascript
// Basic rest parameter
function sum(first, second, ...others) {
  console.log(first);   // 1
  console.log(second);  // 2
  console.log(others);  // [3, 4, 5] (array of remaining arguments)

  return first + second + others.reduce((acc, num) => acc + num, 0);
}

console.log(sum(1, 2, 3, 4, 5)); // 15

// Rest parameter must be last
// function invalid(...rest, last) {} // SyntaxError

// Collecting all arguments
function multiply(...numbers) {
  return numbers.reduce((product, num) => product * num, 1);
}

console.log(multiply(2, 3, 4)); // 24
console.log(multiply(5)); // 5
console.log(multiply()); // 1 (empty array)

// Rest vs arguments object
function oldWay() {
  console.log(arguments); // Arguments object (array-like, not real array)
  console.log(Array.isArray(arguments)); // false

  // Need to convert to array
  const args = Array.from(arguments);
  return args.reduce((sum, num) => sum + num, 0);
}

function modernWay(...numbers) {
  console.log(numbers); // Real array
  console.log(Array.isArray(numbers)); // true

  return numbers.reduce((sum, num) => sum + num, 0);
}

// Useful for function wrappers
function logger(level, ...messages) {
  const timestamp = new Date().toISOString();
  console.log(`[${timestamp}] [${level}]`, ...messages);
}

logger('ERROR', 'Something went wrong', 'in module X', { error: 'details' });
// [2024-01-15T10:30:00.000Z] [ERROR] Something went wrong in module X { error: 'details' }

// Rest in destructuring
const [first, second, ...rest] = [1, 2, 3, 4, 5];
console.log(first); // 1
console.log(second); // 2
console.log(rest); // [3, 4, 5]

// Object rest
const { name, age, ...otherProps } = {
  name: 'John',
  age: 30,
  city: 'NYC',
  country: 'USA',
  job: 'Developer'
};

console.log(name); // 'John'
console.log(age); // 30
console.log(otherProps); // { city: 'NYC', country: 'USA', job: 'Developer' }

// Practical use case: Function that accepts options
function createUser(name, email, ...roles) {
  return {
    name,
    email,
    roles,
    createdAt: new Date()
  };
}

const user = createUser('John', 'john@example.com', 'admin', 'editor', 'viewer');
console.log(user);
// {
//   name: 'John',
//   email: 'john@example.com',
//   roles: ['admin', 'editor', 'viewer'],
//   createdAt: 2024-01-15T10:30:00.000Z
// }
```

---

## 16. Spread Operators

The spread operator (`...`) expands an iterable (like an array or string) into individual elements. It’s the opposite of the rest parameter.

**Example:**

```javascript
// Spreading arrays
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];
const combined = [...arr1, ...arr2];
console.log(combined); // [1, 2, 3, 4, 5, 6]

// Adding elements
const withExtra = [0, ...arr1, 3.5, ...arr2, 7];
console.log(withExtra); // [0, 1, 2, 3, 3.5, 4, 5, 6, 7]

// Copying arrays (shallow copy)
const original = [1, 2, 3];
const copy = [...original];
copy.push(4);
console.log(original); // [1, 2, 3] (unchanged)
console.log(copy); // [1, 2, 3, 4]

// Spreading in function calls
const numbers = [10, 20, 30, 5, 15];
console.log(Math.max(...numbers)); // 30
console.log(Math.min(...numbers)); // 5

// Converting string to array
const str = "hello";
const chars = [...str];
console.log(chars); // ['h', 'e', 'l', 'l', 'o']

// Spreading objects (ES2018)
const user = { name: "John", age: 30 };
const updatedUser = { ...user, age: 31, city: "NYC" };
console.log(updatedUser); // { name: "John", age: 31, city: "NYC" }

// Merging objects (later properties override earlier ones)
const defaults = { theme: 'light', notifications: true };
const userPrefs = { theme: 'dark' };
const settings = { ...defaults, ...userPrefs };
console.log(settings); // { theme: 'dark', notifications: true }

// Shallow copy of objects
const person = {
  name: "Alice",
  address: { city: "NYC", country: "USA" }
};

const personCopy = { ...person };
personCopy.name = "Bob"; // Independent
personCopy.address.city = "LA"; // Affects original (nested object is shared)

console.log(person.name); // "Alice"
console.log(person.address.city); // "LA" (modified through copy)

// Rest vs Spread (same syntax, different context)
function example(...rest) {  // REST: collects arguments into array
  return [...rest];          // SPREAD: expands array into individual elements
}

// Practical use cases

// 1. Cloning with modifications
const product = { id: 1, name: 'Laptop', price: 1000 };
const discountedProduct = { ...product, price: product.price * 0.9 };

// 2. Conditional properties
const includeEmail = true;
const userData = {
  name: 'John',
  age: 30,
  ...(includeEmail && { email: 'john@example.com' })
};
console.log(userData); // { name: 'John', age: 30, email: 'john@example.com' }

// 3. Removing properties
const { password, ...userWithoutPassword } = {
  name: 'John',
  email: 'john@example.com',
  password: 'secret123'
};
console.log(userWithoutPassword); // { name: 'John', email: 'john@example.com' }

// 4. Array concatenation and flattening
const arr = [1, 2, [3, 4]];
const flattened = [].concat(...arr);
console.log(flattened); // [1, 2, 3, 4]

// 5. Converting NodeList to Array
// const divs = [...document.querySelectorAll('div')];

// 6. Unique array values
const withDuplicates = [1, 2, 2, 3, 3, 3, 4];
const unique = [...new Set(withDuplicates)];
console.log(unique); // [1, 2, 3, 4]
```

---

## 17. How Many Ways You Can Create Objects in JS

JavaScript provides multiple ways to create objects, each with different use cases and advantages.

**Example:**

```javascript
// 1. Object Literal (most common and simplest)
const person1 = {
  name: 'John',
  age: 30,
  greet() {
    console.log(`Hello, I'm${this.name}`);
  }
};

// 2. new Object() constructor
const person2 = new Object();
person2.name = 'Jane';
person2.age = 25;
person2.greet = function() {
  console.log(`Hello, I'm${this.name}`);
};

// 3. Constructor Function
function Person(name, age) {
  this.name = name;
  this.age = age;
  this.greet = function() {
    console.log(`Hello, I'm${this.name}`);
  };
}

const person3 = new Person('Bob', 35);

// Better: Add method to prototype (shared across instances)
function PersonOptimized(name, age) {
  this.name = name;
  this.age = age;
}

PersonOptimized.prototype.greet = function() {
  console.log(`Hello, I'm${this.name}`);
};

const person4 = new PersonOptimized('Alice', 28);

// 4. Object.create() - Creates object with specific prototype
const personProto = {
  greet() {
    console.log(`Hello, I'm${this.name}`);
  }
};

const person5 = Object.create(personProto);
person5.name = 'Charlie';
person5.age = 40;

// Object.create(null) - Object with no prototype
const pureObject = Object.create(null);
pureObject.name = 'David';
console.log(pureObject.toString); // undefined (no inherited methods)

// 5. ES6 Class (syntactic sugar over constructor functions)
class PersonClass {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }

  greet() {
    console.log(`Hello, I'm${this.name}`);
  }

  static species = 'Homo sapiens';

  static create(name, age) {
    return new PersonClass(name, age);
  }
}

const person6 = new PersonClass('Eve', 32);
const person7 = PersonClass.create('Frank', 45);

// 6. Factory Function
function createPerson(name, age) {
  return {
    name,
    age,
    greet() {
      console.log(`Hello, I'm${this.name}`);
    }
  };
}

const person8 = createPerson('Grace', 29);

// 7. Object.assign() - Create by merging
const template = {
  greet() {
    console.log(`Hello, I'm${this.name}`);
  }
};

const person9 = Object.assign({}, template, {
  name: 'Henry',
  age: 38
});

// 8. Spread Operator (similar to Object.assign)
const person10 = {
  ...template,
  name: 'Iris',
  age: 27
};

// 9. Singleton Pattern
const Singleton = (function() {
  let instance;

  function createInstance() {
    return {
      name: 'Singleton',
      getId() {
        return 'unique-id';
      }
    };
  }

  return {
    getInstance() {
      if (!instance) {
        instance = createInstance();
      }
      return instance;
    }
  };
})();

const singleton1 = Singleton.getInstance();
const singleton2 = Singleton.getInstance();
console.log(singleton1 === singleton2); // true (same instance)

// Comparison of approaches

// Object Literal - Simple, one-off objects
// Best for: Configuration objects, single instances

// Constructor Function - Multiple instances with shared methods
// Best for: Creating many similar objects (pre-ES6)

// ES6 Class - Modern, cleaner syntax
// Best for: OOP patterns, inheritance

// Factory Function - No 'new' keyword needed, more flexible
// Best for: Privacy, complex object creation logic

// Object.create() - Fine control over prototype chain
// Best for: Prototypal inheritance patterns
```

---

## 18. Generator Functions in JavaScript

Generator functions are special functions that can pause execution and resume later. They are declared with `function*` and use the `yield` keyword to pause. When called, they return an iterator object.

### Key Features:

- Can pause and resume execution
- Maintain state between pauses
- Useful for lazy evaluation and infinite sequences
- Enable cooperative multitasking

**Example:**

```javascript
// Basic generator function
function* numberGenerator() {
  console.log('Start');
  yield 1;
  console.log('After first yield');
  yield 2;
  console.log('After second yield');
  yield 3;
  console.log('End');
}

const gen = numberGenerator();

console.log(gen.next()); // { value: 1, done: false }
// Logs: "Start"

console.log(gen.next()); // { value: 2, done: false }
// Logs: "After first yield"

console.log(gen.next()); // { value: 3, done: false }
// Logs: "After second yield"

console.log(gen.next()); // { value: undefined, done: true }
// Logs: "End"

// Infinite sequence generator
function* idGenerator() {
  let id = 1;
  while (true) {
    yield id++;
  }
}

const ids = idGenerator();
console.log(ids.next().value); // 1
console.log(ids.next().value); // 2
console.log(ids.next().value); // 3

// Fibonacci generator
function* fibonacci() {
  let [prev, curr] = [0, 1];

  while (true) {
    yield curr;
    [prev, curr] = [curr, prev + curr];
  }
}

const fib = fibonacci();
for (let i = 0; i < 10; i++) {
  console.log(fib.next().value);
}
// Output: 1, 1, 2, 3, 5, 8, 13, 21, 34, 55

// Generator with arguments
function* range(start, end, step = 1) {
  for (let i = start; i < end; i += step) {
    yield i;
  }
}

const numbers = range(0, 10, 2);
console.log([...numbers]); // [0, 2, 4, 6, 8]

// Two-way communication with generators
function* twoWay() {
  const value1 = yield 'First';
  console.log('Received:', value1);

  const value2 = yield 'Second';
  console.log('Received:', value2);

  return 'Done';
}

const gen2 = twoWay();
console.log(gen2.next());        // { value: 'First', done: false }
console.log(gen2.next('Hello')); // { value: 'Second', done: false }
// Logs: "Received: Hello"
console.log(gen2.next('World')); // { value: 'Done', done: true }
// Logs: "Received: World"

// Delegating to another generator
function* gen1() {
  yield 1;
  yield 2;
}

function* gen2() {
  yield 3;
  yield 4;
}

function* combined() {
  yield* gen1(); // Delegate to gen1
  yield* gen2(); // Delegate to gen2
}

console.log([...combined()]); // [1, 2, 3, 4]

// Async iteration with generators
function* asyncTasks() {
  const result1 = yield fetch('/api/data1');
  console.log('Got result 1:', result1);

  const result2 = yield fetch('/api/data2');
  console.log('Got result 2:', result2);

  return 'All done';
}

// Generator for tree traversal
function* inorderTraversal(node) {
  if (node.left) {
    yield* inorderTraversal(node.left);
  }
  yield node.value;
  if (node.right) {
    yield* inorderTraversal(node.right);
  }
}

const tree = {
  value: 4,
  left: {
    value: 2,
    left: { value: 1 },
    right: { value: 3 }
  },
  right: {
    value: 6,
    left: { value: 5 },
    right: { value: 7 }
  }
};

console.log([...inorderTraversal(tree)]); // [1, 2, 3, 4, 5, 6, 7]

// Practical use case: Pagination
function* paginate(items, pageSize) {
  for (let i = 0; i < items.length; i += pageSize) {
    yield items.slice(i, i + pageSize);
  }
}

const items = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
const pages = paginate(items, 3);

for (const page of pages) {
  console.log(page);
}
// [1, 2, 3]
// [4, 5, 6]
// [7, 8, 9]
// [10]

// Implementing custom iterator using generator
class Range {
  constructor(start, end) {
    this.start = start;
    this.end = end;
  }

  *[Symbol.iterator]() {
    for (let i = this.start; i <= this.end; i++) {
      yield i;
    }
  }
}

const myRange = new Range(1, 5);
console.log([...myRange]); // [1, 2, 3, 4, 5]

for (const num of myRange) {
  console.log(num); // 1, 2, 3, 4, 5
}
```

---

## 19. JavaScript Single Threaded Language

JavaScript is single-threaded, meaning it has only one call stack and executes one piece of code at a time. Despite this, it can handle asynchronous operations efficiently through the Event Loop, Web APIs, and callback queues.

### Key Concepts:

- **Single Call Stack:** Only one task executes at a time
- **Non-blocking:** Asynchronous operations don’t block the main thread
- **Event Loop:** Manages asynchronous code execution
- **Web APIs:** Browser features that handle async operations outside JS engine

**Example:**

```javascript
// Single-threaded execution
console.log('Start'); // 1st

setTimeout(() => {
  console.log('Timeout'); // 4th (after call stack is clear)
}, 0);

Promise.resolve().then(() => {
  console.log('Promise'); // 3rd (microtask has priority)
});

console.log('End'); // 2nd

// Output:
// Start
// End
// Promise
// Timeout

// Blocking example (bad practice)
function blockFor(ms) {
  const start = Date.now();
  while (Date.now() - start < ms) {
    // Blocks the entire thread
  }
}

console.log('Before blocking');
blockFor(3000); // Blocks for 3 seconds
console.log('After blocking');

// During those 3 seconds:
// - UI freezes
// - No user interactions processed
// - No other code can run

// Non-blocking alternative
console.log('Before async wait');
setTimeout(() => {
  console.log('After async wait');
}, 3000);
console.log('Code continues immediately');

// Demonstrating the call stack
function first() {
  console.log('First function');
  second();
  console.log('First function end');
}

function second() {
  console.log('Second function');
  third();
  console.log('Second function end');
}

function third() {
  console.log('Third function');
}

first();

// Call stack visualization:
// 1. [first]
// 2. [first, second]
// 3. [first, second, third]
// 4. [first, second] (third pops off)
// 5. [first] (second pops off)
// 6. [] (first pops off)

// Output:
// First function
// Second function
// Third function
// Second function end
// First function end

// Web APIs example
console.log('1');

setTimeout(() => {
  console.log('2'); // Handled by Web API
}, 1000);

console.log('3');

fetch('https://api.example.com/data')
  .then(response => response.json())
  .then(data => console.log('4', data)); // Handled by Web API

console.log('5');

// Flow:
// 1. '1' executes (call stack)
// 2. setTimeout handed to Web API
// 3. '3' executes (call stack)
// 4. fetch handed to Web API
// 5. '5' executes (call stack)
// 6. When fetch completes, callback goes to microtask queue
// 7. When timer completes, callback goes to macrotask queue
// 8. Event loop processes queues

// Why single-threaded works for JavaScript:
// 1. Web APIs handle I/O asynchronously
// 2. Event loop coordinates execution
// 3. Microtask queue for Promises (high priority)
// 4. Macrotask queue for setTimeout, I/O (lower priority)
// 5. Most web operations are I/O bound, not CPU bound

// Web Workers: True parallelism (separate thread)
// main.js
if (typeof Worker !== 'undefined') {
  const worker = new Worker('worker.js');

  worker.postMessage({ data: [1, 2, 3, 4, 5] });

  worker.onmessage = (event) => {
    console.log('Result from worker:', event.data);
  };
}

// worker.js (runs in separate thread)
// onmessage = (event) => {
//   const result = expensiveCalculation(event.data);
//   postMessage(result);
// };
```

---

## 20. Callbacks, Why We Need Them

Callbacks are functions passed as arguments to other functions, to be executed later. We need them because JavaScript is asynchronous and single-threaded. Without callbacks (or Promises/async-await), we couldn’t handle operations like API calls, file reading, or timers without blocking the entire application.

**Example:**

```javascript
// Synchronous code (blocking)
function processDataSync() {
  const data = fetchDataSync(); // Blocks execution for 5 seconds
  console.log('Data:', data);
  console.log('Processing complete');
}

// Everything waits for fetchDataSync to complete
processDataSync();
console.log('This line waits for 5 seconds');

// Asynchronous code with callback (non-blocking)
function processDataAsync(callback) {
  fetchDataAsync((data) => {
    callback(data);
  });
  console.log('This runs immediately, not blocked');
}

processDataAsync((data) => {
  console.log('Data:', data);
  console.log('Processing complete');
});

console.log('This also runs immediately');

// Real-world examples

// 1. setTimeout (timer callback)
console.log('Start');

setTimeout(() => {
  console.log('Delayed execution');
}, 2000);

console.log('End');
// Output: Start, End, Delayed execution (after 2 seconds)

// 2. Event listeners (event callback)
const button = document.querySelector('button');

button.addEventListener('click', () => {
  console.log('Button clicked!');
});
// Code continues, button click handled asynchronously

// 3. Array methods (function callbacks)
const numbers = [1, 2, 3, 4, 5];

numbers.forEach((num) => {
  console.log(num * 2);
});

const doubled = numbers.map((num) => num * 2);
console.log(doubled); // [2, 4, 6, 8, 10]

// 4. API calls (async callback)
function fetchUser(id, callback) {
  setTimeout(() => {
    // Simulate API call
    const user = { id, name: 'John Doe' };
    callback(user);
  }, 1000);
}

fetchUser(1, (user) => {
  console.log('User fetched:', user);
});

// 5. File reading (Node.js example)
// const fs = require('fs');
//
// fs.readFile('file.txt', 'utf8', (err, data) => {
//   if (err) {
//     console.error('Error:', err);
//     return;
//   }
//   console.log('File contents:', data);
// });

// Error-first callbacks (Node.js convention)
function fetchData(url, callback) {
  setTimeout(() => {
    const error = null; // or new Error('Failed to fetch')
    const data = { id: 1, name: 'Data' };

    callback(error, data);
  }, 1000);
}

fetchData('https://api.example.com', (err, data) => {
  if (err) {
    console.error('Error:', err);
    return;
  }
  console.log('Data:', data);
});

// Problems with callbacks

// 1. Callback Hell (Pyramid of Doom)
fetchUser(1, (user) => {
  fetchPosts(user.id, (posts) => {
    fetchComments(posts[0].id, (comments) => {
      fetchReplies(comments[0].id, (replies) => {
        console.log(replies);
        // Hard to read and maintain
      });
    });
  });
});

// 2. Error handling becomes complex
getData((err1, data1) => {
  if (err1) {
    handleError(err1);
    return;
  }

  processData(data1, (err2, data2) => {
    if (err2) {
      handleError(err2);
      return;
    }

    saveData(data2, (err3, result) => {
      if (err3) {
        handleError(err3);
        return;
      }

      console.log('Success:', result);
    });
  });
});

// 3. Inversion of control
// You're giving control of your code execution to a third-party function
doSomething(myCallback); // You trust doSomething will call myCallback correctly

// Solutions to callback problems

// 1. Named functions
function handleUser(user) {
  fetchPosts(user.id, handlePosts);
}

function handlePosts(posts) {
  fetchComments(posts[0].id, handleComments);
}

function handleComments(comments) {
  console.log(comments);
}

fetchUser(1, handleUser);

// 2. Promises (introduced to solve callback hell)
fetch('https://api.example.com/user/1')
  .then(response => response.json())
  .then(user => fetch(`https://api.example.com/posts?userId=${user.id}`))
  .then(response => response.json())
  .then(posts => console.log(posts))
  .catch(error => console.error(error));

// 3. Async/await (syntactic sugar over promises)
async function fetchUserData() {
  try {
    const userResponse = await fetch('https://api.example.com/user/1');
    const user = await userResponse.json();

    const postsResponse = await fetch(`https://api.example.com/posts?userId=${user.id}`);
    const posts = await postsResponse.json();

    console.log(posts);
  } catch (error) {
    console.error(error);
  }
}
```

---

# Asynchronous JavaScript

## 21. Event Loop

The Event Loop is the core mechanism in JavaScript that allows the single-threaded engine to handle asynchronous operations without blocking the main thread. It continuously checks the call stack and processes tasks from the task queues.

### Components:

7. **Call Stack** - Executes synchronous code
8. **Web APIs** - Browser features (setTimeout, fetch, DOM events)
9. **Microtask Queue** - Promise callbacks, queueMicrotask
10. **Macrotask Queue** - setTimeout, setInterval, I/O operations
11. **Event Loop** - Coordinates everything

### Process:

12. Execute all synchronous code in the call stack
13. When call stack is empty, process ALL microtasks
14. Take ONE macrotask and execute it
15. Repeat from step 2

**Example:**

```javascript
console.log('Start'); // 1. Synchronous

setTimeout(() => {
  console.log('Timeout 1'); // 5. Macrotask
}, 0);

Promise.resolve().then(() => {
  console.log('Promise 1'); // 3. Microtask
  Promise.resolve().then(() => {
    console.log('Promise 2'); // 4. Microtask (nested)
  });
});

setTimeout(() => {
  console.log('Timeout 2'); // 6. Macrotask
}, 0);

console.log('End'); // 2. Synchronous

// Output:
// Start
// End
// Promise 1
// Promise 2
// Timeout 1
// Timeout 2

// Detailed explanation:
// 1. "Start" executes (call stack)
// 2. setTimeout 1 → Web API → Macrotask queue
// 3. Promise 1 → Microtask queue
// 4. setTimeout 2 → Web API → Macrotask queue
// 5. "End" executes (call stack)
// 6. Call stack empty → Process all microtasks
//    - "Promise 1" executes
//    - Nested promise → Microtask queue
//    - "Promise 2" executes
// 7. All microtasks done → Take one macrotask
//    - "Timeout 1" executes
// 8. Check microtasks (none) → Take next macrotask
//    - "Timeout 2" executes

// Complex example showing priority
console.log('1');

setTimeout(() => console.log('2'), 0);

Promise.resolve()
  .then(() => console.log('3'))
  .then(() => console.log('4'));

setTimeout(() => {
  console.log('5');
  Promise.resolve().then(() => console.log('6'));
}, 0);

Promise.resolve().then(() => {
  console.log('7');
  setTimeout(() => console.log('8'), 0);
});

console.log('9');

// Output:
// 1, 9, 3, 7, 4, 2, 5, 6, 8

// Visualization of the Event Loop
function eventLoopDemo() {
  console.log('Script start');

  setTimeout(() => {
    console.log('setTimeout');
  }, 0);

  Promise.resolve()
    .then(() => {
      console.log('Promise 1');
    })
    .then(() => {
      console.log('Promise 2');
    });

  console.log('Script end');
}

// Call Stack: [eventLoopDemo, console.log('Script start')]
// Web APIs: []
// Microtask Queue: []
// Macrotask Queue: []

// After setTimeout call:
// Web APIs: [setTimeout callback]
// Macrotask Queue: [setTimeout callback] (after timer expires)

// After Promise:
// Microtask Queue: [Promise 1 callback]

// After console.log('Script end'):
// Call Stack: [] (empty)
// Microtask Queue: [Promise 1 callback]
// Event Loop processes all microtasks

// Real-world implication: UI responsiveness
function longRunningTask() {
  // Bad: Blocks UI for 3 seconds
  const start = Date.now();
  while (Date.now() - start < 3000) {
    // Blocking operation
  }
}

function nonBlockingTask() {
  // Good: Allows UI to remain responsive
  let count = 0;
  const total = 1000000;

  function processChunk() {
    for (let i = 0; i < 10000 && count < total; i++) {
      count++;
      // Do work
    }

    if (count < total) {
      setTimeout(processChunk, 0); // Give control back to event loop
    }
  }

  processChunk();
}
```

---

## 22. Callback Queue (Macro-task Queue)

The Callback Queue (also called Task Queue or Macro-task Queue) holds callbacks from asynchronous operations like `setTimeout`, `setInterval`, `setImmediate` (Node.js), I/O operations, and UI rendering tasks.

### Priority:

- Microtasks have higher priority than macrotasks
- Event Loop processes ALL microtasks before taking ONE macrotask
- Then repeats the cycle

**Example:**

```javascript
console.log('Start');

// Macro-task 1
setTimeout(() => {
  console.log('Timeout 1');
}, 0);

// Macro-task 2
setTimeout(() => {
  console.log('Timeout 2');
}, 0);

// Micro-task
Promise.resolve().then(() => {
  console.log('Promise');
});

console.log('End');

// Output:
// Start
// End
// Promise (microtask processed first)
// Timeout 1 (then macrotasks)
// Timeout 2

// Different types of macrotasks
console.log('1');

setTimeout(() => console.log('2 - setTimeout'), 0);

setInterval(() => console.log('3 - setInterval'), 100); // Repeating macrotask

// In Node.js:
// setImmediate(() => console.log('4 - setImmediate'));

console.log('5');

// Nested macrotasks
setTimeout(() => {
  console.log('Outer timeout');

  setTimeout(() => {
    console.log('Inner timeout'); // Goes to end of macrotask queue
  }, 0);

  Promise.resolve().then(() => {
    console.log('Inner promise'); // Processes before next macrotask
  });
}, 0);

// Output:
// Outer timeout
// Inner promise (microtask has priority)
// Inner timeout (next macrotask)

// UI rendering and macrotasks
// In browsers, rendering happens after macrotasks
function updateUI() {
  const div = document.getElementById('status');

  div.textContent = 'Loading...';

  // This setTimeout allows the UI to update
  setTimeout(() => {
    // Heavy computation
    const result = expensiveOperation();
    div.textContent = `Result:${result}`;
  }, 0);

  // Without setTimeout, UI might not update until after computation
}

// Macrotask queue execution order
console.log('A');

setTimeout(() => console.log('B'), 0);
setTimeout(() => console.log('C'), 0);

Promise.resolve()
  .then(() => console.log('D'))
  .then(() => console.log('E'));

setTimeout(() => console.log('F'), 0);

console.log('G');

// Output:
// A, G (synchronous)
// D, E (all microtasks)
// B, C, F (macrotasks in order)

// Practical example: Debouncing with macrotasks
function debounce(func, delay) {
  let timeoutId;

  return function(...args) {
    clearTimeout(timeoutId); // Clear previous macrotask

    timeoutId = setTimeout(() => {
      func.apply(this, args);
    }, delay); // Schedule new macrotask
  };
}

const debouncedSearch = debounce((query) => {
  console.log('Searching for:', query);
}, 500);

// Each call clears the previous macrotask and schedules a new one
debouncedSearch('a');
debouncedSearch('ab');
debouncedSearch('abc'); // Only this executes after 500ms
```

---

## 23. Microtask Queue

The Microtask Queue has higher priority than the Macrotask Queue. It contains callbacks from:
- Promise `.then()`, `.catch()`, `.finally()`
- `queueMicrotask()`
- `MutationObserver`
- `process.nextTick()` (Node.js - even higher priority than other microtasks)

### Key Rule:

**ALL** microtasks must be processed before moving to the next macrotask.

**Example:**

```javascript
console.log('Start');

setTimeout(() => {
  console.log('Timeout'); // Macrotask
}, 0);

Promise.resolve()
  .then(() => {
    console.log('Promise 1'); // Microtask 1
    return Promise.resolve();
  })
  .then(() => {
    console.log('Promise 2'); // Microtask 2
  });

queueMicrotask(() => {
  console.log('queueMicrotask'); // Microtask 3
});

console.log('End');

// Output:
// Start
// End
// Promise 1
// queueMicrotask
// Promise 2
// Timeout

// All microtasks execute before any macrotask

// Microtask queue never empties example
function infiniteMicrotasks() {
  Promise.resolve().then(() => {
    console.log('Microtask');
    infiniteMicrotasks(); // Creates another microtask
  });
}

// infiniteMicrotasks(); // DON'T RUN: Freezes browser (starves macrotask queue)

// Practical example: Breaking the microtask loop
function safeMicrotasks(count = 0) {
  if (count >= 100) {
    console.log('Stopping at 100');
    return;
  }

  Promise.resolve().then(() => {
    console.log('Microtask:', count);
    safeMicrotasks(count + 1);
  });
}

// Or use setTimeout to move to macrotask queue
function breakLoop(count = 0) {
  Promise.resolve().then(() => {
    console.log('Microtask:', count);

    if (count < 100) {
      setTimeout(() => breakLoop(count + 1), 0); // Macrotask: allows UI updates
    }
  });
}

// Microtasks and error handling
Promise.resolve()
  .then(() => {
    console.log('Promise 1');
    throw new Error('Error in Promise 1');
  })
  .then(() => {
    console.log('Promise 2'); // Skipped due to error
  })
  .catch(error => {
    console.error('Caught:', error.message);
  })
  .then(() => {
    console.log('Promise 3'); // Executes after catch
  });

setTimeout(() => {
  console.log('Timeout'); // Executes after all microtasks
}, 0);

// Output:
// Promise 1
// Caught: Error in Promise 1
// Promise 3
// Timeout

// Nested microtasks
Promise.resolve()
  .then(() => {
    console.log('Outer Promise 1');

    Promise.resolve().then(() => {
      console.log('Inner Promise 1');

      Promise.resolve().then(() => {
        console.log('Nested Promise');
      });
    });

    Promise.resolve().then(() => {
      console.log('Inner Promise 2');
    });
  })
  .then(() => {
    console.log('Outer Promise 2');
  });

setTimeout(() => console.log('Timeout'), 0);

// Output:
// Outer Promise 1
// Inner Promise 1
// Inner Promise 2
// Nested Promise
// Outer Promise 2
// Timeout

// process.nextTick in Node.js (higher priority than other microtasks)
// console.log('1');
//
// setTimeout(() => console.log('2'), 0);
//
// Promise.resolve().then(() => console.log('3'));
//
// process.nextTick(() => console.log('4'));
//
// console.log('5');

// Output in Node.js:
// 1, 5, 4, 3, 2
// nextTick executes before Promise microtasks

// Real-world use case: Ensuring state updates
class StateManager {
  constructor() {
    this.state = {};
    this.listeners = [];
  }

  setState(newState) {
    this.state = { ...this.state, ...newState };

    // Use microtask to batch updates
    queueMicrotask(() => {
      this.listeners.forEach(listener => listener(this.state));
    });
  }

  subscribe(listener) {
    this.listeners.push(listener);
  }
}

const store = new StateManager();

store.subscribe(state => console.log('State updated:', state));

store.setState({ count: 1 });
store.setState({ count: 2 });
store.setState({ count: 3 });

// All setState calls happen synchronously,
// but listeners are called once in a microtask
// Output: State updated: { count: 3 }
```

---

## 24. Promises

A Promise is an object representing the eventual completion (or failure) of an asynchronous operation. It can be in one of three states: `pending`, `fulfilled`, or `rejected`.

### States:

- **Pending** - Initial state, neither fulfilled nor rejected
- **Fulfilled** - Operation completed successfully
- **Rejected** - Operation failed

### Methods:

- `.then(onFulfilled, onRejected)` - Handle success/failure
- `.catch(onRejected)` - Handle errors
- `.finally(onFinally)` - Execute regardless of outcome

**Example:**

```javascript
// Creating a Promise
const myPromise = new Promise((resolve, reject) => {
  const success = true;

  setTimeout(() => {
    if (success) {
      resolve('Operation successful!');
    } else {
      reject(new Error('Operation failed!'));
    }
  }, 1000);
});

// Consuming a Promise
myPromise
  .then(result => {
    console.log(result); // "Operation successful!"
    return 'Next step';
  })
  .then(result => {
    console.log(result); // "Next step"
  })
  .catch(error => {
    console.error(error);
  })
  .finally(() => {
    console.log('Cleanup');
  });

// Promise states
const pending = new Promise((resolve) => {
  // Never resolves
});
console.log(pending); // Promise { <pending> }

const fulfilled = Promise.resolve('Success');
console.log(fulfilled); // Promise { 'Success' }

const rejected = Promise.reject(new Error('Failed'));
console.log(rejected); // Promise { <rejected> Error: Failed }

// Chaining Promises
fetch('https://api.example.com/user')
  .then(response => {
    if (!response.ok) throw new Error('Network error');
    return response.json();
  })
  .then(user => {
    console.log('User:', user);
    return fetch(`https://api.example.com/posts?userId=${user.id}`);
  })
  .then(response => response.json())
  .then(posts => {
    console.log('Posts:', posts);
  })
  .catch(error => {
    console.error('Error:', error);
  });

// Promise.all - Wait for all promises
const promise1 = Promise.resolve(3);
const promise2 = new Promise(resolve => setTimeout(() => resolve('foo'), 100));
const promise3 = Promise.resolve(42);

Promise.all([promise1, promise2, promise3])
  .then(values => {
    console.log(values); // [3, 'foo', 42]
  });

// Promise.all fails fast (rejects if any promise rejects)
Promise.all([
  Promise.resolve(1),
  Promise.reject(new Error('Failed')),
  Promise.resolve(3)
])
  .then(results => console.log(results)) // Not called
  .catch(error => console.error(error)); // Error: Failed

// Promise.allSettled - Wait for all, regardless of outcome
Promise.allSettled([
  Promise.resolve(1),
  Promise.reject(new Error('Failed')),
  Promise.resolve(3)
])
  .then(results => {
    console.log(results);
    // [
    //   { status: 'fulfilled', value: 1 },
    //   { status: 'rejected', reason: Error: Failed },
    //   { status: 'fulfilled', value: 3 }
    // ]
  });

// Promise.race - First to settle wins
Promise.race([
  new Promise(resolve => setTimeout(() => resolve('slow'), 1000)),
  new Promise(resolve => setTimeout(() => resolve('fast'), 100))
])
  .then(result => console.log(result)); // "fast"

// Promise.any - First to fulfill wins (ignores rejections)
Promise.any([
  Promise.reject(new Error('Error 1')),
  new Promise(resolve => setTimeout(() => resolve('Success'), 100)),
  Promise.reject(new Error('Error 2'))
])
  .then(result => console.log(result)) // "Success"
  .catch(error => console.error(error)); // Only if all reject

// Error handling in Promises
fetch('/api/data')
  .then(response => response.json())
  .then(data => {
    // Process data
    if (!data.valid) {
      throw new Error('Invalid data');
    }
    return processData(data);
  })
  .catch(error => {
    // Catches network errors, JSON parsing errors, and thrown errors
    console.error('Error:', error);
    return defaultData; // Can return fallback data
  })
  .then(finalData => {
    // This runs even after catch
    console.log('Final data:', finalData);
  });

// Creating utility functions with Promises
function delay(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

async function example() {
  console.log('Start');
  await delay(2000);
  console.log('2 seconds later');
}

// Promisifying callback-based functions
function callbackAPI(arg, callback) {
  setTimeout(() => {
    callback(null, `Result:${arg}`);
  }, 1000);
}

function promisify(fn) {
  return function(...args) {
    return new Promise((resolve, reject) => {
      fn(...args, (error, result) => {
        if (error) reject(error);
        else resolve(result);
      });
    });
  };
}

const promisifiedAPI = promisify(callbackAPI);

promisifiedAPI('test')
  .then(result => console.log(result))
  .catch(error => console.error(error));

// Promise anti-patterns

// BAD: Not returning the promise
fetch('/api/data')
  .then(response => {
    response.json().then(data => { // Nested promise
      console.log(data);
    });
  });

// GOOD: Return the promise
fetch('/api/data')
  .then(response => response.json())
  .then(data => console.log(data));

// BAD: Forgetting .catch()
fetch('/api/data')
  .then(response => response.json())
  .then(data => console.log(data));
// Unhandled promise rejection if error occurs

// GOOD: Always add .catch()
fetch('/api/data')
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error(error));
```

---

## 25. Event Propagation

Event propagation is the process by which an event travels through the DOM. It has three phases:

16. **Capturing Phase** - Event travels from window down to target element
17. **Target Phase** - Event reaches the target element
18. **Bubbling Phase** - Event bubbles up from target back to window

By default, event listeners are registered for the bubbling phase.

**Example:**

```javascript
// HTML structure:
// <div id="grandparent">
//   <div id="parent">
//     <button id="child">Click me</button>
//   </div>
// </div>

const grandparent = document.getElementById('grandparent');
const parent = document.getElementById('parent');
const child = document.getElementById('child');

// Bubbling phase (default, third parameter is false or omitted)
grandparent.addEventListener('click', () => {
  console.log('Grandparent clicked (bubbling)');
});

parent.addEventListener('click', () => {
  console.log('Parent clicked (bubbling)');
});

child.addEventListener('click', () => {
  console.log('Child clicked (bubbling)');
});

// Capturing phase (third parameter is true)
grandparent.addEventListener('click', () => {
  console.log('Grandparent clicked (capturing)');
}, true);

parent.addEventListener('click', () => {
  console.log('Parent clicked (capturing)');
}, true);

child.addEventListener('click', () => {
  console.log('Child clicked (capturing)');
}, true);

// When you click the button, the output is:
// Grandparent clicked (capturing)  ↓ Capturing phase
// Parent clicked (capturing)       ↓
// Child clicked (capturing)        ← Target phase
// Child clicked (bubbling)         ↑ Bubbling phase
// Parent clicked (bubbling)        ↑
// Grandparent clicked (bubbling)   ↑

// Event object properties
child.addEventListener('click', (event) => {
  console.log('event.target:', event.target);       // The clicked element (child)
  console.log('event.currentTarget:', event.currentTarget); // The element with listener (child)
  console.log('event.eventPhase:', event.eventPhase); // 1: capturing, 2: target, 3: bubbling
});
```

---

## 26. Event Bubbling

Event bubbling is the default propagation behavior where an event starts at the target element and then “bubbles up” through its ancestors.

**Example:**

```javascript
<div id="outer" style="padding: 50px; background: lightblue;">
  <div id="middle" style="padding: 50px; background: lightgreen;">
    <div id="inner" style="padding: 50px; background: lightcoral;">
      Click me!
    </div>
  </div>
</div>

<script>
document.getElementById('outer').addEventListener('click', () => {
  console.log('Outer div clicked');
});

document.getElementById('middle').addEventListener('click', () => {
  console.log('Middle div clicked');
});

document.getElementById('inner').addEventListener('click', () => {
  console.log('Inner div clicked');
});

// Clicking "inner" outputs:
// Inner div clicked
// Middle div clicked
// Outer div clicked
</script>
```

---

## 27. Event Capturing

Event capturing is the opposite of bubbling. The event travels from the window down to the target element. Enable it by passing `true` as the third argument to `addEventListener`.

**Example:**

```javascript
document.getElementById('outer').addEventListener('click', () => {
  console.log('Outer div (capturing)');
}, true);

document.getElementById('middle').addEventListener('click', () => {
  console.log('Middle div (capturing)');
}, true);

document.getElementById('inner').addEventListener('click', () => {
  console.log('Inner div (capturing)');
}, true);

// Clicking "inner" outputs:
// Outer div (capturing)
// Middle div (capturing)
// Inner div (capturing)
```

---

## 28. Event stopPropagation

`event.stopPropagation()` prevents the event from traveling further along the propagation path (either up during bubbling or down during capturing). It doesn’t prevent other handlers on the same element from running.

**Example:**

```javascript
document.getElementById('parent').addEventListener('click', () => {
  console.log('Parent clicked');
});

document.getElementById('child').addEventListener('click', (e) => {
  console.log('Child clicked');
  e.stopPropagation(); // Stops event from bubbling to parent
});

document.getElementById('child').addEventListener('click', () => {
  console.log('Child clicked - second handler');
  // This still runs (stopPropagation doesn't affect this)
});

// Clicking child outputs:
// Child clicked
// Child clicked - second handler
// (Parent handler doesn't run)

// stopImmediatePropagation - stops ALL handlers
document.getElementById('button').addEventListener('click', (e) => {
  console.log('First handler');
  e.stopImmediatePropagation();
});

document.getElementById('button').addEventListener('click', () => {
  console.log('Second handler'); // Doesn't run
});
```

---

## 29. Event Delegation

Event delegation is a pattern where you attach a single event listener to a parent element instead of multiple listeners to child elements. You then use `event.target` to determine which child triggered the event.

### Benefits:

- Better performance (fewer event listeners)
- Works with dynamically added elements
- Less memory usage

**Example:**

```javascript
<ul id="todo-list">
  <li>Task 1 <button class="delete">Delete</button></li>
  <li>Task 2 <button class="delete">Delete</button></li>
  <li>Task 3 <button class="delete">Delete</button></li>
</ul>

<script>
// BAD: Multiple event listeners
document.querySelectorAll('.delete').forEach(button => {
  button.addEventListener('click', (e) => {
    e.target.closest('li').remove();
  });
});

// GOOD: Event delegation (one listener on parent)
document.getElementById('todo-list').addEventListener('click', (e) => {
  if (e.target.classList.contains('delete')) {
    e.target.closest('li').remove();
  }
});

// This works even for dynamically added items
const list = document.getElementById('todo-list');
const newItem = document.createElement('li');
newItem.innerHTML = 'Task 4 <button class="delete">Delete</button>';
list.appendChild(newItem);
// The new delete button automatically works!

// More complex delegation example
document.getElementById('app').addEventListener('click', (e) => {
  const target = e.target;

  if (target.matches('.button-primary')) {
    handlePrimaryClick(e);
  } else if (target.matches('.button-secondary')) {
    handleSecondaryClick(e);
  } else if (target.closest('.card')) {
    handleCardClick(e);
  }
});

// Real-world example: Table row actions
document.getElementById('users-table').addEventListener('click', (e) => {
  const target = e.target;
  const row = target.closest('tr');

  if (!row) return;

  const userId = row.dataset.userId;

  if (target.classList.contains('edit-btn')) {
    editUser(userId);
  } else if (target.classList.contains('delete-btn')) {
    deleteUser(userId);
  } else if (target.classList.contains('view-btn')) {
    viewUser(userId);
  }
});
</script>
```

---

## 30. Type Coercion vs. Type Conversion

- **Type Coercion** - Automatic (implicit) conversion performed by JavaScript
- **Type Conversion** - Explicit conversion performed by the developer

**Example:**

```javascript
// Type Coercion (implicit)
console.log("5" + 2);        // "52" (number coerced to string)
console.log("5" - 2);        // 3 (string coerced to number)
console.log("5" * "2");      // 10 (both coerced to numbers)
console.log(true + 1);       // 2 (boolean coerced to 1)
console.log(false + 1);      // 1 (boolean coerced to 0)
console.log("5" == 5);       // true (loose equality coerces)
console.log(null == undefined); // true (special case)

// Type Conversion (explicit)
console.log(Number("5") + 2);        // 7
console.log(String(5) + 2);          // "52"
console.log(Boolean(0));             // false
console.log(Boolean(1));             // true
console.log(Boolean(""));            // false
console.log(Boolean("text"));        // true
console.log(parseInt("123px"));      // 123
console.log(parseFloat("12.5em"));   // 12.5

// Truthy and Falsy values
// Falsy: false, 0, -0, 0n, "", null, undefined, NaN
// Everything else is truthy

if ("0") console.log("Truthy"); // Logs "Truthy" (non-empty string)
if (0) console.log("Truthy");   // Doesn't log (0 is falsy)

// Common coercion gotchas
console.log([] + []);        // "" (arrays to strings, empty + empty)
console.log([] + {});        // "[object Object]"
console.log({} + []);        // "[object Object]" (or 0 in some contexts)
console.log("5" + null);     // "5null"
console.log("5" - null);     // 5 (null coerced to 0)
console.log("5" + undefined); // "5undefined"
console.log("5" - undefined); // NaN

// Best practice: Always use explicit conversion and ===
const userInput = "42";

// BAD
if (userInput == 42) { }

// GOOD
if (Number(userInput) === 42) { }
```

---

## 31. Throttle

Throttling limits the number of times a function can be called over time. It ensures a function is called at most once every specified interval, regardless of how many times the event fires.

### Use Cases:

- Scroll events
- Window resizing
- API rate limiting
- Game loop updates

**Example:**

```javascript
function throttle(func, limit) {
  let inThrottle;

  return function(...args) {
    if (!inThrottle) {
      func.apply(this, args);
      inThrottle = true;

      setTimeout(() => {
        inThrottle = false;
      }, limit);
    }
  };
}

// Usage
const handleScroll = throttle(() => {
  console.log('Scrolled!', Date.now());
}, 1000);

window.addEventListener('scroll', handleScroll);
// Function executes at most once per second

// Advanced throttle with leading and trailing options
function throttleAdvanced(func, limit, options = {}) {
  let timeout;
  let previous = 0;
  const { leading = true, trailing = true } = options;

  return function(...args) {
    const now = Date.now();

    if (!previous && !leading) {
      previous = now;
    }

    const remaining = limit - (now - previous);

    if (remaining <= 0 || remaining > limit) {
      if (timeout) {
        clearTimeout(timeout);
        timeout = null;
      }

      previous = now;
      func.apply(this, args);
    } else if (!timeout && trailing) {
      timeout = setTimeout(() => {
        previous = leading ? Date.now() : 0;
        timeout = null;
        func.apply(this, args);
      }, remaining);
    }
  };
}

// Real-world example: Infinite scroll
const loadMore = throttle(() => {
  if (window.innerHeight + window.scrollY >= document.body.offsetHeight - 100) {
    console.log('Loading more items...');
    // Fetch more data
  }
}, 500);

window.addEventListener('scroll', loadMore);
```

---

## 32. Debounce

Debouncing delays the execution of a function until after a specified time has passed since the last time it was invoked. It’s useful when you want to wait for the user to “finish” an action.

### Use Cases:

- Search input (wait for user to stop typing)
- Form validation
- Window resize
- Auto-save

**Example:**

```javascript
function debounce(func, delay) {
  let timeoutId;

  return function(...args) {
    clearTimeout(timeoutId); // Clear previous timer

    timeoutId = setTimeout(() => {
      func.apply(this, args);
    }, delay);
  };
}

// Usage
const handleSearch = debounce((query) => {
  console.log('Searching for:', query);
  // API call here
}, 500);

searchInput.addEventListener('input', (e) => {
  handleSearch(e.target.value);
});
// Function executes only 500ms after user stops typing

// Debounce with immediate execution
function debounceImmediate(func, delay, immediate = false) {
  let timeoutId;

  return function(...args) {
    const callNow = immediate && !timeoutId;

    clearTimeout(timeoutId);

    timeoutId = setTimeout(() => {
      timeoutId = null;
      if (!immediate) {
        func.apply(this, args);
      }
    }, delay);

    if (callNow) {
      func.apply(this, args);
    }
  };
}

// Real-world examples

// Auto-save
const autoSave = debounce((content) => {
  console.log('Saving draft...');
  fetch('/api/save-draft', {
    method: 'POST',
    body: JSON.stringify({ content })
  });
}, 2000);

editor.addEventListener('input', (e) => {
  autoSave(e.target.value);
});

// Form validation
const validateEmail = debounce((email) => {
  if (!email.includes('@')) {
    showError('Invalid email');
  } else {
    clearError();
  }
}, 300);

emailInput.addEventListener('input', (e) => {
  validateEmail(e.target.value);
});

// Debounce vs Throttle comparison
// User types: "h" "e" "l" "l" "o"
//
// Throttle (500ms): Executes at most every 500ms
// ----h----e---l-l-o
// ^         ^        (fires during typing)
//
// Debounce (500ms): Executes 500ms after last input
// ----h----e---l-l-o-------
//                     ^     (fires 500ms after "o")
```

---

## 33. How JavaScript Parses and Compiles Your Code (Step by Step)

JavaScript execution happens in two main phases: Compilation (Creation) and Execution.

### Phases:

**1. Parsing Phase:**
- **Tokenization/Lexical Analysis** - Code broken into tokens
- **Syntax Analysis** - Tokens parsed into Abstract Syntax Tree (AST)
- **Scope Resolution** - Determine variable scopes

**2. Compilation Phase:**
- **Hoisting** - Function and variable declarations moved to top
- **Scope Chain Creation** - Lexical environments set up
- **Execution Context Creation** - Global and function contexts created

**3. Execution Phase:**
- Code executed line by line
- Variable assignments happen
- Functions invoked
- Call stack manages execution

**Example:**

```javascript
console.log(x); // undefined (hoisted)
var x = 5;

greet(); // "Hello!" (function declaration fully hoisted)
function greet() {
  console.log("Hello!");
}

// During Parsing Phase:
// 1. Code tokenized: console, ., log, (, x, ), ;, var, x, =, 5, ...
// 2. AST created: Structure representing code
// 3. Scopes identified: Global scope, function scopes

// During Compilation Phase (Creation):
// Global Execution Context created:
// - Variable Environment: { x: undefined, greet: <function> }
// - Lexical Environment: Reference to outer scope (null for global)
// - this binding: window/global object

// During Execution Phase:
// Line 1: console.log(x) → prints undefined
// Line 2: x = 5 → assignment
// Line 4: greet() → function invocation creates new execution context

// Modern JS engines (V8) use JIT compilation:
// 1. Parser creates AST
// 2. Interpreter (Ignition) generates bytecode quickly
// 3. Profiler identifies "hot" code (frequently executed)
// 4. Optimizing compiler (TurboFan) compiles hot code to optimized machine code
// 5. Deoptimization if assumptions fail

// Example showing compilation vs execution
function example() {
  console.log(a); // undefined (creation phase: var a hoisted)
  var a = 10;
  console.log(a); // 10 (execution phase: assignment happened)

  function inner() {
    console.log(b); // ReferenceError (let in TDZ)
    let b = 20;
  }
}

// What happens:
// Creation Phase:
// - Global execution context created
// - example function stored
// Execution Phase:
// - example() called
// - New execution context for example created
// - During creation of example's context:
//   - 'a' declared (hoisted), initialized to undefined
//   - 'inner' function stored
// - During execution of example:
//   - console.log(a) executes → undefined
//   - a = 10 assignment
//   - console.log(a) → 10
```

---

## 34. Handling Infinite Microtask Loop

If a microtask continuously creates new microtasks, it starves the macro-task queue, preventing UI updates and freezing the browser.

**Problem:**

```javascript
function infiniteMicrotask() {
  Promise.resolve().then(() => {
    console.log("Microtask");
    infiniteMicrotask(); // Creates another microtask infinitely
  });
}

// infiniteMicrotask(); // DON'T RUN: Freezes browser
```

**Solutions:**

```javascript
// Solution 1: Use setTimeout to break the cycle
function breakableMicrotask() {
  Promise.resolve().then(() => {
    console.log("Microtask");
    setTimeout(breakableMicrotask, 0); // Moves to macrotask queue
  });
}

// Solution 2: Add iteration limit
function limitedMicrotask(count = 0, max = 100) {
  if (count >= max) {
    console.log('Reached limit');
    return;
  }

  Promise.resolve().then(() => {
    console.log("Microtask", count);
    limitedMicrotask(count + 1, max);
  });
}

// Solution 3: Process in chunks
async function processInChunks(items) {
  for (let i = 0; i < items.length; i++) {
    await processItem(items[i]);

    // Break every 100 items
    if (i % 100 === 0) {
      await new Promise(resolve => setTimeout(resolve, 0));
    }
  }
}

// Solution 4: Use generator + scheduler
function* taskGenerator() {
  for (let i = 0; i < 10000; i++) {
    yield i;
  }
}

async function runTasks() {
  const gen = taskGenerator();

  function processNext() {
    const { value, done } = gen.next();

    if (!done) {
      console.log('Processing:', value);
      setTimeout(processNext, 0); // Allows UI updates
    }
  }

  processNext();
}
```

---

## 35. First Class Functions

Functions are “first-class citizens” in JavaScript, meaning they can be:
- Assigned to variables
- Passed as arguments to other functions
- Returned from other functions
- Stored in data structures

**Example:**

```javascript
// 1. Assigned to variables
const greet = function(name) {
  return `Hello,${name}`;
};

const sayHi = greet;
console.log(sayHi("John")); // "Hello, John"

// 2. Passed as arguments (Higher-Order Functions)
function execute(fn, value) {
  return fn(value);
}

console.log(execute(greet, "Alice")); // "Hello, Alice"

// 3. Returned from functions (Closures)
function multiplier(factor) {
  return function(number) {
    return number * factor;
  };
}

const double = multiplier(2);
const triple = multiplier(3);

console.log(double(5)); // 10
console.log(triple(5)); // 15

// 4. Stored in data structures
const mathOperations = {
  add: (a, b) => a + b,
  subtract: (a, b) => a - b,
  multiply: (a, b) => a * b,
  divide: (a, b) => a / b
};

console.log(mathOperations.add(5, 3)); // 8

const operations = [
  x => x + 1,
  x => x * 2,
  x => x - 3
];

let result = 10;
operations.forEach(op => {
  result = op(result);
});
console.log(result); // (10 + 1) * 2 - 3 = 19

// Real-world example: Middleware pattern
const middleware = [];

function use(fn) {
  middleware.push(fn);
}

function execute(data) {
  return middleware.reduce((acc, fn) => fn(acc), data);
}

use(data => ({ ...data, step1: true }));
use(data => ({ ...data, step2: true }));
use(data => ({ ...data, step3: true }));

console.log(execute({ start: true }));
// { start: true, step1: true, step2: true, step3: true }
```

---

## 36. Immediately Invoked Function Expressions (IIFE)

An IIFE is a function that is executed immediately after it’s defined. It’s commonly used to create a private scope and avoid polluting the global namespace.

**Example:**

```javascript
// Basic IIFE
(function() {
  const message = "Hello from IIFE";
  console.log(message);
})();

// IIFE with parameters
(function(name, age) {
  console.log(`${name} is${age} years old`);
})("John", 30);

// Arrow function IIFE
(() => {
  console.log("Arrow IIFE");
})();

// IIFE returning value
const result = (function() {
  const privateVar = 42;
  return privateVar * 2;
})();

console.log(result); // 84

// Named IIFE (useful for recursion)
(function factorial(n) {
  if (n <= 1) return 1;
  return n * factorial(n - 1);
})(5); // 120

// Use case 1: Module pattern
const Counter = (function() {
  let count = 0; // Private variable

  return {
    increment() {
      count++;
      return count;
    },
    decrement() {
      count--;
      return count;
    },
    getCount() {
      return count;
    }
  };
})();

Counter.increment(); // 1
Counter.increment(); // 2
console.log(Counter.getCount()); // 2
// console.log(count); // ReferenceError: count is not defined

// Use case 2: Avoid variable collision
(function() {
  const $ = 'My custom library';
  console.log($); // "My custom library"
})();

console.log($); // jQuery/other library remains unaffected

// Use case 3: Async IIFE
(async function() {
  const data = await fetch('/api/data');
  const json = await data.json();
  console.log(json);
})();

// Use case 4: Creating block scope (pre-ES6)
for (var i = 0; i < 3; i++) {
  (function(j) {
    setTimeout(function() {
      console.log(j); // 0, 1, 2
    }, j * 1000);
  })(i);
}

// ES6 alternative (using let)
for (let i = 0; i < 3; i++) {
  setTimeout(function() {
    console.log(i); // 0, 1, 2
  }, i * 1000);
}
```

---

## 37. Call, Apply, and Bind - Why We Use Them

These methods explicitly set the `this` context of a function.

- **call(thisArg, arg1, arg2, …)** - Invokes immediately, arguments as list
- **apply(thisArg, [args])** - Invokes immediately, arguments as array
- **bind(thisArg)** - Returns new function with bound `this`

**Example:**

```javascript
const person = {
  firstName: 'John',
  lastName: 'Doe',
  fullName() {
    return `${this.firstName}${this.lastName}`;
  }
};

const anotherPerson = {
  firstName: 'Jane',
  lastName: 'Smith'
};

// call - invoke immediately with arguments as list
console.log(person.fullName.call(anotherPerson)); // "Jane Smith"

function greet(greeting, punctuation) {
  return `${greeting},${this.firstName}${punctuation}`;
}

console.log(greet.call(person, 'Hello', '!')); // "Hello, John!"
console.log(greet.call(anotherPerson, 'Hi', '.')); // "Hi, Jane."

// apply - invoke immediately with arguments as array
console.log(greet.apply(person, ['Hey', '...'])); // "Hey, John..."

const numbers = [5, 6, 2, 3, 7];
console.log(Math.max.apply(null, numbers)); // 7

// bind - returns new function with bound this
const greetJohn = greet.bind(person);
console.log(greetJohn('Hello', '!')); // "Hello, John!"

const greetJane = greet.bind(anotherPerson, 'Hello'); // Partial application
console.log(greetJane('!')); // "Hello, Jane!"

// Why we use them:

// 1. Borrowing methods
const obj1 = { name: 'Object 1' };
const obj2 = {
  name: 'Object 2',
  getName() {
    return this.name;
  }
};

console.log(obj2.getName.call(obj1)); // "Object 1"

// 2. Function currying
function multiply(a, b) {
  return a * b;
}

const double = multiply.bind(null, 2);
console.log(double(5)); // 10
console.log(double(10)); // 20

// 3. Event handlers
class Button {
  constructor(label) {
    this.label = label;
    this.clickCount = 0;
  }

  handleClick() {
    this.clickCount++;
    console.log(`${this.label} clicked${this.clickCount} times`);
  }
}

const myButton = new Button('My Button');

// BAD: this is undefined or window
// button.addEventListener('click', myButton.handleClick);

// GOOD: bind preserves this
button.addEventListener('click', myButton.handleClick.bind(myButton));

// Or use arrow function
button.addEventListener('click', () => myButton.handleClick());

// 4. Array-like objects
function sum() {
  return Array.prototype.reduce.call(arguments, (acc, num) => acc + num, 0);
}

console.log(sum(1, 2, 3, 4)); // 10

// 5. Class method chaining
class Calculator {
  constructor() {
    this.value = 0;
  }

  add(num) {
    this.value += num;
    return this;
  }

  multiply(num) {
    this.value *= num;
    return this;
  }

  getValue() {
    return this.value;
  }
}

const calc = new Calculator();
const result = calc.add(5).multiply(2).getValue(); // 10
```

---

## 38. MapLimit

MapLimit processes an array of items with an asynchronous function, but limits the number of concurrent operations. This is essential for rate-limiting API calls or controlling resource usage.

**Example:**

```javascript
async function mapLimit(array, limit, asyncFn) {
  const results = [];
  const executing = [];

  for (const [index, item] of array.entries()) {
    const promise = (async () => {
      try {
        const result = await asyncFn(item, index);
        results[index] = result;
      } catch (error) {
        results[index] = { error };
      }
    })();

    executing.push(promise);

    promise.then(() => {
      executing.splice(executing.indexOf(promise), 1);
    });

    if (executing.length >= limit) {
      await Promise.race(executing);
    }
  }

  await Promise.all(executing);
  return results;
}

// Usage
const urls = [
  'https://api.example.com/item/1',
  'https://api.example.com/item/2',
  'https://api.example.com/item/3',
  'https://api.example.com/item/4',
  'https://api.example.com/item/5',
  'https://api.example.com/item/6'
];

const fetchData = async (url) => {
  const response = await fetch(url);
  return response.json();
};

// Process only 2 requests concurrently
mapLimit(urls, 2, fetchData)
  .then(results => console.log('All results:', results))
  .catch(console.error);

// Real-world example: Batch image uploads
async function uploadImages(images, concurrency = 3) {
  const upload = async (image) => {
    const formData = new FormData();
    formData.append('file', image);

    const response = await fetch('/api/upload', {
      method: 'POST',
      body: formData
    });

    return response.json();
  };

  return mapLimit(images, concurrency, upload);
}
```

---

## 39. Async and Await

`async`/`await` is syntactic sugar over Promises that makes asynchronous code look and behave more like synchronous code.

- `async` keyword makes a function return a Promise
- `await` pauses execution until Promise resolves
- `await` can only be used inside `async` functions

**Example:**

```javascript
// Promise approach (can be hard to read)
function fetchUserData() {
  return fetch('/api/user')
    .then(response => response.json())
    .then(user => {
      return fetch(`/api/posts/${user.id}`);
    })
    .then(response => response.json())
    .then(posts => {
      console.log(posts);
      return posts;
    })
    .catch(error => {
      console.error(error);
    });
}

// Async/await approach (cleaner)
async function fetchUserData() {
  try {
    const userResponse = await fetch('/api/user');
    const user = await userResponse.json();

    const postsResponse = await fetch(`/api/posts/${user.id}`);
    const posts = await postsResponse.json();

    console.log(posts);
    return posts;
  } catch (error) {
    console.error(error);
  }
}

// Key points:

// 1. async functions always return a Promise
async function example() {
  return 'Hello'; // Wrapped in Promise.resolve()
}

example().then(result => console.log(result)); // "Hello"

// 2. await pauses execution
async function demo() {
  console.log('1');
  const result = await Promise.resolve('2');
  console.log(result);
  console.log('3');
}

demo();
console.log('4');
// Output: 1, 4, 2, 3

// 3. Error handling with try-catch
async function fetchData() {
  try {
    const response = await fetch('/api/data');
    if (!response.ok) throw new Error('HTTP error');
    const data = await response.json();
    return data;
  } catch (error) {
    console.error('Error:', error);
    return null; // Return fallback
  } finally {
    console.log('Cleanup');
  }
}

// 4. Parallel execution (await all)
async function fetchMultiple() {
  // Sequential (slow) - 6 seconds total
  const user = await fetch('/api/user'); // 2 seconds
  const posts = await fetch('/api/posts'); // 2 seconds
  const comments = await fetch('/api/comments'); // 2 seconds

  // Parallel (fast) - 2 seconds total
  const [userRes, postsRes, commentsRes] = await Promise.all([
    fetch('/api/user'),
    fetch('/api/posts'),
    fetch('/api/comments')
  ]);

  const userData = await userRes.json();
  const postsData = await postsRes.json();
  const commentsData = await commentsRes.json();

  return { userData, postsData, commentsData };
}

// 5. Async iteration
async function processItems(items) {
  for (const item of items) {
    await processItem(item); // Sequential
  }
}

// 6. Async IIFE
(async () => {
  const data = await fetch('/api/data');
  console.log(await data.json());
})();

// 7. Handling rejections
async function example() {
  // Option 1: try-catch
  try {
    await riskyOperation();
  } catch (error) {
    handleError(error);
  }

  // Option 2: .catch()
  await riskyOperation().catch(handleError);

  // Option 3: Promise.allSettled for multiple operations
  const results = await Promise.allSettled([
    operation1(),
    operation2(),
    operation3()
  ]);
}

// Real-world example: API with retries
async function fetchWithRetry(url, maxRetries = 3) {
  for (let i = 0; i < maxRetries; i++) {
    try {
      const response = await fetch(url);
      if (!response.ok) throw new Error('HTTP error');
      return await response.json();
    } catch (error) {
      if (i === maxRetries - 1) throw error;
      await new Promise(resolve => setTimeout(resolve, 1000 * (i + 1)));
    }
  }
}
```

---

## 40. Then and Catch

`.then()` and `.catch()` are methods on Promise objects used to handle asynchronous results.

**Example:**

```javascript
// Basic then and catch
fetch('/api/data')
  .then(response => {
    console.log('Success:', response);
    return response.json();
  })
  .catch(error => {
    console.error('Error:', error);
  })
  .finally(() => {
    console.log('Cleanup');
  });

// then accepts two callbacks: onFulfilled and onRejected
promise.then(
  result => console.log(result),
  error => console.error(error)
);

// catch is syntactic sugar for then(null, onRejected)
promise.catch(error => console.error(error));
// Same as:
promise.then(null, error => console.error(error));

// Chaining
fetch('/api/user')
  .then(response => response.json())
  .then(user => {
    console.log('User:', user);
    return fetch(`/api/posts/${user.id}`);
  })
  .then(response => response.json())
  .then(posts => {
    console.log('Posts:', posts);
  })
  .catch(error => {
    console.error('Any error in the chain:', error);
  });

// catch doesn't stop the chain
Promise.resolve(1)
  .then(x => {
    console.log(x); // 1
    throw new Error('Error!');
  })
  .catch(error => {
    console.error(error.message); // "Error!"
    return 2; // Recovery value
  })
  .then(x => {
    console.log(x); // 2 (chain continues)
  });

// Multiple catches
fetch('/api/data')
  .then(response => {
    if (!response.ok) throw new Error('HTTP error');
    return response.json();
  })
  .catch(error => {
    console.error('Network or HTTP error:', error);
    throw error; // Re-throw for outer catch
  })
  .then(data => {
    if (!data.valid) throw new Error('Invalid data');
    return processData(data);
  })
  .catch(error => {
    console.error('Processing error:', error);
    return defaultData; // Provide fallback
  })
  .then(finalData => {
    console.log('Final data:', finalData);
  });

// finally - runs regardless of outcome
let isLoading = true;

fetch('/api/data')
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error(error))
  .finally(() => {
    isLoading = false; // Always runs
    console.log('Loading complete');
  });

// Return values in then
Promise.resolve(1)
  .then(x => x + 1) // Returns value: Promise.resolve(2)
  .then(x => {
    console.log(x); // 2
    return x + 1;
  })
  .then(x => {
    console.log(x); // 3
    return Promise.resolve(x + 1); // Returns promise
  })
  .then(x => {
    console.log(x); // 4
  });
```

---

# Object-Oriented Programming

## 41. Variable Shadowing

Variable shadowing occurs when a variable declared in an inner scope has the same name as a variable in an outer scope. The inner variable “shadows” the outer one within its scope.

**Example:**

```javascript
let x = 10; // Global scope

function outer() {
  let x = 20; // Shadows global x
  console.log(x); // 20

  function inner() {
    let x = 30; // Shadows outer's x
    console.log(x); // 30
  }

  inner();
  console.log(x); // 20 (outer's x)
}

outer();
console.log(x); // 10 (global x)

// Illegal shadowing: let/const cannot shadow let/const in same scope
function test1() {
  let a = 10;
  {
    var a = 20; // SyntaxError: Identifier 'a' has already been declared
  }
}

// Legal shadowing: let can shadow var
function test2() {
  var a = 10;
  {
    let a = 20; // Legal: different scope
    console.log(a); // 20
  }
  console.log(a); // 10
}

// Shadowing with function parameters
function greet(name) {
  // 'name' parameter shadows any outer 'name'
  const name = 'John'; // Error: Cannot redeclare 'name'
}

// Temporal Dead Zone and shadowing
let y = 'outer';

function test() {
  console.log(y); // ReferenceError: Cannot access 'y' before initialization
  let y = 'inner'; // Shadows outer y, but creates TDZ
}

// Practical example: Avoiding shadowing issues
const config = { timeout: 5000 };

function makeRequest(config) { // Shadows outer config
  // Use different name to avoid confusion
  const requestConfig = config || { timeout: 3000 };
  // ...
}

// Block scope shadowing
let count = 0;

for (let count = 0; count < 5; count++) {
  // This 'count' shadows the outer 'count'
  console.log(count); // 0, 1, 2, 3, 4
}

console.log(count); // 0 (outer count unchanged)
```

---

## 42. What Does `static` Mean in a JavaScript Class?

`static` methods and properties belong to the class itself, not to instances of the class. They’re called directly on the class and cannot access instance-specific data.

**Example:**

```javascript
class MathHelper {
  static PI = 3.14159;

  static add(a, b) {
    return a + b;
  }

  static multiply(a, b) {
    return a * b;
  }

  // Instance method (for comparison)
  calculateArea(radius) {
    return MathHelper.PI * radius * radius;
  }
}

// Static members accessed on class
console.log(MathHelper.PI); // 3.14159
console.log(MathHelper.add(5, 3)); // 8

// Cannot access static members on instance
const helper = new MathHelper();
// console.log(helper.PI); // undefined
// console.log(helper.add(5, 3)); // TypeError

// But can access instance methods
console.log(helper.calculateArea(5)); // 78.53975

// Common use case: Factory methods
class User {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }

  static fromJSON(json) {
    const data = JSON.parse(json);
    return new User(data.name, data.email);
  }

  static create(name, email) {
    // Validation logic
    if (!email.includes('@')) {
      throw new Error('Invalid email');
    }
    return new User(name, email);
  }
}

const user1 = User.fromJSON('{"name":"John","email":"john@example.com"}');
const user2 = User.create('Jane', 'jane@example.com');

// Static methods can't access 'this' (refers to class, not instance)
class Example {
  constructor(value) {
    this.value = value;
  }

  static staticMethod() {
    // console.log(this.value); // undefined
    console.log(this.name); // "Example" (class name)
  }

  instanceMethod() {
    console.log(this.value); // Instance value
  }
}

// Static methods in inheritance
class Animal {
  static kingdom = 'Animalia';

  static classify() {
    return this.kingdom;
  }
}

class Dog extends Animal {
  static kingdom = 'Canine';
}

console.log(Animal.classify()); // "Animalia"
console.log(Dog.classify()); // "Canine"

// Real-world example: Singleton pattern
class Database {
  static instance;

  constructor() {
    if (Database.instance) {
      return Database.instance;
    }
    this.connection = this.connect();
    Database.instance = this;
  }

  connect() {
    console.log('Connecting to database...');
    return {}; // Mock connection
  }

  static getInstance() {
    if (!Database.instance) {
      Database.instance = new Database();
    }
    return Database.instance;
  }
}

const db1 = Database.getInstance();
const db2 = Database.getInstance();
console.log(db1 === db2); // true (same instance)
```

---

## 43. Undefined vs. Not-Defined vs. Null

Three different concepts representing absence or non-existence of value:

**Example:**

```javascript
// 1. undefined - Variable declared but not assigned
let a;
console.log(a); // undefined
console.log(typeof a); // "undefined"

// Function with no return
function test() {
  // No return statement
}
console.log(test()); // undefined

// Missing property
const obj = { name: 'John' };
console.log(obj.age); // undefined

// Missing array element
const arr = [1, , 3]; // Sparse array
console.log(arr[1]); // undefined

// Function parameter not provided
function greet(name) {
  console.log(name);
}
greet(); // undefined

// 2. null - Intentional absence of value
let b = null;
console.log(b); // null
console.log(typeof b); // "object" (historical JavaScript bug)

// Common use: Initialize or reset value
let user = { name: 'John' };
user = null; // Clear the reference

// API might return null for "no data"
const data = fetchData(); // Returns null if nothing found

// 3. Not-defined - Variable never declared
// console.log(c); // ReferenceError: c is not defined
// console.log(typeof c); // "undefined" (typeof doesn't throw error)

// Comparisons
console.log(undefined == null);  // true (loose equality)
console.log(undefined === null); // false (strict equality)

console.log(null == 0);    // false
console.log(null === 0);   // false
console.log(null < 1);     // true (null coerced to 0)

// Checking for null/undefined
let value;

// Check for both
if (value == null) { // undefined or null
  console.log('No value');
}

// Check specifically
if (value === undefined) {
  console.log('Undefined');
}

if (value === null) {
  console.log('Null');
}

// Nullish coalescing operator (??)
const config = {
  timeout: null,
  retries: undefined
};

// ?? returns right side only if left is null or undefined
const timeout = config.timeout ?? 5000; // 5000
const retries = config.retries ?? 3;    // 3
const zero = 0 ?? 5;                    // 0 (not null/undefined)

// Optional chaining (?.)
const user = {
  name: 'John',
  address: null
};

console.log(user.address?.city); // undefined (doesn't throw error)
console.log(user.phone?.number); // undefined

// Best practices
// Use undefined for:
// - Function return when no value to return
// - Optional function parameters
// - Missing object properties

// Use null for:
// - Intentionally empty values
// - Resetting variables
// - API responses indicating "no data"
```

---

## 44. Higher Order Functions

A higher-order function either:
1. Takes one or more functions as arguments, OR
2. Returns a function as its result

**Example:**

```javascript
// 1. Takes function as argument
function repeat(n, action) {
  for (let i = 0; i < n; i++) {
    action(i);
  }
}

repeat(3, console.log); // 0, 1, 2

// 2. Returns a function
function greaterThan(n) {
  return function(m) {
    return m > n;
  };
}

const greaterThan10 = greaterThan(10);
console.log(greaterThan10(11)); // true
console.log(greaterThan10(9));  // false

// Built-in higher-order functions
const numbers = [1, 2, 3, 4, 5];

// map
const doubled = numbers.map(n => n * 2);
console.log(doubled); // [2, 4, 6, 8, 10]

// filter
const evens = numbers.filter(n => n % 2 === 0);
console.log(evens); // [2, 4]

// reduce
const sum = numbers.reduce((acc, n) => acc + n, 0);
console.log(sum); // 15

// find
const found = numbers.find(n => n > 3);
console.log(found); // 4

// Function composition
const compose = (...fns) => (x) =>
  fns.reduceRight((acc, fn) => fn(acc), x);

const addOne = x => x + 1;
const double = x => x * 2;
const square = x => x * x;

const composed = compose(square, double, addOne);
console.log(composed(2)); // square(double(addOne(2))) = 36

// Pipe (left-to-right composition)
const pipe = (...fns) => (x) =>
  fns.reduce((acc, fn) => fn(acc), x);

const piped = pipe(addOne, double, square);
console.log(piped(2)); // square(double(addOne(2))) = 36

// Practical examples

// 1. Array utilities
function filterMap(array, filterFn, mapFn) {
  return array.filter(filterFn).map(mapFn);
}

const result = filterMap(
  [1, 2, 3, 4, 5],
  n => n % 2 === 0,
  n => n * 2
);
console.log(result); // [4, 8]

// 2. Middleware pattern
function createMiddleware() {
  const middlewares = [];

  return {
    use(fn) {
      middlewares.push(fn);
    },
    execute(data) {
      return middlewares.reduce((acc, fn) => fn(acc), data);
    }
  };
}

const app = createMiddleware();
app.use(data => ({ ...data, step1: true }));
app.use(data => ({ ...data, step2: true }));

console.log(app.execute({ start: true }));
// { start: true, step1: true, step2: true }

// 3. Memoization higher-order function
function memoize(fn) {
  const cache = new Map();
  return function(...args) {
    const key = JSON.stringify(args);
    if (cache.has(key)) return cache.get(key);
    const result = fn.apply(this, args);
    cache.set(key, result);
    return result;
  };
}

const factorial = memoize(n => {
  if (n <= 1) return 1;
  return n * factorial(n - 1);
});

// 4. Partial application
function partial(fn, ...fixedArgs) {
  return function(...remainingArgs) {
    return fn(...fixedArgs, ...remainingArgs);
  };
}

function greet(greeting, name) {
  return `${greeting},${name}!`;
}

const sayHello = partial(greet, 'Hello');
console.log(sayHello('John')); // "Hello, John!"
```

---

## 45. Callback Hell

Callback Hell (or “Pyramid of Doom”) refers to deeply nested callbacks that make code hard to read, maintain, and debug.

**Example:**

```javascript
// Callback Hell example
getData(function(a) {
  getMoreData(a, function(b) {
    getEvenMoreData(b, function(c) {
      getYetMoreData(c, function(d) {
        getFinalData(d, function(e) {
          console.log(e);
        });
      });
    });
  });
});

// Real-world example
fs.readFile('file1.txt', 'utf8', (err, data1) => {
  if (err) return handleError(err);

  fs.readFile('file2.txt', 'utf8', (err, data2) => {
    if (err) return handleError(err);

    fs.readFile('file3.txt', 'utf8', (err, data3) => {
      if (err) return handleError(err);

      processData(data1 + data2 + data3, (err, result) => {
        if (err) return handleError(err);

        saveResult(result, (err) => {
          if (err) return handleError(err);
          console.log('All done!');
        });
      });
    });
  });
});

// Solutions:

// 1. Named functions
function handleA(a) {
  getMoreData(a, handleB);
}

function handleB(b) {
  getEvenMoreData(b, handleC);
}

function handleC(c) {
  getYetMoreData(c, handleD);
}

function handleD(d) {
  getFinalData(d, handleE);
}

function handleE(e) {
  console.log(e);
}

getData(handleA);

// 2. Promises
getData()
  .then(a => getMoreData(a))
  .then(b => getEvenMoreData(b))
  .then(c => getYetMoreData(c))
  .then(d => getFinalData(d))
  .then(e => console.log(e))
  .catch(error => console.error(error));

// 3. Async/Await (cleanest)
async function processData() {
  try {
    const a = await getData();
    const b = await getMoreData(a);
    const c = await getEvenMoreData(b);
    const d = await getYetMoreData(c);
    const e = await getFinalData(d);
    console.log(e);
  } catch (error) {
    console.error(error);
  }
}

// 4. Promise.all for parallel operations
async function fetchAllData() {
  try {
    const [data1, data2, data3] = await Promise.all([
      readFile('file1.txt'),
      readFile('file2.txt'),
      readFile('file3.txt')
    ]);

    const result = await processData(data1 + data2 + data3);
    await saveResult(result);
    console.log('All done!');
  } catch (error) {
    handleError(error);
  }
}
```

---

[Continue in next file due to length limit…]