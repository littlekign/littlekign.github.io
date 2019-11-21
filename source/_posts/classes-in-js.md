---
title: classes-in-js
date: 2017-1-21 6:23:35
tags: js class
---

## Introduction

JavaScript is a prototype-based language, and every object in JavaScript has a hidden internal property called `[[Prototype]]` that can be used to extend object properties and methods. You can read more about prototypes in our Understanding Prototypes and Inheritance in JavaScript tutorial.

Until recently, industrious developers used constructor functions to mimic an object-oriented design pattern in JavaScript. The language specification ECMAScript 2015, often referred to as ES6, introduced classes to the JavaScript language. Classes in JavaScript do not actually offer additional functionality, and are often described as providing “syntactical sugar” over prototypes and inheritance in that they offer a cleaner and more elegant syntax. Because other programming languages use classes, the class syntax in JavaScript makes it more straightforward for developers to move between languages.

## Classes Are Functions

A JavaScript class is a type of function. Classes are declared with the `class` keyword. We will use function expression syntax to initialize a function and class expression syntax to initialize a class.

```js
// Initializing a function with a function expression
const x = function() {};
```

```js
// Initializing a class with a class expression
const y = class {};
```

We can access the `[[Prototype]]` of an object using the `Object.getPrototypeOf()` method. Let’s use that to test the empty `function` we created.

```js
Object.getPrototypeOf(x);
```

```js
Output
ƒ () { [native code] }
```
