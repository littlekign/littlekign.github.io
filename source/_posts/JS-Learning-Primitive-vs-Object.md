---
title: JS Learning Primitive vs Object
date: 2019-11-15 09:33:03
tags: js
---

#### Javascript 有两种基本数据类型，Primitive 和 Object。Object 是 properties 的聚合，其 property 可以是 Object 也可以是 Primitive。Primitive 只有 value, 没有 properties。Javascript 有五种 Primitive:

- string
- number
- boolean
- null
- undefined

除了 null 和 undefined，其余 Primitive 都有对应的 Object 封装，如 Object String 对应 string。

# Stack vs Heap

Javascript 是一门动态语言，同一个变量在不同时候可能被赋予不同数据类型，比如前面是 number 而后面变成 Object。所以 Javascript 变量值所占用的内存空间应该是可以动态变化的。

```javascript
var a = 10;
a = {};
```

实际上，不管是 Primitive 还是 Object，这些 Javascript 变量的值都存储在可以动态分配空间的 heap 上。而它在 stack 上保存的则是一个固定 size 的包含该值所在 heap 位置信息的引用（类似 pointer）。JS engine 通过对引用指向的修改来实现同一个变量指向不同的数据类型

![Javascript memory model](http://tva1.sinaimg.cn/large/0080xEK2ly1g8yqay5bdwj30xc0onjs6.jpg)

> It's wrong to state that primitives are allocated from the stack and only objects are allocated from the heap. This is the biggest difference between C and JavaScript.

最新的 JS engine 做了很多优化，使得内存分配更为高效但也更为复杂，具体可参考该 [StackOverflow](http://stackoverflow.com/a/10618981/3697757) 回答。

# 值传递 vs 引用传递

Primitive 是值传递（passed by value），Object 是引用传递（passed by reference）。

上面说到，Javascript 变量在 stack 保存的是地址引用，为什么又说 Primitive 是值传递呢？Javascript 中所有 Primitive 都是 immutable 的。在传递 Primitive 时，JS engine 在 heap 上复制一个值相同的 Primitive，然后把新变量的引用传递出去，这就是所谓 JS 中的值传递。对于 Object，JS engine 并不会复制一个新的 Object，而是直接传递它的地址引用（与 C++/Java 中的引用传递类似）。

> 该引用能识别其指向的变量是 Primitive 还是 Object，所以它不是一个简单的 pointer，而是包含所指向变量类型信息的“智能引用”。

# 对 Prototype 影响

在 Javascript 中，子类修改父类的 property 会影响到所有继承该父类的子类。但这影响会根据父类 property 是属于 Primitive 还是 Object 而有微妙差别。我们设想有一个父类 father，和两个继承了他的子类 son1 和 son2。

```javascript
var father = {
  primitive: 1,
  object: {
    fruit: "APPLE"
  }
};
var son1 = Object.create(father);
var son2 = Object.create(father);
```

# Primitive Property

如果父类的 property 是 Primitive，该 property 通过 passed by value 传递到子类。这时，子类修改该 property 不会影响到父类本身，更不会传递到其他子类上。

```javascript
console.log("father's primitive is " + father.primitive);
console.log("son1's primitive is " + son1.primitive);
console.log("son2's primitive is " + son2.primitive);
// father's primitive is 1
// son1's primitive is 1
// son2's primitive is 1

son1.primitive = 2;

console.log("father's primitive is " + father.primitive);
console.log("son1's primitive is " + son1.primitive);
console.log("son2's primitive is " + son2.primitive);
// father's primitive is 1
// son1's primitive is 2
// son2's primitive is 1
```

# Object Property

如果父类的 property 是 Object，该 property 通过 passed by reference 传递到子类上。子类修改该 property 会通过父类传递到所有继承它的子类上。

```javascript
console.log("father's fruit is " + father.object.fruit);
console.log("son1's fruit is " + son1.object.fruit);
console.log("son2's fruit is " + son2.object.fruit);
// father's fruit is APPLE
// son1's fruit is APPLE
// son2's fruit is APPLE

son1.object.fruit = "GRAPE";

console.log("father's fruit is " + father.object.fruit);
console.log("son1's fruit is " + son1.object.fruit);
console.log("son2's fruit is " + son2.object.fruit);
// father's fruit is GRAPE
// son1's fruit is GRAPE
// son2's fruit is GRAPE
```

# Reference

[Primitive value vs Reference value](http://stackoverflow.com/a/13268731/3697757)
[How variables are allocated memory in Javascript?](http://stackoverflow.com/a/10618981/3697757)
