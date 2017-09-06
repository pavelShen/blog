---
title: codewar-05
date: 2017-09-07 00:53:14
tags:
  - codewar
---

# codewar刷题之旅开始啦~

## Duplicate Encoder (根据字母是否出现过进行转码

#### 直接判断indexOf和lastIndexOf
```javascript
function duplicateEncode(word){
  let result =''
  word = word.toLowerCase()
  for(let item of word){
    if(word.indexOf(item)===word.lastIndexOf(item)){
      result += '('
    }
    else{
      result += ')'
    }
  }
  
  return result
}
```

#### 正则（特殊字符有bug）
无法再regexp对象的参数中传入转义字符

```javascript
function duplicateEncode(word){
  let result =''
  
  for(let item of word){
    let reg = new RegExp(item,'ig')
    if(word.match(reg).length===1){
      result += '('
    }
    else{
      result += ')'
    }
  }
  
  return result
}

duplicateEncode("(( @")
```

---