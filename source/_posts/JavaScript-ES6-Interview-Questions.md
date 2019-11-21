---
title: JavaScript-ES6-Interview-Questions
date: 2019-11-17 15:55:37
tags: Interview
---

[TOC]

ES6 refers to version 6 of the ECMA Script programming language. ECMA Script is the standardized name for JavaScript, and version 6 is the next version after version 5, which was released in 2011. It is a major enhancement to the JavaScript language, and adds many more features intended to make large-scale software development easier. Both ES6 and ES2015 names used for the version of JavaScript that introduces arrow functions, template strings, and Promises.

## Q1: Could you explain the difference between ES5 and ES6

- ECMAScript 5 (ES5): The 5th edition of ECMAScript, standardized in 2009. This standard has been implemented fairly completely in all modern browsers

- ECMAScript 6 (ES6)/ ECMAScript 2015 (ES2015): The 6th edition of ECMAScript, standardized in 2015. This standard has been partially implemented in most modern browsers.

Here are some key differences between ES5 and ES6:

- Arrow functions & string interpolation:
  Consider :

```javascript
const greetings = name => {
  return `hello ${name}`;
};
```

and even:

```javascript
const greetings = name => `hello ${name}`;
```

- Const
  Const works like a constant in other languages in many ways but there are some caveats. Const stands for ‘constant reference’ to a value. So with const, you can actually mutate the properties of an object being referenced by the variable. You just can’t change the reference itself.

```javascript
const NAMES = [];
NAMES.push("Jim");
console.log(NAMES.length === 1); // true
NAMES = ["Steve", "John"]; // error
```

- Block-scoped variables.
  The new ES6 keyword let allows developers to scope variables at the block level. Let doesn’t hoist in the same way var does.

* Default parameter values
  Default parameters allow us to initialize functions with default values. A default is used when an argument is either omitted or undefined — meaning null is a valid value.

```javascript
// Basic syntax
function multiply(a, b = 2) {
  return a * b;
}
multiply(5); // 10
```

- Class Definition and Inheritance
  ES6 introduces language support for classes (class keyword), constructors (constructor keyword), and the extend keyword for inheritance.

* for-of operator
  The for...of statement creates a loop iterating over iterable objects.
* Spread Operator For objects merging

```javascript
const obj1 = { a: 1, b: 2 };
const obj2 = { a: 2, c: 3, d: 4 };
const obj3 = { ...obj1, ...obj2 };
```

- Promises
  Promises provide a mechanism to handle the results and errors from asynchronous operations. You can accomplish the same thing with callbacks, but promises provide improved readability via method chaining and succinct error handling.

```javascript
const isGreater = (a, b) => {
  return new Promise((resolve, reject) => {
    if (a > b) {
      resolve(true);
    } else {
      reject(false);
    }
  });
};
isGreater(1, 2)
  .then(result => {
    console.log("greater");
  })
  .catch(result => {
    console.log("smaller");
  });
```

- Modules exporting & importing Consider module exporting:

```javascript
const myModule = {
  x: 1,
  y: () => {
    console.log("This is ES5");
  }
};
export default myModule;
```

and importing:

```javascript
import myModule fomr './myModule';
```

## Q2: What is IIFEs (Immediately Invoked Function Expressions)?

It’s an Immediately-Invoked Function Expression, or IIFE for short. It executes immediately after it’s created:

```javascript
(function IIFE() {
  console.log("Hello!");
})();
// "Hello!"
```

This pattern is often used when trying to avoid polluting the global namespace, because all the variables used inside the IIFE (like in any other normal function) are not visible outside its scope.

### Source:[stackoverflow.com](https://stackoverflow.com/questions/8228281/what-is-the-function-construct-in-javascript)

## Q3: When should I use Arrow functions in ES6?

I'm now using the following rule of thumb for functions in ES6 and beyond:

- Use `function` in the global scope and for Object.prototype properties.
- Use `class` for object constructors.
- Use `=>` everywhere else.

Why use arrow functions almost everywhere?

- **Scope safety**: When arrow functions are used consistently, everything is guaranteed to use the same thisObject as the root. If even a single standard function callback is mixed in with a bunch of arrow functions there's a chance the scope will become messed up.
- **Compactness**: Arrow functions are easier to read and write. (This may seem opinionated so I will give a few examples further on).
- **Clarity**: When almost everything is an arrow function, any regular function immediately sticks out for defining the scope. A developer can always look up the next-higher function statement to see what the thisObject is.

## Q4: What is the motivation for bringing Symbols to ES6?

`Symbols` are a new, special kind of object that can be used as a unique property name in objects. Using `Symbol` instead of string's allows different modules to create properties that don't conflict with one another. `Symbols` can also be made private, so that their properties can't be accessed by anyone who doesn't already have direct access to the `Symbol`.

`Symbols` are a new primitive. Just like the `number`, `string`, and `boolean` primitives, `Symbol` have a function which can be used to create them. Unlike the other primitives, `Symbols` do not have a literal syntax (e.g how string have '') - the only way to create them is with the `Symbol` constructor in the following way:

```javascript
let symbol = Symbol();
```

In reality, `Symbol`'s are just a slightly different way to attach properties to an object - you could easily provide the well-known Symbols as standard methods, just like `Object.prototype.hasOwnProperty` which appears in everything that inherits from Object.

## Q5: What are the benefits of using spread syntax in ES6 and how is it different from rest syntax?

ES6's spread syntax is very useful when coding in a functional paradigm as we can easily create copies of arrays or objects without resorting to `Object.create`, `slice`, or a library function. This language feature is used often in Redux and rx.js projects.

```javascript
function putDookieInAnyArray(arr) {
  return [...arr, "dookie"];
}

const result = putDookieInAnyArray(["I", "really", "don't", "like"]); // ["I", "really", "don't", "like", "dookie"]

const person = {
  name: "Todd",
  age: 29
};

const copyOfTodd = { ...person };
```

ES6's rest syntax offers a shorthand for including an arbitrary number of arguments to be passed to a function. It is like an inverse of the spread syntax, taking data and stuffing it into an array rather than unpacking an array of data, and it works in function arguments, as well as in array and object destructuring assignments.

```javascript
function addFiveToABunchOfNumbers(...numbers) {
  return numbers.map(x => x + 5);
}

const result = addFiveToABunchOfNumbers(4, 5, 6, 7, 8, 9, 10); // [9, 10, 11, 12, 13, 14, 15]

const [a, b, ...rest] = [1, 2, 3, 4]; // a: 1, b: 2, rest: [3, 4]

const { e, f, ...others } = {
  e: 1,
  f: 2,
  g: 3,
  h: 4
}; // e: 1, f: 2, others: { g: 3, h: 4 }
```

## Q6: What are the differences between ES6 class and ES5 function constructors?

Let's first look at example of each:

```javascript
// ES5 Function Constructor
function Person(name) {
  this.name = name;
}

// ES6 Class
class Person {
  constructor(name) {
    this.name = name;
  }
}
```

For simple constructors, they look pretty similar.

The main difference in the constructor comes when using inheritance. If we want to create a `Student` class that subclasses `Person` and add a `studentId` field, this is what we have to do in addition to the above.

```javascript
// ES5 Function Constructor
function Student(name, studentId) {
  // Call constructor of superclass to initialize superclass-derived members.
  Person.call(this, name);

  // Initialize subclass's own members.
  this.studentId = studentId;
}

Student.prototype = Object.create(Person.prototype);
Student.prototype.constructor = Student;

// ES6 Class
class Student extends Person {
  constructor(name, studentId) {
    super(name);
    this.studentId = studentId;
  }
}
```

It's much more verbose to use inheritance in ES5 and the ES6 version is easier to understand and remember.

## Q7: What's the difference between .call and .apply?

Both `.call` and `.apply` are used to invoke functions and the first parameter will be used as the value of `this` within the function. However, `.call` takes in comma-separated arguments as the next arguments while `.apply` takes in an array of arguments as the next argument. An easy way to remember this is C for `call` and comma-separated and A for `apply` and an array of arguments.

```javascript
function add(a, b) {
  return a + b;
}

console.log(add.call(null, 1, 2)); // 3
console.log(add.apply(null, [1, 2])); // 3
```

## Q8: Why should we use ES6 classes?

Some reasons you might choose to use Classes:

- The syntax is simpler and less error-prone.
- It's much easier (and again, less error-prone) to set up inheritance hierarchies using the new syntax than with the old.
- `class` defends you from the common error of failing to use `new` with the constructor function (by having the constructor throw an exception if this isn't a valid object for the constructor).
- Calling the parent prototype's version of a method is much simpler with the new syntax than the old (`super.method()` instead of `ParentConstructor.prototype.method.call(this)` or `Object.getPrototypeOf(Object.getPrototypeOf(this)).method.call(this)`).

Consider:

```javascript
// **ES5**
var Person = function(first, last) {
  if (!(this instanceof Person)) {
    throw new Error("Person is a constructor function, use new with it");
  }
  this.first = first;
  this.last = last;
};

Person.prototype.personMethod = function() {
  return (
    "Result from personMethod: this.first = " +
    this.first +
    ", this.last = " +
    this.last
  );
};

var Employee = function(first, last, position) {
  if (!(this instanceof Employee)) {
    throw new Error("Employee is a constructor function, use new with it");
  }
  Person.call(this, first, last);
  this.position = position;
};
Employee.prototype = Object.create(Person.prototype);
Employee.prototype.constructor = Employee;
Employee.prototype.personMethod = function() {
  var result = Person.prototype.personMethod.call(this);
  return result + ", this.position = " + this.position;
};
Employee.prototype.employeeMethod = function() {
  // ...
};
```

And the same with ES6 classes:

```javascript
// ***ES2015+**
class Person {
  constructor(first, last) {
    this.first = first;
    this.last = last;
  }

  personMethod() {
    // ...
  }
}

class Employee extends Person {
  constructor(first, last, position) {
    super(first, last);
    this.position = position;
  }

  employeeMethod() {
    // ...
  }
}
```

## Q9: What is the preferred syntax for defining enums in JavaScript?

Since 1.8.5 it's possible to seal and freeze the object, so define the above as:

```javascript
var DaysEnum = Object.freeze({
    "monday": 1,
    "tuesday": 2,
    "wednesday": 3,
    ...
})
```

or

```javascript
var DaysEnum = {
    "monday": 1,
    "tuesday": 2,
    "wednesday": 3,
    ...
}
Object.freeze(DaysEnum)
```

and voila! JS enums.

However, this doesn't prevent you from assigning an undesired value to a variable, which is often the main goal of enums:

```javascript
let day = DaysEnum.tuesday;
day = 298832342; // goes through without any errors
```

## Q10: Explain the difference between Object.freeze() vs const

`const` and `Object.freeze` are two completely different things.

- `const` applies to **bindings** ("variables"). It creates an immutable binding, i.e. you cannot assign a new value to the binding.

```javascript
const person = {
  name: "Leonardo"
};
let animal = {
  species: "snake"
};
person = animal; // ERROR "person" is read-only
```

- Object.freeze works on values, and more specifically, object values. It makes an object immutable, i.e. you cannot change its properties.

```javascript
let person = {
  name: "Leonardo"
};
let animal = {
  species: "snake"
};
Object.freeze(person);
person.name = "Lima"; //TypeError: Cannot assign to read only property 'name' of object
console.log(person);
```

## Q11: What is generator in JS?

Generators are functions which can be exited and later re-entered. Their context (variable bindings) will be saved across re-entrances. Generator functions are written using the `function*` syntax. When called initially, generator functions do not execute any of their code, instead returning a type of iterator called a Generator. When a value is consumed by calling the generator's next method, the Generator function executes until it encounters the `yield` keyword.

The function can be called as many times as desired and returns a new Generator each time, however each Generator may only be iterated once.

```javascript
function* makeRangeIterator(start = 0, end = Infinity, step = 1) {
  let iterationCount = 0;
  for (let i = start; i < end; i += step) {
    iterationCount++;
    yield i;
  }
  return iterationCount;
}
```

## Q12: What is Hoisting in JavaScript?

Hoisting is the JavaScript interpreter's action of moving all variable and function declarations to the top of the current scope. There are two types of hoisting:

- variable hoisting - rare
- function hoisting - more common

Wherever a `var` (or function declaration) appears inside a scope, that declaration is taken to belong to the entire scope and accessible everywhere throughout.

```javascript
var a = 2;
foo(); // works because `foo()`
// declaration is "hoisted"

function foo() {
  a = 3;
  console.log(a); // 3
  var a; // declaration is "hoisted"
  // to the top of `foo()`
}

console.log(a); // 2
```

## Q13: Explain the Prototype Design Pattern

The Prototype Pattern creates new objects, but rather than creating non-initialized objects it returns objects that are initialized with values it copied from a prototype - or sample - object. The Prototype pattern is also referred to as the Properties pattern.

An example of where the Prototype pattern is useful is the initialization of business objects with values that match the default values in the database. The prototype object holds the default values that are copied over into a newly created business object.

Classical languages rarely use the Prototype pattern, but JavaScript being a prototypal language uses this pattern in the construction of new objects and their prototypes.

## Q14: What is the Temporal Dead Zone in ES6?

In ES6 `let` and `const` are hoisted (like `var`, `class` and `function`), but there is a period between entering scope and being declared where they cannot be accessed. **This period is the temporal dead zone (TDZ).**

Consider:

```javascript
//console.log(aLet)  // would throw ReferenceError

let aLet;
console.log(aLet); // undefined
aLet = 10;
console.log(aLet); // 10
```

In this example the **TDZ** ends when `aLet` is declared, rather than assigned.

## Q15: When should you NOT use arrow functions in ES6? Name three or more cases.

`WeakMaps` provide a way to extend objects from the outside without interfering with garbage collection. Whenever you want to extend an object but can't because it is sealed - or from an external source - a WeakMap can be applied.

`WeakMap` is only available for ES6 and above. A WeakMap is a collection of key and value pairs where the key must be an object.

```javascript
var map = new WeakMap();
var pavloHero = {
  first: "Pavlo",
  last: "Hero"
};
var gabrielFranco = {
  first: "Gabriel",
  last: "Franco"
};
map.set(pavloHero, "This is Hero");
map.set(gabrielFranco, "This is Franco");
console.log(map.get(pavloHero)); //This is Hero
```

The interesting aspect of the WeakMaps is the fact that it holds a weak reference to the key inside the map. A weak reference means that if the object is destroyed, the garbage collector will remove the entire entry from the WeakMap, thus freeing up memory.

## Q17: Explain why the following doesn't work as an IIFE. What needs to be changed to properly make it an IIFE?

```javascirpt
function foo(){ }();
```

Answer:
IIFE stands for Immediately Invoked Function Expressions. The JavaScript parser reads `function foo(){ }()`; as `function foo(){ }1 and 1()`;, where the former is a function declaration and the latter (a pair of brackets) is an attempt at calling a function but there is no name specified, hence it throws `Uncaught SyntaxError: Unexpected token )`.

Here are two ways to fix it that involves adding more brackets: `(function foo(){ })()` and `(function foo(){ }())`. These functions are not exposed in the global scope and you can even omit its name if you do not need to reference itself within the body.

You might also use `void` operator: `void function foo(){ }()`;. Unfortunately, there is one issue with such approach. The evaluation of given expression is always `undefined`, so if your IIFE function returns anything, you can't use it. An example:

```javascript
// Don't add JS syntax to this code block to prevent Prettier from formatting it.
const foo = void (function bar() {
  return "foo";
})();

console.log(foo); // undefined
```

## Q18: Could you compare usage of Module Pattern vs Constructor/Prototype pattern?

The module pattern is typically used for namespacing, where you'll have a single instance acting as a store to group related functions and objects. This is a different use case from what prototyping is good for. They're not really competing with each other; you can quite happily use both together (eg put a constructor-function inside a module and say `new MyNamespace.MyModule.MyClass(arguments))`.

Constructor-functions and prototypes are one of the reasonable ways to implement classes and instances. They don't quite correspond to that model so you typically need to choose a particular scheme or helper method to implement classes in terms of prototypes.

## Q19: What's the difference between ES6 Map and WeakMap?

They both behave differently when a object referenced by their keys/values gets deleted. Lets take the below example code:

`var map = new Map()`; `var weakmap = new WeakMap()`;

```javascript
(function() {
  var a = {
    x: 12
  };
  var b = {
    y: 12
  };

  map.set(a, 1);
  weakmap.set(b, 2);
})();
```

The above IIFE is executed there is no way we can reference `{x: 12}` and `{y: 12}` anymore. Garbage collector goes ahead and deletes the key b pointer from “WeakMap” and also removes `{y: 12}` from memory. But in case of “Map”, the garbage collector doesn’t remove a pointer from “Map” and also doesn’t remove `{x: 12}` from memory.

WeakMap allows garbage collector to do its task but not Map. With manually written maps, the array of keys would keep references to key objects, preventing them from being garbage collected. In native WeakMaps, references to key objects are held "weakly", which means that they do not prevent garbage collection in case there would be no other reference to the object.

## Q20: Can you give an example of a curry function and why this syntax offers an advantage?

**Currying** is a pattern where a function with more than one parameter is broken into multiple functions that, when called in series, will accumulate all of the required parameters one at a time. This technique can be useful for making code written in a functional style easier to read and compose. It's important to note that for a function to be curried, it needs to start out as one function, then broken out into a sequence of functions that each accepts one parameter.

```javascript
function curry(fn) {
  if (fn.length === 0) {
    return fn;
  }

  function _curried(depth, args) {
    return function(newArgument) {
      if (depth - 1 === 0) {
        return fn(...args, newArgument);
      }
      return _curried(depth - 1, [...args, newArgument]);
    };
  }

  return _curried(fn.length, []);
}

function add(a, b) {
  return a + b;
}

var curriedAdd = curry(add);
var addFive = curriedAdd(5);

var result = [0, 1, 2, 3, 4, 5].map(addFive); // [5, 6, 7, 8, 9, 10]
```

## Q21: How to "deep-freeze" object in JavaScript?

If you want make sure the object is deep frozen you have to create a recursive function to freeze each property which is of type object:

Without deep freeze:

```javascript
let person = {
  name: "Leonardo",
  profession: {
    name: "developer"
  }
};
Object.freeze(person); // make object immutable
person.profession.name = "doctor";
console.log(person); //output { name: 'Leonardo', profession: { name: 'doctor' } }
```

With deep freeze:

```javascript
function deepFreeze(object) {
  let propNames = Object.getOwnPropertyNames(object);
  for (let name of propNames) {
    let value = object[name];
    object[name] =
      value && typeof value === "object" ? deepFreeze(value) : value;
  }
  return Object.freeze(object);
}
let person = {
  name: "Leonardo",
  profession: {
    name: "developer"
  }
};
deepFreeze(person);
person.profession.name = "doctor"; // TypeError: Cannot assign to read only property 'name' of object
```

## Q22: Compare Async/Await and Generators usage to achive same functionality

- Generator function are executed yield by yield i.e one yield-expression at a time by its iterator (the next method) where as Async-await, they are executed sequential await by await.
- Async/await makes it easier to implement a particular use case of Generators.

* The return value of Generator is always {value: X, done: Boolean} where as for Async function it will always be a promise that will either resolve to the value X or throw an error.

- Async function can be **decomposed into Generator and promise** implementation like:
  ![compare picture](https://cdn-images-1.medium.com/max/2000/1*_rNhPf1WeuVIB_qwtlKmUQ.png)
