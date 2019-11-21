---
title: js-Array-advanced
date: 2019-11-17 14:02:44
tags: js
---

在 Javascript 中，array 是一个类数组的 object。顾名思义，它能够在一个变量上存储多个值。

> 数组是值的有序集合。每个值叫做一个元素，而每个元素在数组中有一个位置，以数字表示，称为索引。JavaScript 数组是无类型：数组元素可以是任意类型，并且同一个数组中的不同元素也可能有不同的类型。 --《JavaScript 权威指南(第六版)》

array 在一般 Javascript object 基础上，有自己额外的属性。它采用 numbered index 作为的 key，有一个 length property 跟踪数组长度，还有如 push/pop、shift/unshift 等数组特有操作。

# 本质是 object

作为一个 object，我们可以在 array 上面进行所有 object 的合法操作，比如设置一个 named key。

```javascript
var test = [];
test.fruit = "APPLE";
console.log(test.fruit); // APPLE
```

上面仅仅把 array 作为一个普通的 Javascript object 使用，等价于 var test = {}。在这种场合，我们应该选择普通的 Javascirpt object，而非 array。

# Numbered index

Javascript 中的 array object 采用了一个很朴素的思想来实现数组 —— 用数字来充当 object 的 keys。这样在表面上延续了我们在 C++/Java 上使用数组的编程体验。

```javascirpt
{
    0 : item0,
    1 : item1,
    2 : item2
}
```

array 的这种实现方式导致了一些尴尬问题，比如删除元素、元素遍历。我们会在后面谈到这些问题。

# 删除元素

delete operator（不推荐）

和一般 Javascript object 一样，我们可以使用 delete 来删除 object 中的 property。

```javascript
var arr = ["a", "b", "c", "d"];
delete arr[1];
console.log(arr);
console.log(arr[1]);
// [ 'a', , 'c', 'd' ]
// undefined
```

当我们使用 delete 来删除某一个元素 arr[1]时，arr 中 key 1 和 value b 之间的连接被切断，key 1 对应的值被重置为 undefined。注意，这时候，array 中其他元素并没有改变自身的 index 来填补这 key 1 这个空洞，而是保持原值。这样，key 1 就变成了 arr 中的一个洞了！

除非有特殊需求，比如需要用到 sparse array，一般情况下并不推荐使用 delete operator 来删除 array 中的元素。

splice() （推荐）

```javascript
var fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.splice(0, 1); // Removes the first element of fruits
```

第一个参数 (0) 定义新元素要从哪个位置插入。第二个参数 (1) 定义新元素插入位置开始有多少元素要被删除。后面的参数被忽略掉了，表示并没有要插入的元素。

filter()（推荐）

```javascript
var fruits = ["Banana", "Orange", "Apple", "Mango"];
var toDelete = "Apple";
fruits = fruits.filter(function(value) {
  return value != toDelete;
});
// [ 'Banana', 'Orange', 'Mango' ]
```

理解 length

length 是 array object 的一个 property， 根据名字来看，似乎是记录 array 的长度。其实，length 的本质是跟踪 array 中的 max_index，并始终保持值是 max_index + 1。

这和记录长度有什么区别呢？

还记得使用 delete 来删除元素的情形吧？中间元素被删了，但是其他元素没有改变自身的 index 来填补空间。这时候即使 delete 了多个元素，数组的 length 可能并没有发生变化。

```javascript
var array_object = [1, 2, 3];
console.log(array_object.length); // 3
delete array_object[1];
console.log(array_object.length); // 3!
```

Array 遍历

如果 array 中没有“洞”， for loop 和 forEach 两种遍历方式区别不大。在 array 有洞的情况下，两者略有不同，其中 forEach 会跳过这些洞，而传统的 for loop 并不会。

```javascript
var array_object = [1, 2, 3];
delete array_object[1];

// 不跳过洞
for (var i = 0; i < array_object.length; i++) {
  console.log(array_object[i]);
}
//1 undefined 3

//跳过洞
array_object.forEach(function(s) {
  console.log(s);
});
// 1 3
```

遍历 array 的方法还有其他，更多方法可以参考这篇[StackOverflow 问答。](http://stackoverflow.com/q/3010840/3697757)

[] vs. new Array()

当新建一个 array 时候，我们有下面两种方式: array literal [] 或 array constructor new Array(arg)

```javascript
var arrayA = [];
var arrayB = new Array();
```

当使用[]时候， JS engine 会直接调用 global Array 的 constructor 新建一个 array 变量并返回。当使用 new Array()时候， JS engine 会沿着 Execution Context 往上追溯到名为 Array 的 constructor function，并据此生成一个 object。这时候，虽然不大可能，Array 可能会在中间某个 Execution Context 中被用户重新定义。下面是 coderjoe 在 [StackOverflow](http://stackoverflow.com/a/1273936/3697757) 中提出的例子。在例子中，我们最后得到的并不是我们期待的原生 Array。

```javascript
function Array() {
  this.is = "SPARTA";
}
var a = new Array();
var b = [];
alert(a.is);
// => 'SPARTA'
alert(b.is);
// => undefineda.push('Woa');
// => TypeError: a.push is not a functionb.push('Woa');
// => 1 (OK)
```

大部分 Javascirpt 社区推荐使用[]来新建 array。

> You never need to use new Object() in JavaScript. Use the object literal {} instead. Similarly, don’t use new Array(), use the array literal [] instead. Arrays in JavaScript work nothing like the arrays in Java, and use of the Java-like syntax will confuse you. [LINK](http://yuiblog.com/blog/2006/11/13/javascript-we-hardly-new-ya/)

Array 复制

浅度复制

```javascript
var arr1 = ["a", "b", "c"];
var arr2 = arr1;
arr2[0] = 1; // 对数组arr2的元素进行修改
console.log(arr1); // [1, 'b', 'c']
```

深度复制

```javascript
var arr1 = ["a", "b", "c", "d", "e"];
var arr2 = arr1.concat(); // 使用concat()方法，返回新的数组
arr2[0] = 1;
console.log(arr1); // => ['a', 'b', 'c', 'd', 'e']：数组arr1的元素没变更
console.log(arr2); // => [ 1, 'b', 'c', 'd', 'e']：数组arr2的元素发生了变更
```

Associate Array

在计算机科学中，采用 named index 而非 numbered index 的数组被称为[Associative Array。](https://en.wikipedia.org/wiki/Associative_array)

```javascript
var associative_array = new Array();
associative_array["one"] = "Lorem";
associative_array["two"] = "Ipsum";
associative_array["three"] = "dolor";

for (i in associative_array) {
  console.log(i);
}
// one two three
```

上面操作可以应用在任何 Javascript object 上，array object 也不例外。但是在 Javascript array 上进行这种操作是很糟糕的。当需要使用 named string 作为 key 时候，我们应该使用一般的 object，而非 array。

# Javascript 里面并不支持 named index 的 array。

> If you use a named index, JavaScript will redefine the array to a standard object. After that, all array methods and properties will produce incorrect results... In JavaScript, arrays always use numbered indexes. [LINK](http://www.w3schools.com/js/js_arrays.asp)

# Reference

- [Iterating over an array with “holes” in JavaScript](http://stackoverflow.com/a/13847793/3697757)
- [Create an empty object in JavaScript with {} or new Object()?](http://stackoverflow.com/a/5665204/3697757)
- [What’s the difference between “Array and “[]” while declaring a JavaScript array?](http://stackoverflow.com/q/931872/3697757)
- [JavaScript Array 对象](http://www.cnblogs.com/polk6/p/3542523.html)
