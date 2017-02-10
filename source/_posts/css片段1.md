---
title: css片段1
date: 2017-02-10 18:28:05
tags:
  - css
---

## div边框重叠

设置div的margin值为边框值的相反数：

    .div {width:100px; height:103px; border:1px solid #333; margin-right:-1px; margin-bottom:-1px;}


---

## 背景图中间扣出毛玻璃区域（内部背景和外部背景互相契合）

参考链接：http://www.cnblogs.com/ghost-xyx/p/5677168.html
[demo](https://darylxyx.github.io/Demo/blur/)

坑： 该方案是靠 ```background-attachment:fixed;``` 来实现，所以不适合有滚动条的页面，如果有滚动条，还是老老实实用background-position来定位

#### html

    <div class='content'>
    </div>


#### css

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
