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