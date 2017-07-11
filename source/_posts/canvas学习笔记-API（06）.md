---
title: canvas学习笔记 - API（06）
date: 2017-07-11 14:27:46
tags:
  - canvas
---

## 动画

### 动画的基本

- 清空 canvas
除非接下来要画的内容会完全充满 canvas （例如背景图），否则你需要清空所有。最简单的做法就是用 clearRect 方法。
- 保存 canvas 状态
如果你要改变一些会改变 canvas 状态的设置（样式，变形之类的），又要在每画一帧之时都是原始状态的话，你需要先保存一下。
- 绘制动画图形（animated shapes）
这一步才是重绘动画帧。
- 恢复 canvas 状态
如果已经保存了 canvas 的状态，可以先恢复它，然后重绘下一帧。

### 有安排的更新画布 Scheduled updates
可以用window.setInterval(), window.setTimeout(),和window.requestAnimationFrame()来设定定期执行一个指定函数。

### 长尾效果  
直到现在，我们已经使用clearRect 函数当我们清除前一帧动画。当你用一个半透明的fillRect函数取代这个函数的时候，你可以轻松获得长尾效果。
```javascript
ctx.fillStyle = 'rgba(255,255,255,0.3)';
ctx.fillRect(0,0,canvas.width,canvas.height);
```

### demo
```javascript
var canvas = document.getElementById('canvas');
var ctx = canvas.getContext('2d');
var raf;
var ball = {
  x: 100,
  y: 100,
  vx: 5,
  vy: 2,
  radius: 25,
  color: 'blue',
  draw: function() {
    ctx.beginPath();
    ctx.arc(this.x, this.y, this.radius, 0, Math.PI*2, true);
    ctx.closePath();
    ctx.fillStyle = this.color;
    ctx.fill();
  }
};
function draw() {
  // 长尾效果
  //ctx.clearRect(0,0, canvas.width, canvas.height);
  ctx.fillStyle = 'rgba(255,255,255,0.3)';
  ctx.fillRect(0,0,canvas.width,canvas.height);
  // 加速减速
  // ball.vy *= .99;
  // ball.vy += .25;

  //边界
  if (ball.y + ball.vy > canvas.height || ball.y + ball.vy < 0) {
    ball.vy = -ball.vy;
  }
  if (ball.x + ball.vx > canvas.width || ball.x + ball.vx < 0) {
    ball.vx = -ball.vx;
  }
  ball.x += ball.vx;
  ball.y += ball.vy;
  ball.draw();
  raf = window.requestAnimationFrame(draw);
}
canvas.addEventListener('mouseover', function(e){
  raf = window.requestAnimationFrame(draw);
});
canvas.addEventListener("mouseout",function(e){
  window.cancelAnimationFrame(raf);
});
ball.draw();
```