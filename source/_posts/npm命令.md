---
title: npm命令
date: 2017-02-10 18:29:01
tags:
  - nodejs
---

cnpm 设置：
alias cnpm="npm --registry=https://registry.npm.taobao.org \
--cache=$HOME/.npm/.cache/cnpm \
--disturl=https://npm.taobao.org/dist \
--userconfig=$HOME/.cnpmrc"

npm help 查看帮助
npm ls  显示安装的包

命令配置：   不带值默认设置true
会查看到安装包所包含的所有依赖文件； npm ls --global
只查看顶级安装包； npm ls --global --depth=0

npm bin -g :包的安装路径
npm cache clean:删除安装包缓存
npm dedupe 包去重

npm install 不同版本
npm install aa@latest
npm install aa@0.1.1
npm install aa@">=0.1.0 <0.2.0"
npm install aa --force 强制拉取远程包

npm outdated 包过期检查
