---
title: 小程序填坑之路
date: 2016-09-26 08:53:00
tags:
  - weixin
  - app
---
- scroll-view 需要height 才能触发bindscrolltolower，如果使用100%，则父元素需要有高度（可以使用100%，我偷偷的把page设成100%，感觉会埋坑，不过page的样式只会在当前页面有效，又感觉还行）实在不行，可以使用onReachBottom 替代（常用在既有下拉刷新又有上拉加载的页面，不过下拉刷新的ui感觉很挫，能不用还是不用吧）
- app.json pages数组第一项代表小程序初始页,可以修改顺序方便调试页面
- 不要在小程序中随便用es6浪([小程序ES6兼容文档](https://mp.weixin.qq.com/debug/wxadoc/dev/devtools/details.html))，调试工具ok 但是手机上就是报错，这个问题找了好久 [捂脸笑]
- 预览二维码牛逼的缓存，可以通过开关代码压缩上传选项重新生成二维码
- 和wx:if相对应的hidden 逻辑与wx:if相反的  hidden="true" 才是隐藏组件
- 在一个页面中有多个音频的情况下
  如果直接在audio标签中通过模板变量修改src，使用play()方法会提示播放被中断，需要双击才能正确播放，
  但是可以通过
  ```javascript
  this.audioCtx.setSrc(src);
  this.audioCtx.play();   
  ```
  的方式修改音频的播放地址,则一切ok

- 要解决wx.navigateBack后的信息同步问题的方案：在global放置一个全局变量，通过onShow监听页面显示，当变量有值时进行数据同步并把这个全局变量设置为空。

- 提交时获取formid，用于发送模板消息

```HTML
<form report-submit="true" bindsubmit="sendTempMessage">
      <button formType="submit">提交</button>
</form>
```
```JACASCRIPT
sendTempMessage(e){
  console.log(e.detail.formId)
}
```

- 在更新for列表的时候，新增的数据必须带有wx:key所指定的值，否则虽然数据变了，但是列表渲染并不会改变（常见于插入临时数据）
- 微信开发者工具 textarea 超过140个汉字时 无法触发事件，手机实测ok
- 小程序中想要修改对象的某个值，只能在修改了data之后，将整个对象重新赋值给data（setData就是个坑，希望以后能直接同步值）

## 常见钩子函数：
```HTML
- onLoad       监听页面加载
- onReady     监听页面初次渲染完成
- onShow      监听页面显示
- onHide        监听页面隐藏
- onUnload    监听页面卸载
```

相关文章：
- [微信小程序踩坑记](http://www.acfunc.com/)
- [官方教程](https://mp.weixin.qq.com/debug/wxadoc/dev/?t=1474644087418)
