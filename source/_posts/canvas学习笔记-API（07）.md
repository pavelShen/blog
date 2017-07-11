---
title: canvas学习笔记 - API（07）
date: 2017-07-11 14:27:50
tags:
  - canvas
---

## 像素操作

#### ImageData 对象
**ImageData对象中存储着canvas对象真实的像素数据，它包含以下几个只读属性：**

- width 图片宽度，单位是像素
- height 图片高度，单位是像素
- data
Uint8ClampedArray类型的一维数组，包含着RGBA格式的整型数据，范围在0至255之间（包括255）。
data属性返回一个 Uint8ClampedArray，它可以被使用作为查看初始像素数据。每个像素用4个1bytes值(按照红，绿，蓝和透明值的顺序; 这就是"RGBA"格式) 来代表。每个颜色值部份用0至255来代表。每个部份被分配到一个在数组内连续的索引，左上角像素的红色部份在数组的索引0位置。像素从左到右被处理，然后往下，遍历整个数组。

#### 创建一个ImageData对象

去创建一个新的，空白的ImageData对象，你应该会使用createImageData() 方法。有2个版本的createImageData()方法。
1. `var myImageData = ctx.createImageData(width, height);`
上面代码创建了一个新的具体特定尺寸的ImageData对象。所有像素被预设为透明黑。

2. `var myImageData = ctx.createImageData(anotherImageData);`
你也可以创建一个被anotherImageData对象指定的相同像素的ImageData对象。这个新的对象像素全部被预设为透明黑。这个并非复制了图片数据。


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

#### 反锯齿
反锯齿默认开启，手动关闭可以看清放大后的图片的具体像素
```javascript
zoomctx.imageSmoothingEnabled = this.checked;
zoomctx.mozImageSmoothingEnabled = this.checked;
zoomctx.webkitImageSmoothingEnabled = this.checked;
zoomctx.msImageSmoothingEnabled = this.checked;
```

#### 保存图片
- canvas.toDataURL('image/png')  默认设定。创建一个PNG图片。
- canvas.toDataURL('image/jpeg', quality)
创建一个JPG图片。你可以有选择地提供从0到1的品质量，1表示最好品质，0基本不被辨析但有比较小的文件大小。
