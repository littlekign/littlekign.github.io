---
title: JS-Syntax-Parser
date: 2019-11-17 15:27:31
tags: js
---

# Javascript Syntax Parser

一门语言的执行，大致经历下面这些过程：词法分析 -- 语法分析 -- 语义分析 -- 中间代码生成 -- 优化代码 -- 代码生成。

在 Javascript 中，Syntax Parser 的作用是进行词法分析和语法分析。

> A program that reads your code and determines what it does and if its grammar is valid.

词法分析挨个字符地扫描代码，把关键 token 识别出来。语法分析利用词法分析的结果建立上下文关系语法树 Abstract Syntax Tree (AST)。一般情况下，我们不会直接和语法树打交道，但会在进行 Uglify 代码压缩、IDE 语法高亮、Babel 重编译、关键字匹配和作用域判断时间接涉及到。

```javascript
var AST = "is Tree";
```

![Js ast](http://azu.github.io/slide/JSojisan/resources/ast-is-true.png)

传统的 Javascript 引擎直接根据语法树的的结果进行解释执行，导致效率比 C/C++较为低下。一些最新的 Javascript 引擎（如 V8），会将部分 Javascript 代码编译成为目标代码以提高执行效率。

介绍几个 Javascript 的 Syntax Parser

- [esprima](https://github.com/ariya/esprima)
- [acorn](https://github.com/ternjs/acorn)

# Reference

[javascript-ast-tutorial](http://wwsun.github.io/posts/javascript-ast-tutorial.html)
[javascript-syntax-tree](http://purplebamboo.github.io/2014/09/27/javascript-syntax-tree)
