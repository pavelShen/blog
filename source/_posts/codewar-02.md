---
title: codewar-02
date: 2017-09-04 22:50:14
tags:
  - codewar
---

# codewar刷题之旅开始啦~

## Array.diff (在数组a中去除数组b的元素)

```javascript
function array_diff(a, b) {
  return a.filter((item) => b.indexOf(item) === -1 ) 
}
```

---

## List Filtering (过滤数组，删除数组中的字符串)

和上题类似，一个filter解决

```javascript
function filter_list(l) {
  let toString = Object.prototype.toString
  return l.filter((item) => toString.call(item) !=='[object String]')
}
```

基本类型，用typeof判断就可以了！不必使用通用方法

```javascript
function filter_list(l) {
  return l.filter( function(elem){return typeof elem != "string"} )
}
```

---

## Vowel Count(找出字符串中元音字母的个数)

#### 无脑的循环遍历，看了答案，忘记考虑大写的元音字母了
```javascript
function getCount(str) {
  let vowelsCount = 0;
  let vowelCollection = ['a','e','i','o','u']
  
  for(let item of str){
    if(vowelCollection.indexOf(item)!==-1){
      vowelsCount ++
    }
  }
  
  return vowelsCount;
}
```

#### match方案,不错的思路
```javascript
function getCount(str) {
  return (str.match(/[aeiou]/ig)||[]).length;
}
```