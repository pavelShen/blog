---
title: codewar-03
date: 2017-09-05 00:50:14
tags:
  - codewar
---

# codewar刷题之旅开始啦~

## Format a string of names like 'Bart, Lisa & Maggie'.

#### 自己写的
偷鸡的使用了replace的分组，如何用正则获取最后一次出现的指定字符可以研究下
`/,(?!.*,)/`  逻辑：匹配后面没有逗号的逗号

```javascript
function list(names){
  let result
  let nameArr = names.map(function(item){
    return item.name
  })
  
  result = nameArr.join(', ')
  return result.replace(/(.*)(,\s)/,function(match,p1,p2){
		  return p1 + ' & '
		})
}
```

#### reduce 感觉这题就是给reduce打造的。。。
```javascript
function list(names){
  return names.reduce(function(prev, current, index, array){
    if (index === 0){
      return current.name;
    }
    else if (index === array.length - 1){
      return prev + ' & ' + current.name;
    } 
    else {
      return prev + ', ' + current.name;
    }
  }, '');
 }
```


#### 把最后一个名字用pop弹出并存起来（不错的想法）
```javascript
function list(names) {
  var xs = names.map(p => p.name)
  var x = xs.pop()
  return xs.length ? xs.join(", ") + " & " + x : x || ""
}
```

---

## Sum of two lowest positive integers （数组最小的2个值相加）

#### 先排序再求和，但是按照算法复杂度来说，复杂了
```javascript
function sumTwoSmallestNumbers(numbers) {  
  //Code here
  let temp = numbers.slice()
  let sortedNum = temp.sort((a,b) => a-b)
  return sortedNum[0] + sortedNum[1]
};
```

#### 设置2个变量获取最小的2个值，和自己写的demo中获取最大的2个值 是一个思路

```javascript
function sumTwoSmallestNumbers(numbers) {  
  var min = Number.MAX_SAFE_INTEGER;
  var secondMin = Number.MAX_SAFE_INTEGER;
  
  var n;
  for (i = 0; i < numbers.length; i++) {
    n = numbers[i];
    if(n < min) {
      secondMin = min;
      min = n;
    } else if( n < secondMin ) {
      secondMin = n;
    }
  }
  
  return min + secondMin;
}
```

---

## Get the Middle Character(获取字符串中间的部分，长度奇数返回1个字符，偶数返回2个)

```javascript
function getMiddle(s){
  //Code goes here!
  let result
  let length = s.length
  let halfIndex = Math.floor(length / 2)
  let isEven = length % 2 === 0
  if(isEven){
    result = s.slice(halfIndex-1,halfIndex+1)
  }
  else{
    result = s.charAt(halfIndex)
  }
  
  return result
}
```