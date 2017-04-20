---
title: 读书笔记：深入浅出ES6（14）let & const
date: 2017-04-20 10:54:53
tags:
  - javascript
  - es6
---

原文线上地址：http://www.infoq.com/cn/es6-in-depth/

## let & const

#### var的缺陷

- JS没有块级作用域
- 循环内变量过度共享

#### 知识点

- 变量提升：JS引擎将所有var声明和function函数声明都提升到函数内的最高处。
- let声明的全局变量不是全局对象的属性。这就意味着，你不可以通过window.变量名的方式访问这些变量。它们只存在于一个不可见的块的作用域中，这个块理论上是Web页面中运行的所有JS代码的外层块。
- TDZ临时死区（Temporal Dead Zone
```javascript
let x = 'outer value'
(function() {
  // 这里会产生 TDZ for x
  console.log(x) // TDZ期间访问，产生ReferenceError错误
  let x = 'inner value' // 对x的声明语句，这里结束 TDZ for x
}())
```
