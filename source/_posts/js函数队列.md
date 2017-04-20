---
title: js函数队列
date: 2017-04-20 15:42:39
tags:
---

参考地址
- https://segmentfault.com/a/1190000008320677
- https://segmentfault.com/a/1190000008195180

代码一多，光看还是很容易头疼的，建议在答案代码中多打几个断点来看（智商不足的苦啊 TAT ）

## 函数队列的实现

### 基本思路

```javascript
var index = 0;
function next() {
    var fn = stack[index];
    index = index + 1; // 其实也可以用shift 把fn 拿出来
    if (typeof fn === 'function') fn();
}
var stack = [];
// 定义index 和next
function fn1() {
    console.log('第一个调用');
    next(); // stack 中每一个函数都必须调用`next`
};
stack.push(fn1);
function fn2() {
    setTimeout(function fn2Timeout() {
         console.log('第二个调用');
         next(); // 调用`next`
    }, 0);
}
stack.push(fn2, function() {
    console.log('第三个调用');
    next(); // 最后一个可以不调用，调用也没用。
});
next(); // 调用next，最终按顺序输出'第一个调用'、'第二个调用'、'第三个调用'。
```

### 尝试题目：Lazyman
```javascript
// 实现一个LazyMan，可以按照以下方式调用:
LazyMan(“Hank”)
/* 输出:
Hi! This is Hank!
*/
LazyMan(“Hank”).sleep(10).eat(“dinner”)输出
/* 输出:
Hi! This is Hank!
// 等待10秒..
Wake up after 10
Eat dinner~
*/
LazyMan(“Hank”).eat(“dinner”).eat(“supper”)
/* 输出:
Hi This is Hank!
Eat dinner~
Eat supper~
*/
LazyMan(“Hank”).sleepFirst(5).eat(“supper”)
/* 等待5秒，输出
Wake up after 5
Hi This is Hank!
Eat supper
*/
```


### 答案
```javascript
function _LazyMan(name) {
    // 事件存储队列
    this.tasks = [];
    // 绑定this指向
    var _this = this;
    // 使用闭包
    var fn = (function(n) {
        // 绑定作用域
        var name = n;
        return function() {
            console.log("my name is "+ name);
            _this.next();
        }
    })(name);
    this.tasks.push(fn);
    // 启动任务
    setTimeout(function() {
        _this.next();
    }, 0)
}
_LazyMan.prototype.next = function() {
  var fn = this.tasks.shift();
  fn && fn();
}
_LazyMan.prototype.wakeup = function(times) {
  var _this = this;
  var fn = (function(time){
    return function() {
      setTimeout(function(){
          console.log("getup after "+time+"s !")
          _this.next();
      }, time*1000)
    }
  })(times)
  // 从队列头部插入
  _this.tasks.unshift(fn);
  return this;
}
_LazyMan.prototype.eat = function(sth){
  var _this = this;
  var fn = (function(sth){
    return function(){
      console.log('Eat '+ sth +'~')
      _this.next();
    }
  })(sth);
  _this.tasks.push(fn);
  return this;
}
function LazyMan(name) {
  return new _LazyMan(name)
}
LazyMan('Hank').wakeup(3).eat('haha').eat('hehe');
```
