---
title: 读书笔记：深入浅出ES6（12）代理 Proxies
date: 2017-04-20 10:54:51
tags:
  - javascript
  - es6
---

原文线上地址：http://www.infoq.com/cn/es6-in-depth/

## 代理 Proxies

**警告：该功能现在无法通过polyfill进行降级处理，所以不要在浏览器端使用，在Node.js或io.js环境中使用代理，首先你需要添加名为harmony-reflect的polyfill，然后在执行时启用一个非默认的选项（--harmony_proxies）**

#### 语法

ES6规范定义了一个全新的全局构造函数：代理（Proxy）。它可以接受两个参数：目标对象（target）与句柄对象（handler）

```javascript
var target = {}, handler = {};
var proxy = new Proxy(target, handler);

proxy.color = "pink";
target.color //pink
```
代理的行为很简单：将代理的所有内部方法转发至目标。简单来说，如果调用proxy.\[[Enumerate]]()，就会返回target.\[[Enumerate]]()。

#### 内部方法

我们可以在[ES6标准列表5和6](http://www.ecma-international.org/ecma-262/6.0/index.html#sec-proxy-object-internal-methods-and-internal-slots)中找到全部的14种方法。双方括号[[ ]]代表内部方法，在一般的JS代码中不可见，你可以调用、删除或覆写普通方法，但是无法操作内部方法。

在整个ES6标准中，只要有可能，任何语法或对象相关的内建函数都是基于这14种内部方法构建的。ES6在对象的中枢系统周围划分了一个清晰的界限，你可以借助代理特性用任意JS代码替换标准中枢系统的内部方法。

#### 代理句柄
句柄对象的方法可以覆写任意代理的内部方法。
```javascript
var target = {};
var handler = {
      set: function (target, key, value, receiver) {
        throw new Error("请不要为这个对象设置属性。");
      }
};
var proxy = new Proxy(target, handler);
proxy.name = "angelina"; //Error: 请不要为这个对象设置属性。
```
