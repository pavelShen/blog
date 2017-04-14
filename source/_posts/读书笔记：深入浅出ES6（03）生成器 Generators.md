---
title: 读书笔记：深入浅出ES6（03）生成器 Generators
date: 2017-04-14 10:54:45
tags:
  - javascript
  - es6
---

原文线上地址：http://www.infoq.com/cn/es6-in-depth/

## 生成器 Generators

### 语法
```javascript
function* quips(name) {
  yield "你好 " + name + "!";
  yield "希望你能喜欢这篇介绍ES6的译文";
  yield "我们下次再见！";
}

var iter = quips("jorendorff");    //[object Generator]
iter.next()     //{ value: "你好 jorendorff!", done: false }
iter.next()     //{ value: "希望你能喜欢这篇介绍ES6的译文", done: false }
iter.next()     //{ value: "我们下次再见！", done: false }
iter.next()      //{ value: undefined, done: true }
```

### 普通函数和生成器函数区别

- 普通函数使用function声明，而生成器函数使用function*声明。
- 在生成器函数内部，有一种类似return的语法：关键字yield。二者的区别是，普通函数只可以return一次，而生成器函数可以yield多次（当然也可以只yield一次）。在生成器的执行过程中，遇到yield表达式立即暂停，后续可恢复执行状态。

### How it work
1. 当你调用一个生成器函数时，它并非立即执行，而是返回一个已暂停的生成器对象。你可将这个生成器对象视为一次函数调用，只不过立即冻结了，它恰好在生成器函数的最顶端的第一行代码之前冻结了。
2. 每当你调用生成器对象的.next()方法时，函数调用将其自身解冻并一直运行到下一个yield表达式，再次暂停。
3. 调用最后一个iter.next()时，我们最终抵达生成器函数的末尾，所以返回结果中done的值为true。抵达函数的末尾意味着没有返回值，所以返回结果中value的值为undefined。

### 知识点
- **生成器不是线程** 当生成器运行时，它和调用者处于同一线程中，拥有确定的连续执行顺序，永不并发
- **生成器是迭代器** 所有的生成器都有内建.next()和\[Symbol.iterator]()方法的实现。你只须编写循环部分的行为。

### 前一章的例子用生成器改写：
```javascript
function* students(index){
  for(var i=1;i<=index;i++){
    yield i
  }
}
var g = students(5);
console.log(g.next());
console.log(g.next());
console.log(g.next());
console.log(g.next());
console.log(g.next());
console.log(g.next());
```
