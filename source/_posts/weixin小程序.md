---
title: 小程序填坑之路
date: 2016-09-26 08:53:00
tags:
  - weixin
  - app
---
- scroll-view 需要height 才能触发bindscrolltolower，如果使用100%，则父元素需要有高度（可以使用100%，我偷偷的把page设成100%，感觉会埋坑，不过page的样式只会在当前页面有效，又感觉还行）实在不行，可以使用onReachBottom 替代
- app.json pages数组第一项代表小程序初始页,可以修改顺序方便调试页面
- 不要在小程序中随便用es6浪([小程序ES6兼容文档](https://mp.weixin.qq.com/debug/wxadoc/dev/devtools/details.html))，调试工具ok 但是手机上就是报错，这个问题找了好久 [捂脸笑]
- 预览二维码牛逼的缓存，可以通过开关代码压缩上传选项重新生成二维码
- 和wx:if相对应的hidden 逻辑与wx:if相反的  hidden="true" 才是隐藏组件
- 在一个页面中有多个音频的情况下 
  如果直接在audio标签中通过模板变量修改src，使用play()方法会提示播放被中断，需要双击才能正确播放，
  但是可以通过
  
      this.audioCtx.setSrc(src);
      this.audioCtx.play();   
  
  的方式修改音频的播放地址,则一切ok

相关文章：
- [微信小程序踩坑记](http://www.acfunc.com/)
- [官方教程](https://mp.weixin.qq.com/debug/wxadoc/dev/?t=1474644087418)
