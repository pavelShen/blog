---
title: javascript设计模式学习-01
date: 2017-08-05 17:13:35
tags:
---

原文：http://www.cnblogs.com/tugenhua0707/p/5198407.html

## 工场模式 
类的继承
```javascript
// 定义自行车的构造函数
var BicycleShop = function(name){
    this.name = name;
    this.method = function(){
        return this.name;
    }
};
BicycleShop.prototype = {
    constructor: BicycleShop,
    /*
     * 买自行车这个方法
     * @param {model} 自行车型号
    */
    sellBicycle: function(model){
            var bicycle = this.createBicycle(model);
            // 执行A业务逻辑
            bicycle.A();
            // 执行B业务逻辑
            bicycle.B();
            return bicycle;
        },
        createBicycle: function(model){
            throw new Error("父类是抽象类不能直接调用，需要子类重写该方法");
        }
    };
    // 实现原型继承
    function extend(Sub,Sup) {
        //Sub表示子类，Sup表示超类
        // 首先定义一个空函数
        var F = function(){};
        // 设置空函数的原型为超类的原型
        F.prototype = Sup.prototype; 
        // 实例化空函数，并把超类原型引用传递给子类
        Sub.prototype = new F();

        // 重置子类原型的构造器为子类自身
        Sub.prototype.constructor = Sub;

        // 在子类中保存超类的原型,避免子类与超类耦合
        Sub.sup = Sup.prototype;
        if(Sup.prototype.constructor === Object.prototype.constructor) {
            // 检测超类原型的构造器是否为原型自身
            Sup.prototype.constructor = Sup;
        }
    }
    var BicycleChild = function(name){
        this.name = name;
// 继承构造函数父类中的属性和方法
        BicycleShop.call(this,name);
    };
    // 子类继承父类原型方法
    extend(BicycleChild,BicycleShop);
// BicycleChild 子类重写父类的方法
BicycleChild.prototype.createBicycle = function(){
    var A = function(){
        console.log("执行A业务操作");    
    };
    var B = function(){
        console.log("执行B业务操作");
    };
    return {
        A: A,
        B: B
    }
}
var childClass = new BicycleChild("龙恩");
console.log(childClass);
```

## 单例模式
比如我现在页面上需要创建一个div的元素，那么我们肯定需要有一个创建div的函数，而现在我只需要这个函数只负责创建div元素，其他的它不想管，也就是想实现单一职责原则

```javascript
// 单体模式
var Singleton = function(name){
    this.name = name;
};
Singleton.prototype.getName = function(){
    return this.name;
}
// 获取实例对象
var getInstance = (function() {
    var instance = null;
    return function(name) {
        if(!instance) {
            instance = new Singleton(name);
        }
        return instance;
    }
})();
// 测试单体模式的实例
var a = getInstance("aa");
var b = getInstance("bb");
```

防止重复生成弹窗实例（减少重绘，提升性能）
例子：
```javascript
// 实现单体模式弹窗
var createWindow = (function(){
    var div;
    return function(){
        if(!div) {
            div = document.createElement("div");
            div.innerHTML = "我是弹窗内容";
            div.style.display = 'none';
            document.body.appendChild(div);
        }
        return div;
    }
})();
document.getElementById("Id").onclick = function(){
    // 点击后先创建一个div元素
    var win = createWindow();
    win.style.display = "block";
}
```

封装
```javascript
// 创建div
var createWindow = function(){
    var div = document.createElement("div");
    div.innerHTML = "我是弹窗内容";
    div.style.display = 'none';
    document.body.appendChild(div);
    return div;
};
// 创建iframe
var createIframe = function(){
    var iframe = document.createElement("iframe");
    document.body.appendChild(iframe);
    return iframe;
};
// 获取实例的封装代码
var getInstance = function(fn) {
    var result;
    return function(){
        return result || (result = fn.call(this,arguments));
    }
};
// 测试创建div
var createSingleDiv = getInstance(createWindow);
document.getElementById("Id").onclick = function(){
    var win = createSingleDiv();
    win.style.display = "block";
};
// 测试创建iframe
var createSingleIframe = getInstance(createIframe);
document.getElementById("Id").onclick = function(){
    var win = createSingleIframe();
    win.src = "http://cnblogs.com";
};
```
