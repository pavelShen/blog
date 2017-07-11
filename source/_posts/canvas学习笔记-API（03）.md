---
title: canvas学习笔记 - API（03）
date: 2017-07-11 14:27:31
tags:
  - canvas
---

## 图片

### 引入图像到canvas里需要以下两步基本操作：
1. 获得一个指向HTMLImageElement的对象或者另一个canvas元素的引用作为源，也可以通过提供一个URL的方式来使用图片
2. 使用drawImage()函数将图片绘制到画布上

### drawImage

##### 绘制
drawImage(image, x, y) 其中 image 是 image 或者 canvas 对象，x 和 y 是其在目标 canvas 里的起始坐标。

##### 缩放
drawImage(image, x, y, width, height)  这个方法多了2个参数：width 和 height，这两个参数用来控制 当像canvas画入时应该缩放的大小

##### 切片
drawImage(image, sx, sy, sWidth, sHeight, dx, dy, dWidth, dHeight)
第一个参数和其它的是相同的，都是一个图像或者另一个 canvas 的引用。其它8个参数最好是参照右边的图解，前4个是定义图像源的切片位置和大小，后4个则是定义切片的目标显示位置和大小。

### 源
- HTMLImageElement  这些图片是由Image()函数构造出来的，或者任何的`<img>`元素
- HTMLVideoElement   用一个HTML的 `<video>`元素作为你的图片源，可以从视频中抓取当前帧作为一个图像
- HTMLCanvasElement  可以使用另一个 `<canvas>` 元素作为你的图片源。
- ImageBitmap    这是一个高性能的位图，可以低延迟地绘制，它可以从上述的所有源以及其它几种源中生成。

### 使用相同页面内的图片
- document.images集合
- document.getElementsByTagName()方法
- 如果你知道你想使用的指定图片的ID，你可以用document.getElementById()获得这个图片

### 使用其它域名下的图片
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

##### 设置crossOrigin属性
在HTML5中，一些 HTML 元素提供了对 CORS 的支持， 例如 `<img>` 和 `<video>` 均有一个跨域属性 (crossOrigin property)，它允许你配置元素获取数据的 CORS 请求。

可选值：
- anonymous    对此元素的CORS请求将不设置凭据标志。
- use-credentials    对此元素的CORS请求将设置凭证标志; 这意味着请求将提供凭据。
无效的关键字和空字符串也会被当作 anonymous 关键字使用

示例
`<script src="https://example.com/example-framework.js"  crossorigin="anonymous"></script>`

##### 污染canvas
尽管不通过 CORS 就可以在画布中使用图片，但是这会污染画布。一旦画布被污染，你就无法读取其数据。例如，你不能再使用画布的 toBlob(), toDataURL() 或 getImageData() 方法，调用它们会抛出安全错误。
这种机制可以避免未经许可拉取远程网站信息而导致的用户隐私泄露。

### 使用其它 canvas 元素
和引用页面内的图片类似地，用 document.getElementsByTagName 或 document.getElementById 方法来获取其它 canvas 元素。但你引入的应该是已经准备好的 canvas。
**一个常用的应用就是将第二个canvas作为另一个大的 canvas 的缩略图。**
