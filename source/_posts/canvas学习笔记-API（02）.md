---
title: canvas学习笔记 - API（02）
date: 2017-07-11 14:27:21
tags:
  - canvas
---

## 色彩

- fillStyle = color               设置图形的填充颜色。
- strokeStyle = color         设置图形轮廓的颜色。
- globalAlpha = transparencyValue  这个属性影响到 canvas 里所有图形的透明度，有效的值范围是 0.0 （完全透明）到 1.0（完全不透明），默认是 1.0。

## 线型 Line styles

- lineWidth = value 设置线条宽度。
- lineCap = type     设置线条末端样式。
- lineJoin = type     设定线条与线条间接合处的样式。
- miterLimit = value  限制当两条线相交时交接处最大长度；所谓交接处长度（斜接长度）是指线条交接处内角顶点到外角顶点的长度。
- getLineDash()       返回一个包含当前虚线样式，长度为非负偶数的数组。
- setLineDash(segments)   设置当前虚线样式。
- lineDashOffset = value      设置虚线样式的起始偏移量。

线条的一些细节
![图片](https://mdn.mozillademos.org/files/201/canvas-grid.png "title" )

## 渐变

我们用下面的方法新建一个 canvasGradient 对象，并且赋给图形的 fillStyle 或 strokeStyle 属性。

- createLinearGradient(x1, y1, x2, y2)             createLinearGradient 方法接受 4 个参数，表示渐变的起点 (x1,y1) 与终点 (x2,y2)。
- createRadialGradient(x1, y1, r1, x2, y2, r2)   createRadialGradient 方法接受 6 个参数，前三个定义一个以 (x1,y1) 为原点，半径为 r1 的圆，后三个参数则定义另一个以 (x2,y2) 为原点，半径为 r2 的圆。
- gradient.addColorStop(position, color)         addColorStop 方法接受 2 个参数，position 参数必须是一个 0.0 与 1.0 之间的数值，表示渐变中颜色所在的相对位置。例如，0.5 表示颜色会出现在正中间。color 参数必须是一个有效的 CSS 颜色值（如 #FFF， rgba(0,0,0,1)，等等）


## 图案
- createPattern(image, type)  该方法接受两个参数。Image 可以是一个 Image 对象的引用，或者另一个 canvas 对象。Type 必须是下面的字符串值之一：repeat，repeat-x，repeat-y 和 no-repeat。
```javascript
function draw() {
  var ctx = document.getElementById('canvas').getContext('2d');
  // 创建新 image 对象，用作图案
  var img = new Image();
  img.src = 'images/wallpaper.png';
  img.onload = function(){
    // 创建图案
    var ptrn = ctx.createPattern(img,'repeat');
    ctx.fillStyle = ptrn;
    ctx.fillRect(0,0,150,150);
  }
}
```

## 文字
- fillText(text, x, y [, maxWidth])       **实心**文字，参数为文字内容，x坐标，y坐标，最大宽度
- strokeText(text, x, y [, maxWidth]) **空心**文字，参数为文字内容，x坐标，y坐标，最大宽度
- font = value  当前文字的样式，和css文字样式的属性相同，默认值为10px sans-serif。
- textAlign = value  文字对齐方式，可选值: start, end, left, right or center. 默认值 start。
- textBaseline = value 文字基线设置 ，可选值: top, hanging, middle, alphabetic, ideographic, bottom。默认值 alphabetic。
- direction = value  方向，可选值: ltr, rtl, inherit. 默认值 inherit。
- measureText() 返回一个TextMetrics对象，包含将被画在画布上的指定文字的像素级宽度
```javascript
function draw() {
  var ctx = document.getElementById('canvas').getContext('2d');
  var text = ctx.measureText('foo'); // TextMetrics object
  text.width; // 16;
}
```

## 阴影
- shadowOffsetX = float
shadowOffsetX 和 shadowOffsetY 用来设定阴影在 X 和 Y 轴的延伸距离，它们是不受变换矩阵所影响的。负值表示阴影会往上或左延伸，正值则表示会往下或右延伸，它们默认都为 0。
- shadowOffsetY = float
shadowOffsetX 和 shadowOffsetY 用来设定阴影在 X 和 Y 轴的延伸距离，它们是不受变换矩阵所影响的。负值表示阴影会往上或左延伸，正值则表示会往下或右延伸，它们默认都为 0。
- shadowBlur = float
shadowBlur 用于设定阴影的模糊程度，其数值并不跟像素数量挂钩，也不受变换矩阵的影响，默认为 0。
- shadowColor = color
shadowColor 是标准的 CSS 颜色值，用于设定阴影颜色效果，默认是全透明的黑色。

## Canvas 填充规则
设置fill的填充规则，以下为2个可选值
- nonzero (默认)
这个值确定了某点属于该形状的“内部”还是“外部”。从点向任意方向的无限远处绘制射线，然后检测形状与射线相交的位置。开始于0数，射线上每次从左向右相交就加1，每次从右向左相交就减1。数一下相交次数，如果结果是0，点就在路径外面，如果结果大于0，点在路径里面。
- evenodd
这个值用确定了某点属于该形状的“内部”还是“外部”。从点向任意方向的无限远处绘制射线，并数一数给定形状与射线相交的路径段的数目，如果数目是奇数的，点在内部，如果数目是偶数的，点在外部。

代码示例
```javascript
function draw() {
  var ctx = document.getElementById('canvas').getContext('2d'); 
  ctx.beginPath(); 
  ctx.arc(50, 50, 30, 0, Math.PI*2, true);
  ctx.arc(50, 50, 15, 0, Math.PI*2, true);
  ctx.fill("evenodd");
}
```
