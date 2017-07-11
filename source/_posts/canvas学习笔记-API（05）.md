---
title: canvas学习笔记 - API（05）
date: 2017-07-11 14:27:41
tags:
  - canvas
---

## 组合

- globalCompositeOperation = type
注意：下面所有例子中，蓝色方块是先绘制的，即“已有的 canvas 内容”，红色圆形是后面绘制，即“新图形”。

### type值

#### source-over (default)
这是默认设置，新图形会覆盖在原有内容之上。
![pic](https://mdn.mozillademos.org/files/220/Canvas_composite_srcovr.png)

---

#### destination-over
会在原有内容之下绘制新图形。
![pic](https://mdn.mozillademos.org/files/215/Canvas_composite_destovr.png)

---

#### source-in
新图形会仅仅出现与原有内容重叠的部分。其它区域都变成透明的。
![pic](https://mdn.mozillademos.org/files/218/Canvas_composite_srcin.png)

---

#### destination-in
原有内容中与新图形重叠的部分会被保留，其它区域都变成透明的。
![pic](https://mdn.mozillademos.org/files/213/Canvas_composite_destin.png)

---

#### source-out
结果是只有新图形中与原有内容不重叠的部分会被绘制出来。
![pic](https://mdn.mozillademos.org/files/219/Canvas_composite_srcout.png)

---

#### destination-out
原有内容中与新图形不重叠的部分会被保留。
![pic](https://mdn.mozillademos.org/files/214/Canvas_composite_destout.png)

---

#### source-atop
新图形中与原有内容重叠的部分会被绘制，并覆盖于原有内容之上。
![pic](https://mdn.mozillademos.org/files/217/Canvas_composite_srcatop.png)

---

#### destination-atop
原有内容中与新内容重叠的部分会被保留，并会在原有内容之下绘制新图形
![pic](https://mdn.mozillademos.org/files/212/Canvas_composite_destatop.png)

---

#### lighter
两图形中重叠部分作加色处理。
![pic](https://mdn.mozillademos.org/files/216/Canvas_composite_lighten.png)

---

#### darken
两图形中重叠的部分作减色处理。
![pic](https://mdn.mozillademos.org/files/211/Canvas_composite_darken.png)

---

#### xor
重叠的部分会变成透明。
![pic](https://mdn.mozillademos.org/files/221/Canvas_composite_xor.png)

---

#### copy
只有新图形会被保留，其它都被清除掉。
![pic](https://mdn.mozillademos.org/files/210/Canvas_composite_copy.png)

---


## 裁切

```javascript
ctx.beginPath();
ctx.arc(0,0,60,0,Math.PI*2,true);
ctx.clip();
```
裁切路径不会在 canvas 上绘制东西，而且它永远不受新图形的影响。这些特性使得它在特定区域里绘制图形时相当好用。( 可以把它理解成PS里面的选择工具 )

**裁切区域需要通过save&restore方法去还原，因为是路径方法，所以要记得beginPath**
