---
title: 使用fiddler
date: 2016-08-09 08:36:08
tags:
  - tools
---

## 其他设备通过fiddler访问网络（手机调试抓包使用）

菜单栏点击 Tools > Fiddler Options.

勾选 '''Ensure Allow remote clients to connect'''.

![1-1](/img/use-fiddler/1-1.png)

在手机中设置网络代理

代理模式为手动，IP地址为电脑的IP,端口为上图中port的值

用手机访问网站时，可以通过fiddler查看网络通信

**事后别忘记把代理关闭，不然你关了fiddler就上不了网了**

---

## fiddler插件安装

地址：http://www.telerik.com/fiddler/add-ons

tip: 如果使用迅雷下载，记得选择只从原始地址下载，不然任务总是失败

---

## 定制fiddler规则 ( Write a FiddlerScript Rule )

菜单栏点击 Tools > Fiddler Options > Tools. 修改默认编辑器

菜单栏点击 Rules > Customize Rules.

修改js文件

#### 例子

在 OnBeforeRequest 方法中加入代码:

    if (oSession.host.toLowerCase() == "localhost:8888"){
      oSession.host = "localhost:80";
    }

访问localhost:8888 实际访问的是localhost:80

---

## Session List 操作

- ctrl+f 查找
- 选择session 按p 定为请求的父级 按c 定为请求的子集

---

## AutoResponder

- 在选项卡内勾选Enable rules 开启规则
- rule列表上方的权重较高，可以通过+-来调整rule的权重

#### 导入/导出rules

1. 选择多个rule，右击 选择Export all 导出 
2. Add Rule 右侧的Import按钮 点击选择之前导出的文件进行导入

---

## 重发请求

方法1：选择左侧session,右击 选择replay->reissue requests
方法2：
1. 在右侧composer的选项卡内选择Scratchpad
2. 点击或者拖动左侧session至选项卡内
3. 三次点击选中选项卡内的请求头部信息
4. 点击Execute 发送

---

## 导入/导出网络请求

File->import session / File->export session 

---

