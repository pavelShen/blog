---
title: 读书笔记：深入浅出ES6（11）生成器 Generators，续篇
date: 2017-04-20 10:54:49
tags:
  - javascript
  - es6
---

原文线上地址：http://www.infoq.com/cn/es6-in-depth/

## Generators

#### 如何关停生成器

- generator.next()
- generator.return()
- generator.throw(error)
- yield*

**当生成器执行到一个yield表达式并暂停后可以实现以下功能：**
- 调用generator.next(value)，生成器从离开的地方恢复执行。
- 调用generator.return()，传递一个可选值，生成器只执行finally代码块并不再恢复执行。
- 调用generator.throw(error)，生成器表现得像是yield表达式调用一个函数并抛出错误。
- 或者，什么也不做，生成器永远保持冻结状态。（是的，对于一个生成器来说，很可能执行到一个try代码块，永不执行finally代码块。这种状态下的生成器可以被垃圾收集器回收。）

```javascript
var generator = function* () {
  for(let i = 0; i < 10; i++) {
    try {
      var value = yield i;
      console.log(i);
    } catch(e) {
      console.log("catch exception...");
    }
  }
};
var g = generator();
g.next();
g.throw();
```

#### .next参数

生成器的.next()方法接受一个可选参数，参数稍后会作为yield表达式的返回值出现在生成器中。那就是说，yield语句与return语句不同，它是一个只有当生成器恢复时才会有值的表达式。

```javascript
function* gen() {
  while(true) {
    var value = yield 'abc';
    console.log(value);
  }
}
var g = gen();
g.next(1);   // "{ value: null, done: false }"
g.next(2);   // "{ value: null, done: false }"
// 2
```
- 在该示例中，调用 next 方法并传入了参数，请注意，首次调用 next 方法时没有出任何输出, 这是因为初始状态时生成器通过yield 返回了'abc'.
- 暂停生成器，生成字符串值。
- 此时可以暂停任意长的时间。
- 最终，直到有人调用.next(2)，我们将这个对象(这里是数字2)存储在本地变量value中并继续执行下一行代码（输出console.log）。

#### 语法糖

普通yield表达式只生成一个值，而yield*表达式可以通过迭代器进行迭代生成所有的值。
```javascript
function* concat(iter1, iter2) {
      for (var value of iter1) {
        yield value;
      }
      for (var value of iter2) {
        yield value;
      }
    }
```
等效于

```javascript
function* concat(iter1, iter2) {
  yield* iter1;
  yield* iter2;
}
```
