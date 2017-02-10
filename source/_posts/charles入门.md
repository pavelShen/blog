---
title: charles入门
date: 2017-02-10 18:25:10
tags:
  - tools
---
参考文章 http://blog.devtang.com/2015/11/14/charles-introduction/

1. 选择菜单中的 “Proxy” -> “Mac OS X Proxy” 来将 Charles 设置成系统代理

#### http抓包 ####

1. 在 Charles 的菜单栏上选择 “Proxy”->”Proxy Settings”，填入代理端口 8888，并且勾上 “Enable transparent HTTP proxying” 就完成了在 Charles 上的设置。
2. 手机 WIFI 设置手动代理，HELP -》 LOCAL ADDRESS 查看本地ip -》 端口填入设置里的端口号

#### https抓包 ####

1. 完成http抓包设置
2. 选择 “Help” -> “SSL Proxying” -> “Install Charles Root Certificate”，然后输入系统的帐号密码，即可在 KeyChain 看到添加好的证书。
3. 在证书菜单信任该证书（可能需要重启电脑，不然我这里就莫名的上不了网了）
4. 在 Charles 的菜单栏上选择 “Proxy”->”SSL Proxying Settings” 勾选enable，添加需要捕获的https的url
4. 选择 “Help” -> “SSL Proxying” -> “Save Charles Root Certificate” 保存为cer格式，传输给手机放入SD卡根目录（试了下不用放根目录也行），在安卓（小米）手机中搜索  从SD卡安装（证书）选择该证书文件，终于安装成功

#### 功能 #### 
1. 模拟慢速网络：”Proxy”->”Throttle Setting”
2. 修改网络请求内容：选中请求，右击选择compose，编辑请求参数，点击execute按钮
3. 修改服务器返回内容  
- Map功能：请求重定向
- Rewrite功能： 对请求，响应进行正则替换，适合长期批量替换
- Breakpoints功能：和Rewrite一样，但是只会修改一次

