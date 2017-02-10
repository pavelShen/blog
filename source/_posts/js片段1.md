---
title: js片段1
date: 2017-01-17 13:12:33
tags:
  - javascript
---

## 获取url中的参数 ##

```
function getUrlParam(key){
    var obj = {};
    var str = location.search.substring(1);
    var arr = str.split("&");
    var name,value;
    if(str.length>0){
        for(var i=0,length=arr.length;i<length;i++){
            var num = arr[i].indexOf("=");
            if(num>0){
                name =arr[i].substring(0,num);
                value=arr[i].substring(num+1);
            }
            obj[name] = value;
        }
    }
    return obj[key];
}
```

---

<!--more-->

## 深度克隆 ##

来源：http://www.tuicool.com/articles/6FvYFfn

```
var cloneObj = function(obj) {
    var str, newobj = obj.constructor === Array ? [] : {};
    if (typeof obj !== 'object') {
        return;
    } else if (window.JSON) {
        str = JSON.stringify(obj), newobj = JSON.parse(str);
    } else {
        for (var i in obj) {
            newobj[i] = typeof obj[i] === 'object' ? cloneObj(obj[i]) : obj[i];
        }
    }
    return newobj;
};
var obj = { a: 0, b: 1, c: 2 };
var arr = [0, 1, 2];
var newobj = cloneObj(obj);
var newarr = cloneObj(arr);
delete newobj.a;
newarr.splice(0, 1);
console.log(obj, arr, newobj, newarr); //结果： {a: 0, b: 1, c: 2}, [0, 1, 2], {b: 1, c: 2}, [1, 2];
```

---

## 设置返回页面 ##

```
if(document.referrer===''){
  //iphone 6 (without plus) load auto popstate
  setTimeout(function(){
    window.onpopstate = function(event) {
      location.replace("http://"+location.host+"/"+CUR_DATA.langs+"/")
    };
    history.pushState({}, "");
  },1000);
}
```

---

## 通过setTimeout实现输入框与页面元素内容同步 ##

来源：http://ghmagical.com/article/page/id/H61NOVU0RZ9Y
```
document.querySelector('#one input').onkeydown = function() {   
  document.querySelector('#one span').innerHTML = this.value;   
};   
document.querySelector('#second input').onkeydown = function() {   
  setTimeout(function() {   
    document.querySelector('#second span').innerHTML = document.querySelector('#second input').value;
  }, 0);
};
```
**原理：**
未使用setTimeout函数，执行顺序是：onkeydown => onkeypress => onkeyup
使用setTimeout函数，执行顺序是：onkeydown => onkeypress => function => onkeyup
虽然我们可以使用keyup来替代keydown，不过有一些问题，那就是长按时，keyup并不会触发。

长按时，keydown、keypress、keyup的调用顺序：
```
keydown
keypress
keydown
keypress
...
keyup
```
也就是说keyup只会触发一次，所以你无法用keyup来实时获取值。

**附赠：**
setTimeout 第三第四个参数
```
setTimeout(function(a, b){   
  console.log(a); // 3
  console.log(b); // 4
},0, 3, 4);
```
