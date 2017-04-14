---
title: js片段2
date: 2017-04-01 17:56:51
tags:
  - javascript
---

## 获取伪类的样式
参考文章：http://www.cnblogs.com/libin-1/p/6339978.html
```javascript
var pEle = document.getElementsByTagName('div')[0];
console.log(window.getComputedStyle(pEle,':after').color)
```
---

## encodeHTML & decodeHTML
```javascript
var HtmlUtil = {
    /*1.用浏览器内部转换器实现html转码*/
    htmlEncode:function (html){
        //1.首先动态创建一个容器标签元素，如DIV
        var temp = document.createElement ("div");
        //2.然后将要转换的字符串设置为这个元素的innerText(ie支持)或者textContent(火狐，google支持)
        (temp.textContent != undefined ) ? (temp.textContent = html) : (temp.innerText = html);
        //3.最后返回这个元素的innerHTML，即得到经过HTML编码转换的字符串了
        var output = temp.innerHTML;
        temp = null;
        return output;
    },
    /*2.用浏览器内部转换器实现html解码*/
    htmlDecode:function (text){
        //1.首先动态创建一个容器标签元素，如DIV
        var temp = document.createElement("div");
        //2.然后将要转换的字符串设置为这个元素的innerHTML(ie，火狐，google都支持)
        temp.innerHTML = text;
        //3.最后返回这个元素的innerText(ie支持)或者textContent(火狐，google支持)，即得到经过HTML解码的字符串了。
        var output = temp.innerText || temp.textContent;
        temp = null;
        return output;
    },
    /*3.用正则表达式实现html转码*/
    htmlEncodeByRegExp:function (str){  
         var s = "";
         if(str.length == 0) return "";
         s = str.replace(/&/g,"&amp;");
         s = s.replace(/</g,"&lt;");
         s = s.replace(/>/g,"&gt;");
         s = s.replace(/ /g,"&nbsp;");
         s = s.replace(/\'/g,"&#39;");
         s = s.replace(/\"/g,"&quot;");
         return s;  
   },
   /*4.用正则表达式实现html解码*/
   htmlDecodeByRegExp:function (str){  
         var s = "";
         if(str.length == 0) return "";
         s = str.replace(/&amp;/g,"&");
         s = s.replace(/&lt;/g,"<");
         s = s.replace(/&gt;/g,">");
         s = s.replace(/&nbsp;/g," ");
         s = s.replace(/&#39;/g,"\'");
         s = s.replace(/&quot;/g,"\"");
         return s;  
   }
};
```
---

## replace 参数

语法：str.replace( regexp|substr , newSubStr|function )

当第二个参数为一个function时，function的第一个参数为正则匹配到的值，第二个参数是第一个子表达式所匹配的值，依次类推

## 数组扁平化

#### es5 reduce

```javascript
var list1 = [[0, 1], [2, 3], [4, 5]];
var list2 = [0, [1, [2, [3, [4, [5]]]]]];

const flatten = arr => arr.reduce(
  (acc, val) => acc.concat(
    Array.isArray(val) ? flatten(val) : val
  ),
  []
);

function flatten2(arr){
  return arr.reduce(function(acc,val){
    return acc.concat(Array.isArray(val) ? flatten2(val) : val);
  },[])
}

console.log(flatten2(list2));
```

#### es3

```javascript
var list1 = [[0, 1], [2, 3], [4, 5]];
var list2 = [0, [1, [2,9, [3, [4, [5]]]]]];
function isArray(arr){
  return Object.prototype.toString.call(arr)==='[object Array]';
}
function flatten(arr,temp){
    temp = temp || [];
    for(var i=0;i<arr.length;i++){
        if( !isArray(arr[i]) ){
        temp.push(arr[i])
    }
    else{
        flatten(arr[i],temp)
    }
    }
  return temp
}
console.log(flatten(list1));
```

