## div边框重叠

设置div的margin值为边框值的相反数：

    .div {width:100px; height:103px; border:1px solid #333; margin-right:-1px; margin-bottom:-1px;}


---

## 背景图中间扣出毛玻璃区域（内部背景和外部背景互相契合）

参考链接：http://www.cnblogs.com/ghost-xyx/p/5677168.html
[demo](https://darylxyx.github.io/Demo/blur/)

坑： 该方案是靠 `background-attachment:fixed;` 来实现，所以不适合有滚动条的页面，如果有滚动条，还是老老实实用background-position来定位

#### html
```HTML
<div class='content'></div>
```

#### css     
```CSS
*{margin:0;padding:0}
.content{
  width:540px;
  height:303px;
  background:url('http://n1image.hjfile.cn/mh/2017/01/12/bcfb0406e08ba59e779316c20c373cf6.jpg');
  position:relative;
}
.content:after{
  background:url('http://n1image.hjfile.cn/mh/2017/01/12/bcfb0406e08ba59e779316c20c373cf6.jpg');
  content:'';
  width:250px;
  height:150px;
  position:absolute;
  left:50%;
  TOP:50%;
  margin-top:-75px;
  margin-left:-125px;
  background-attachment:fixed;
  -webkit-filter: blur(20px);
  overflow:hidden;
}
```
---

## 自定义滚动条

来源 http://www.xuanfengge.com/css3-webkit-scrollbar.html

- ::-webkit-scrollbar 滚动条整体部分
- ::-webkit-scrollbar-thumb  滚动条里面的小方块，能向上向下移动（或往左往右移动，取决于是垂直滚动条还是水平滚动条）
- ::-webkit-scrollbar-track  滚动条的轨道（里面装有Thumb）
- ::-webkit-scrollbar-button 滚动条的轨道的两端按钮，允许通过点击微调小方块的位置。
- ::-webkit-scrollbar-track-piece 内层轨道，滚动条中间部分（除去）
- ::-webkit-scrollbar-corner 边角，即两个滚动条的交汇处
- ::-webkit-resizer 两个滚动条的交汇处上用于通过拖动调整元素大小的小控件

```CSS
/*定义滚动条高宽及背景 高宽分别对应横竖滚动条的尺寸*/
::-webkit-scrollbar
{
    width: 16px;
    height: 16px;
    background-color: #F5F5F5;
}

/*定义滚动条轨道 内阴影+圆角*/
::-webkit-scrollbar-track
{
    -webkit-box-shadow: inset 0 0 6px rgba(0,0,0,0.3);
    border-radius: 10px;
    background-color: #F5F5F5;
}

/*定义滑块 内阴影+圆角*/
::-webkit-scrollbar-thumb
{
    border-radius: 10px;
    -webkit-box-shadow: inset 0 0 6px rgba(0,0,0,.3);
    background-color: #555;
}
```

## calc 计算属性

语法：```width: calc(100% - 32px);``` 计算符左右需要空格

作用： 适合不同单位之间的计算

坑： caniuse 上写 UC浏览器不兼容，但是小米下的uc 测下来ok 


## :last-of-type

语法：```p:last-of-type``` 获取最后一个p元素