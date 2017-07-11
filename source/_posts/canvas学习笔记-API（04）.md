---
title: canvas学习笔记 - API（04）
date: 2017-07-11 14:27:35
tags:
  - canvas
---

## 变形

- save()restore()
save 和 restore 方法是用来保存和恢复 canvas 状态的，都没有参数。Canvas 的状态就是当前画面应用的所有样式和变形的一个快照。

Canvas状态存储在栈中，每当save()方法被调用后，当前的状态就被推送到栈中保存。一个绘画状态包括：
- 当前应用的变形（即移动，旋转和缩放，见下）
- strokeStyle, fillStyle, globalAlpha, lineWidth, lineCap, lineJoin, miterLimit, shadowOffsetX, shadowOffsetY, shadowBlur, shadowColor, globalCompositeOperation 的值
- 当前的裁切路径（clipping path）

你可以调用任意多次 save 方法。
每一次调用 restore 方法，上一个保存的状态就从栈中弹出，所有设定都恢复。

#### 移动Translating

translate(x, y)
translate 方法接受两个参数。x 是左右偏移量，y 是上下偏移量，**它用来移动 canvas 和它的原点到一个不同的位置**。
![图片](https://mdn.mozillademos.org/files/234/Canvas_grid_translate.png "title" )

#### 旋转 Rotating

rotate(angle)
这个方法只接受一个参数：旋转的角度(angle)，它是顺时针方向的，以弧度为单位的值。
旋转的中心点始终是 canvas 的原点，如果要改变它，我们需要用到 translate 方法。
![图片](https://mdn.mozillademos.org/files/233/Canvas_grid_rotate.png "title" )

#### 缩放 Scaling

scale(x, y)
scale 方法接受两个参数。x,y 分别是横轴和纵轴的缩放因子，它们都必须是正值。值比 1.0 小表示缩小，比 1.0 大则表示放大，值为 1.0 时什么效果都没有。
