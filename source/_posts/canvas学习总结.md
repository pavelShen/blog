---
title: canvas学习总结
date: 2017-07-17 14:12:29
tags:
  - canvas
---

## 常用API

`ctx = canvas.getContext(‘2d’)` 获取上下文
`ctx.canvas.height ctx.canvas.width` 获取画布宽高
`beginPath()` 清空子路径列表开始一个新路径的方法
`closePath()` 将笔点返回到当前子路径起始点的方法。它尝试从当前点到起始点绘制一条直线。 如果图形已经是封闭的或者只有一个点，那么此方法不会做任何操作
`globalCompositeOperation = type`   我们不仅可以在已有图形后面再画新图形，还可以用来遮盖指定区域，清除画布中的某些部分（清除区域不仅限于矩形，像clearRect()方法做的那样 ）以及更多其他操作。

#### 状态快照
```javascript
save()
restore()
```
save 和 restore 方法是用来保存和恢复 canvas 状态的，都没有参数。Canvas 的状态就是当前画面应用的所有样式和变形的一个快照。
Canvas状态存储在栈中，每当save()方法被调用后，当前的状态就被推送到栈中保存。一个绘画状态包括：
- 当前应用的变形（即移动，旋转和缩放，见下）
- strokeStyle, fillStyle, globalAlpha, lineWidth, lineCap, lineJoin, miterLimit, shadowOffsetX, shadowOffsetY, shadowBlur, shadowColor, globalCompositeOperation 的值
- 当前的裁切路径（clipping path）

你可以调用任意多次 save 方法。
每一次调用 restore 方法，上一个保存的状态就从栈中弹出，所有设定都恢复。

#### tip:
- 调用beginPath()之后（或者canvas刚建的时候）第一条路径构造命令通常被视为是moveTo，所以这里最好使用moveTo命令显示声明起始点
- 当你调用fill()函数时，所有没有闭合的形状都会自动闭合，所以你不需要调用closePath()函数。但是调用stroke()时不会自动闭合。

---

## 绘画步骤

#### 路径绘制
```javascript
rect(x, y, width, height)
arc(x, y, radius, startAngle, endAngle, anticlockwise)
lineTo(x,y)

var rectangle = new Path2D();  // 将某一个路径作为变量——创建一个它的副本
rectangle.rect(10, 10, 50, 50);
ctx.stroke(rectangle);
```

#### 设置样式
```javascript
// color  DOMString 字符串，可以转换成 CSS <color> 值。
// gradient  CanvasGradient 对象（线性渐变或放射性渐变）。
// pattern   CanvasPattern 对象（可重复的图片）。
ctx.fillStyle = color;
ctx.fillStyle = gradient;
ctx.fillStyle = pattern;

//strokeStyle 参考fillStyle
```

最后通过 `ctx.fill()` 或者 `ctx.stroke()`进行绘制

---

## 图片

#### 引入图像到canvas里需要以下两步基本操作：
1. 获得一个指向HTMLImageElement的对象或者另一个canvas元素的引用作为源，也可以通过提供一个URL的方式来使用图片
2. 使用drawImage()函数将图片绘制到画布上

#### drawImage
- 绘制
drawImage(image, x, y) 其中 image 是 image 或者 canvas 对象，x 和 y 是其在目标 canvas 里的起始坐标。
- 缩放
drawImage(image, x, y, width, height) 这个方法多了2个参数：width 和 height，这两个参数用来控制 当像canvas画入时应该缩放的大小
- 切片
drawImage(image, sx, sy, sWidth, sHeight, dx, dy, dWidth, dHeight)
第一个参数和其它的是相同的，都是一个图像或者另一个 canvas 的引用。其它8个参数最好是参照右边的图解，前4个是定义图像源的切片位置和大小，后4个则是定义切片的目标显示位置和大小。

#### 源
- HTMLImageElement 这些图片是由Image()函数构造出来的，或者任何的`<img>`元素
- HTMLVideoElement 用一个HTML的 `<video>`元素作为你的图片源，可以从视频中抓取当前帧作为一个图像
- HTMLCanvasElement 可以使用另一个 `<canvas>` 元素作为你的图片源。
- ImageBitmap 这是一个高性能的位图，可以低延迟地绘制，它可以从上述的所有源以及其它几种源中生成。

#### 使用其它域名下的图片
在 HTMLImageElement上使用crossOrigin属性，你可以请求加载其它域名上的图片。如果图片的服务器允许跨域访问这个图片，那么你可以使用这个图片而不污染canvas，否则，使用这个图片将会污染canvas。
示例代码
```javascript
<img src="http://n1image.hjfile.cn/mh/2016/09/02/a0c9ca472e8ae8e26e34a31d98c074ce.jpg" id="a" crossorigin="anonymous"></img>
<canvas id="myCanvas" width="500" height="550" style="border:1px solid #d3d3d3;"></canvas>
<script>
var c=document.getElementById("myCanvas");
var ctx=c.getContext("2d");
var img = document.getElementById('a')
img.onload = function(){
  ctx.drawImage(img,0,0)
}
function copy()
{
var imgData=ctx.getImageData(20,20,150,150);
ctx.putImageData(imgData,150,70);
}
</script>
<button onclick="copy()">复制</button>
```
#### 设置crossOrigin属性
在HTML5中，一些 HTML 元素提供了对 CORS 的支持， 例如 `<img>` 和 `<video>` 均有一个跨域属性 (crossOrigin property)，它允许你配置元素获取数据的 CORS 请求。
可选值：
- anonymous 对此元素的CORS请求将不设置凭据标志。
- use-credentials 对此元素的CORS请求将设置凭证标志; 这意味着请求将提供凭据。
无效的关键字和空字符串也会被当作 anonymous 关键字使用
示例
`<script src="https://example.com/example-framework.js" crossorigin="anonymous"></script>`

#### 污染canvas
尽管不通过 CORS 就可以在画布中使用图片，但是这会污染画布。一旦画布被污染，你就无法读取其数据。例如，你不能再使用画布的 toBlob(), toDataURL() 或 getImageData() 方法，调用它们会抛出安全错误。
这种机制可以避免未经许可拉取远程网站信息而导致的用户隐私泄露。

#### 使用其它 canvas 元素
和引用页面内的图片类似地，用 document.getElementsByTagName 或 document.getElementById 方法来获取其它 canvas 元素。但你引入的应该是已经准备好的 canvas。
**一个常用的应用就是将第二个canvas作为另一个大的 canvas 的缩略图。**

---

## 像素操作

#### ImageData 对象
**ImageData对象中存储着canvas对象真实的像素数据，它包含以下几个只读属性：**
- width 图片宽度，单位是像素
- height 图片高度，单位是像素
- data
Uint8ClampedArray类型的一维数组，包含着RGBA格式的整型数据，范围在0至255之间（包括255）。
data属性返回一个 Uint8ClampedArray，它可以被使用作为查看初始像素数据。每个像素用4个1bytes值(按照红，绿，蓝和透明值的顺序; 这就是"RGBA"格式) 来代表。每个颜色值部份用0至255来代表。每个部份被分配到一个在数组内连续的索引，左上角像素的红色部份在数组的索引0位置。像素从左到右被处理，然后往下，遍历整个数组。

#### 得到场景像素数据
`var myImageData = ctx.getImageData(left, top, width, height);`
注：任何在画布以外的元素都会被返回成一个透明黑的ImageData对像。

#### demo取色器
```javascript
var img = new Image();
img.src = 'https://mdn.mozillademos.org/files/5397/rhino.jpg';
var canvas = document.getElementById('canvas');
var ctx = canvas.getContext('2d');
img.onload = function() {
  ctx.drawImage(img, 0, 0);
  img.style.display = 'none';
};
var color = document.getElementById('color');
function pick(event) {
  var x = event.layerX;
  var y = event.layerY;
  var pixel = ctx.getImageData(x, y, 1, 1);
  var data = pixel.data;
  var rgba = 'rgba(' + data[0] + ',' + data[1] +
             ',' + data[2] + ',' + (data[3] / 255) + ')';
  color.style.background = rgba;
  color.textContent = rgba;
}
canvas.addEventListener('mousemove', pick);
```

#### 在场景中写入像素数据
`ctx.putImageData(myImageData, dx, dy);` dx和dy参数表示你希望在场景内左上角绘制的像素数据所得到的设备坐标。

#### 保存图片
- canvas.toDataURL('image/png') 默认设定。
- canvas.toDataURL('image/jpeg', quality)
创建一个JPG图片。你可以有选择地提供从0到1的品质量，1表示最好品质，0基本不被辨析但有比较小的文件大小。

---

## 其他

#### 线条
宽度为奇数的线并不能精确呈现，这就是因为路径的定位问题
![图片](https://mdn.mozillademos.org/files/201/canvas-grid.png "title" )

#### clip裁切
```javascript
ctx.beginPath();
ctx.arc(0,0,60,0,Math.PI*2,true);
ctx.clip();
```
裁切路径不会在 canvas 上绘制东西，而且它永远不受新图形的影响。这些特性使得它在特定区域里绘制图形时相当好用。( 可以把它理解成PS里面的选择工具 )
**裁切区域需要通过save&restore方法去还原，因为是路径方法，所以要记得beginPath**

## 参考文档
MDN:https://developer.mozilla.org/zh-CN/docs/Web/API/Canvas_API/Tutorial
