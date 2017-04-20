---
title: 读书笔记：深入浅出ES6（13）类 Class
date: 2017-04-20 10:54:52
tags:
  - javascript
  - es6
---

参考地址：
http://es6.ruanyifeng.com/#docs/class
https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Classes

## 类 Class

#### static

static 关键字用来定义类的静态方法。静态方法是指那些不需要对类进行实例化，使用类名就可以直接访问的方法，需要注意的是**静态方法不能被实例化的对象调用**。静态方法经常用来作为工具函数。

```javascript
class Point {
    constructor(x, y) {
        this.x = x;
        this.y = y;
    }
    static distance(a, b) {
        const dx = a.x - b.x;
        const dy = a.y - b.y;
        return Math.sqrt(dx*dx + dy*dy);
    }
}
const p1 = new Point(5, 5);
const p2 = new Point(10, 10);
console.log(Point.distance(p1, p2));
```

#### extends & super
- extends 关键字在类声明或类表达式中用于创建一个类作为另一个类的一个子类。
- 如果子类中存在构造函数，则需要在使用“this”之前首先调用super（）。
- ES6 要求，子类的构造函数必须执行一次super函数。否则在调用的时候会报错
- 由于super指向父类的原型对象，所以定义在父类实例上的方法或属性，是无法通过super调用的，super.haha()就相当于调用Point.prototype.haha()。
- 如果不希望创建出来的实例继承自Object.prototype，你甚至可以在extends后使用null来进行声明。
```javascript
class Point {
  constructor(x, y) {
    this.x = x;
    this.y = y;
    this.hehe = 'hehe'
  }
  haha(){
    return 'haha'
  }
}
class ColorPoint extends Point {
  constructor(x, y, color) {
    super(x, y);
    this.color = color; // 正确
  }
  hello(){
    console.log('haha:' + super.haha());
    console.log('hehe:' + super.hehe);
  }
}
var cp = new ColorPoint()
cp.hello();
// haha:haha
// hehe:undefined
```
