---
title: codewar-04
date: 2017-09-06 00:50:14
tags:
  - codewar
---

# codewar刷题之旅开始啦~

## Title Case(标题风格，传入一个title，首字母大写，如果传入例外的数组，则例外的短字符大小写不变，首字母始终大写)
titleCase('a clash of KINGS', 'a an the of') // should return: 'A Clash of Kings'
titleCase('THE WIND IN THE WILLOWS', 'The In') // should return: 'The Wind in the Willows'
titleCase('the quick brown fox') // should return: 'The Quick Brown Fox'

#### 强写，感觉写复杂了。。。。。看看别人的答案吧
```javascript
function titleCase(title, minorWords) {
  let result 
  let temp = title.toLowerCase().replace(/(^|\s)([A-Za-z])/g,(match)=>{
    return match.toUpperCase()
  })
  
  if(minorWords){
    let arrTitle = temp.split(' ')
    let arrMinor = minorWords.split(' ').map((item)=>{
      return item.toLowerCase()
    })
    
    result = arrTitle.map((item,index)=>{
      if(arrMinor.indexOf(item.toLowerCase())!== -1 && index !== 0){
        return item.toLowerCase()
      }
      else {
        return item
      }
    })
    
    result = result.join(' ')
  }
  else{
    result = temp
  }
  
  return result
}
```

#### 一个思路，设置一个首字母大写的方法，切分数组后，根据条件决定是否执行该方法
```javascript
String.prototype.capitalize = function() {
    return this.charAt(0).toUpperCase() + this.slice(1).toLowerCase();
}

function titleCase(title, minorWords) {  
  var titleAr = title.toLowerCase().split(' '),
      minorWordsAr = minorWords ? minorWords.toLowerCase().split(' ') : [];
    
  return titleAr.map(function(e, i){return minorWordsAr.indexOf(e) === -1 || i === 0 ? e.capitalize() : e })
                .join(' ');
}
```

---

## Does my number look big in this?(一个数是否为水仙花数)
水仙花数是指一个 n 位数（n≥3 ），它的每个位上的数字的 n 次幂之和等于它本身（例如：1^3 + 5^3+ 3^3 = 153）。

```javascript
function narcissistic( value ) {
  // Code me
  let stringValue = value.toString()
  let digit = stringValue.length
  let result = 0 
  
  for(let i of stringValue){
    i = parseInt(i)
    result += Math.pow(i,digit)
  }
  
  return result === value ? true : false
}
```

#### 累加可以用数组的reduce
```javascript
function narcissistic( value ) {
  return ('' + value).split('').reduce(function(p, c){
    return p + Math.pow(c, ('' + value).length)
    }, 0) == value;
}
```

---

## Vasya - Clerk (买票找零问题)

#### 想的太复杂，最笨的办法也许就是最好的办法

```javascript
function tickets(peopleInLine){
  let M25 = 0,M50 =0
  for(let item of peopleInLine){
    if(item===25){
      M25 ++
    }
    else if(item===50){
      M25 --
      M50 ++
    }
    else if(item===100){
      if(M50>0){
        M50 --
        M25 --
      }
      else{
        M25 -= 3
      }
    }
    
    if(M25<0 || M50<0){
      return 'NO'
    }
  }
  
  return 'YES'
}
```