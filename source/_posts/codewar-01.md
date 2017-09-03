---
title: codewar-01
date: 2017-09-03 16:53:14
tags:
---

codewar刷题之旅开始啦~

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