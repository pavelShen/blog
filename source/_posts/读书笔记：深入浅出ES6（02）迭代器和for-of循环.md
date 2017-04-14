---
title: 读书笔记：深入浅出ES6（02）迭代器和for-of循环
date: 2017-04-14 10:54:44
tags:
  - javascript
  - es6
---

原文线上地址：http://www.infoq.com/cn/es6-in-depth/

## 迭代器和for-of循环

### forEach缺点:
- ES5的forEach无法break中断，也不能通过return语句返回到外层函数

### for-in 缺点：
 - index的值是字符串的'0','1','2'
 - 会遍历到**自定义**的属性（包括原型链上的自定义属性）
```javascript
    var arr = ['a','b','c'];
    arr.name = "fff";

    for(var index in arr){
      console.log(arr[index]) //a,b,c,fff
    }
```
- 无法保证遍历顺序

### for-of 优势
- 支持大多数类数组对象，如DOM NodeList对象
- 支持字符串遍历
- 支持Map和Set对象
```javascript
for (var [key, value] of phoneBookMap) {
   console.log(key + "'s phone number is: " + value);
}
```

### 应用
- 使jquery支持for-of循环
```javascript
jQuery.prototype[Symbol.iterator] = Array.prototype[Symbol.iterator];
```

### 原理
for-of循环首先调用集合的\[Symbol.iterator]()方法，紧接着返回一个新的迭代器对象。迭代器对象可以是任意具有next()方法的对象；for-of循环将重复调用这个方法，每次循环调用一次。
```javascript
var students = {}
students[Symbol.iterator] = function() {
  let index = 1;
  return {
    next() {
      return {done: index>100, value: index++}
    }
  }
}
for(var i of students) {
  console.log(i);
}  
```



<!--more-->
