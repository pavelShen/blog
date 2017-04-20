---
title: 读书笔记：深入浅出ES6（16）模块 Modules
date: 2017-04-20 10:54:55
tags:
  - javascript
  - es6
---

原文线上地址：http://www.infoq.com/cn/es6-in-depth/

## 模块 Modules

#### 知识点

- 在ES6模块中，无论你是否加入“use strict;”语句，默认情况下模块都是在严格模式下运行。
- 你可以导出所有的最外层函数、类以及var、let或const声明的变量。

#### Export & Import
**要导出请直接用 module.exports 或者 export default**
exports 是 module.exports 的一个引用，如果 module.exports 改了，exports 就是失去意义了。

###### Export列表
语法
```javascript
function detectCats(canvas, options) { ... }
class Kittydar { ... }
module.exports = {detectCats, Kittydar};
```

###### 重命名import
语法
```javascript
// suburbia.js
// 这两个模块都会导出以`flip`命名的东西。
// 要同时导入两者，我们至少要将其中一个的名称改掉。
import {flip as flipOmelet} from "eggs.js";
import {flip as flipHouse} from "real-estate.js";
```

###### Default exports
`import _ from "lodash";` 等价于 `import {default as _} from "lodash";`

语法
```javascript
export default {
      field1: value1,
      field2: value2
};
```
关键字export default后可跟随任何值：一个函数、一个类、一个对象字面量，只要你能想到的都可以。

###### 导入所有并重命名
`import * as cows from "cows";`
