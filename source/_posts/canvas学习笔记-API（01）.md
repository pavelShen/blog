---
title: canvas学习笔记 - API（01）
date: 2017-07-11 14:25:13
tags:
  - canvas
---

#### 基础
- ctx = canvas.getContext('2d') 获取上下文
- `ctx.canvas.height` `ctx.canvas.width`获取画布宽高

#### 矩形
- fillStyle = "rgb(200,0,0)"; 设置填充色
- fillRect(x, y, width, height) 绘制一个填充的矩形
- strokeRect(x, y, width, height) 绘制一个矩形的边框
- clearRect(x, y, width, height) 清除指定矩形区域，让清除部分完全透明。
- rect(x, y, width, height)
绘制一个左上角坐标为（x,y），宽高为width以及height的矩形**路径**。
当该方法执行的时候，moveTo()方法自动设置坐标参数（0,0）。也就是说，当前笔触自动重置会默认坐标。 

#### 路径
- beginPath()   新建一条路径，生成之后，图形绘制命令被指向到路径上生成路径。
- closePath()   闭合路径之后图形绘制命令又重新指向到上下文中。
- stroke()   通过线条来绘制图形轮廓。
- fill() 通过填充路径的内容区域生成实心的图形。


#### 移动笔触
- moveTo(x, y) 将笔触移动到指定的坐标x以及y上。


#### 线
- lineTo(x, y)  绘制一条从当前位置到指定x以及y位置的直线。


#### 圆弧
当然可以使用arcTo()，不过这个的实现并不是那么的可靠，所以我们这里不作介绍

- arc(x, y, radius, startAngle, endAngle, anticlockwise) 
这里详细介绍一下arc方法，该方法有六个参数：x,y为绘制圆弧所在圆上的圆心坐标。radius为半径。startAngle以及endAngle参数用弧度定义了开始以及结束的弧度。这些都是以x轴为基准。参数anticlockwise 为一个布尔值。为true时，是逆时针方向，否则顺时针方向。


#### Path2D对象
```javascript
new Path2D(); // 空的Path对象
new Path2D(path); // 克隆Path对象
```

Path2D()会返回一个新初始化的Path2D对象（可能将某一个路径作为变量——创建一个它的副本，或者将一个包含SVG path数据的字符串作为变量）。

- Path2D.addPath(path [, transform])  **浏览器支持较差** 添加了一条路径到当前路径（可能添加了一个变换矩阵）。

代码示例
```javascript
function draw() {
  var canvas = document.getElementById('canvas');
  if (canvas.getContext){
    var ctx = canvas.getContext('2d');
    var rectangle = new Path2D();
    rectangle.rect(10, 10, 50, 50);
    var circle = new Path2D();
    circle.moveTo(125, 35);
    circle.arc(100, 35, 25, 0, 2 * Math.PI);
    ctx.stroke(rectangle);
    ctx.fill(circle);
  }
}
```