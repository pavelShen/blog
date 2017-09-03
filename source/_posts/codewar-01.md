---
title: codewar-01
date: 2017-09-03 16:53:14
tags:
  - codewar
---

# codewar刷题之旅开始啦~

## Binary Addition（二进制加法）

#### toString

```javascript
function addBinary(a,b){
  return (a+b).toString(2)
}
```

#### 原生
```javascript
function addBinary(a,b) {
  var sum = Math.abs(a+b)
  var sumPrefix = a+b>=0 ? '' : '-'
  var result = 0
  
  while(sum>0){
    result = ((sum % 2) + result).toString()
    sum = Math.floor(sum / 2)
  }
  
  return sumPrefix+result
}
```

---

## Find The Parity Outlier （寻找奇偶异常点）

[2, 4, 0, 100, 4, 11, 2602, 36]

Should return: 11

[160, 3, 1719, 19, 11, 13, -21]

Should return: 160

#### 注意点
使用javascript取余的时候，负数得到的余值也为负数

#### 自己写的，偷懒多循环了3个数字

```javascript
function findOutlier(integers){
  let arrayType,isOdd=0,isEven=0
  
  for(let i=0;i<3;i++){
    Math.abs(integers[i]%2)===0 ? (isEven ++) : (isOdd ++)
  }
  
  arrayType = (isEven>isOdd)?'even':'odd'
  
  for(let item of integers){
    if(arrayType==='even'){
      if(Math.abs(item % 2) === 1){
        return item
      }
    }
    else{
      if(Math.abs(item % 2) === 0){
        return item
      }
    }
  }
}
```

#### 利用了filter返回数组的特性
```javascript
function findOutlier(int){
  var even = int.filter(a=>a%2==0);
  var odd = int.filter(a=>a%2!==0);
  return even.length==1? even[0] : odd[0];
}
```

---

## Jaden Casing Strings（首字母大写）

比较令人不解的是 为何第一个正则里面如果直接返回p1.toUpperCase() 会将空格替换掉

#### 又low了，先处理了首字母，再处理空格后面的英文

```javascript
String.prototype.toJadenCase = function () {
  let reg = /\s([a-z])/g
  let handledStr = handleFirstLetter(this)
  return handledStr.replace(reg,(match,p1)=>{
    return ' '+p1.toUpperCase()
  })
};

function handleFirstLetter(str){
  let reg = /^[a-z]/
  return str.replace(reg,(match)=>{
    return match.toUpperCase()
  })
}
```

#### 先拆了再组合是个很好的思路
```javascript
String.prototype.toJadenCase = function () { 
  return this.split(" ").map(function(word){
    return word.charAt(0).toUpperCase() + word.slice(1);
  }).join(" ");
}
```

#### 一个正则搞定
```javascript
String.prototype.toJadenCase = function () {
  return this.replace(/(^|\s)[a-z]/g, function(x){     return x.toUpperCase(); });
  };
```