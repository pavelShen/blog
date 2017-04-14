---
title: 读书笔记：深入浅出ES6（08）Symbols
date: 2017-04-14 10:54:48
tags:
  - javascript
  - es6
---

原文线上地址：http://www.infoq.com/cn/es6-in-depth/

## Symbols

#### 第七种原始类型

原来的六种类型：
1. Undefined 未定义
2. Null 空值
3. Boolean 布尔类型
4. Number 数字类型
5. String 字符串类型
6. Object 对象类型

#### 知识点
- symbol是程序创建并且可以用作属性键的值，并且它能避免命名冲突的风险。
- 调用Symbol()创建一个新的symbol，它的值与其它任何值皆不相等。
- 创建的变量类型为symbol，而不是一个字符串。除此之外，它与一个普通的属性没有什么区别。
- 不能被类似obj.name的点号法访问，你必须使用方括号访问这些属性
- 不能被for-in 遍历出来（ Object.getOwnPropertySymbols(obj)就可以列出对象的symbol键 ）
- symbol被创建后就不可变更，你不能为它设置属性

```javascript
var mySymbol = Symbol('abc');
var mySymbol2 = Symbol('abc');
console.log(mySymbol==mySymbol2); //false
var obj = {};
obj[mySymbol] = "ok!";
obj[mySymbol2] = "xxx";
console.log(obj[mySymbol]); // ok!
console.log(obj) //Object {Symbol(abc): "ok!", Symbol(abc): "xxx"}
```

#### 获取symbol的三种方法
- 调用Symbol()。正如我们上文中所讨论的，这种方式每次调用都会返回一个新的唯一symbol。
- 调用Symbol.for(string)。这种方式会访问symbol注册表，其中存储了已经存在的一系列symbol。这种方式与通过Symbol()定义的独立symbol不同，symbol注册表中的symbol是共享的。注册表非常有用，在多个web页面或同一个web页面的多个模块中经常需要共享一个symbol。
```javascript
var d = Symbol.for("cat");
var e = Symbol.for("cat");
console.log(typeof d) // symbol
console.warn(d===e) // true
```
- 使用标准定义的symbol，例如：Symbol.iterator。标准根据一些特殊用途定义了少许的几个symbol。
