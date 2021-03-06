---
title: 学习koa 1.0
date: 2016-08-09 08:36:08
tags:
  - koa
  - nodejs
---

## 参考资料
  koa基础：http://javascript.ruanyifeng.com/nodejs/koa.html
  log4js: http://qianduan.guru/2016/08/21/nodejs-lesson-1-log4js/
  单元测试：http://taobaofed.org/blog/2015/12/29/nodejs-unit-tests-workflow/

具体实际代码参考github库 https://github.com/pavelShen/koa_project


## koa基础

### 中间件级联

    app.use(function* () {
      this.body = "header\n";
      yield saveResults.call(this);
      this.body += "footer\n";}
    );

    function* saveResults() {
      this.body += "Results Saved!\n";
    }

### 中间件传参

    function logger(format) {
      return function* (next){
        var str = format
          .replace(':method', this.method)
          .replace(':url', this.url);

        console.log(str);

        yield next;
      }}

    app.use(logger(':method :url'));

### 多个中间件的合并 完整事例

    var koa = require('koa');
    var app = koa();

    function* random(next) {
        console.log('r');
        yield next;
        console.log('over');
    };

    function* backwards(next) {
        console.log('b');
        yield next;
    }

    function* pi(next) {
        console.log('pi');
        yield next;
    }

    function test(str){
        return function* (next){
            console.log(str);
            yield next;
        }
    }

    function* all(next) {
      yield random.call(this, backwards.call(this, pi.call(this, next)));
    }

    function* all2(next) {
      yield random.call(this, backwards.call(this, pi.call(this, test('aa').call(this,next))));
    }

    app.use(all2);
    app.listen(3000);

### 错误监听

    app.on('error', function(err){
      log.error('server error', err);}
    );

---

<!--more-->

## koa-router

### koa路由

以下为一套路由
this.params 为路由中带的参数
this.status 为页面返回给你的状态码

    var app = require('koa')();
    var Router = require('koa-router');

    var myRouter = new Router();

    myRouter.get('/', function* (next) {
      console.log(myRouter.url('user',{id:99}))
    });

    myRouter.get('user','/users/:id', function* (next) {
      this.response.body = this.params.id;
    });

    myRouter.get('detail','/detail/:id', function* (next) {
      this.response.body = this.params.id;
    });

    myRouter.get('/tes/:user', function* (next) {
        this.body = this.user;
    }).param('user', function* (id, next) {
        var users = [ '0号用户', '1号用户', '2号用户'];
        this.user = users[id];
        if (!this.user) return this.status = 404;
        yield next;
    })

    app.use(myRouter.routes());
    app.listen(3000);

---

## 日志

### log4js

    var log4js = require('log4js');

    	log4js.configure({
    	  appenders: [
    	    { type: 'console' },
    	    { type: 'file', filename: 'log/app.log', category: 'siteName' },
          {
            type: 'logLevelFilter',
            level: 'DEBUG',
            category: 'category1',
            appender: {
              type: 'file',
              filename: 'default.log'
            }
          }
    	  ]
    	});

    var logger = log4js.getLogger("siteName");
    var logger2 = log4js.getLogger("category1");

    module.exports = logger;

### 常用属性介绍

**Appender** 日志的出口

- Console 控制台输出
- File 文件输出
- [DateFile](https://github.com/nomiddlename/log4js-node/wiki/Date%20rolling%20file%20appender) 日志输出到文件，日志文件可以安特定的日期模式滚动
- [DateFileSync](https://github.com/nomiddlename/log4js-node/wiki/Date%20rolling%20file%20appender%20-%20with%20synchronous%20file%20output%20modes) 同步模式
- [SMTP](https://github.com/nomiddlename/log4js-node/wiki/SMTP) 输出日志到邮件
- [Mailgun](https://github.com/nomiddlename/log4js-node/wiki/MailGun-Appender) 输出日志到邮件 通过（mailgun API）
- [hook.io](https://github.com/nomiddlename/log4js-node/wiki/hook.io)
- [GELF](https://github.com/nomiddlename/log4js-node/wiki/GELF)
- [Multiprocess](https://github.com/nomiddlename/log4js-node/wiki/Multiprocess)
- [Loggly](https://github.com/nomiddlename/log4js-node/wiki/Loggly)
- [Clustered](https://github.com/nomiddlename/log4js-node/wiki/Clustered)


使用 logLevelFilter 和 level 来对日志的级别进行过滤，所有权重大于或者等于DEBUG的日志将会输出。这也是之前提到的日志级别权重的意义；
通过 category 来选择要输出日志的类别，category2 下面的日志被过滤掉了，该配置也接受一个数组，例如 ['category1', 'category2']，这样配置两个类别的日志都将输出到文件中。

**layout** 日志内容

    // file: layout-pattern.js
    var log4js = require('log4js');
    log4js.configure({
      appenders: [{
        type: 'console',
        layout: {
          type: 'pattern',
          pattern: '[%r] [%[%5.5p%]] - %m%n'
        }
      }]
    })
    var logger = log4js.getLogger('layout-pattern');
    logger.debug("Time:", new Date());

pattern 字符含义
- %r - 本地时间
- %p - 日志等级
- %c - log category
- %h - hostname
- %m - 日志信息
- %d - 日期格式化
```
%d{ISO8601}
%d{ISO8601_WITH_TZ_OFFSET}
%d{ABSOLUTE}
%d{DATE}
```
- %% - %
- %n - 换行
- %x{<tokenname>} - token值
- %[ and %] - 定义颜色



![示意图](http://7xrvqo.com1.z0.glb.clouddn.com/images/log4js/LOGGER_APPENDER_LAYOUT.365eb730.png)

---

## 单元测试

###  mocha + should + supertest

#### package.js

    "scripts": {
      "test": "./node_modules/.bin/mocha test/*.test.js --timeout 20000",
      "cov": "./node_modules/.bin/istanbul cover ./node_modules/.bin/_mocha -- -u exports test/*.test.js --timeout 20000"
    }

#### a.test.js

    'use strict';

    const should = require('should');
    const supertest = require('supertest');
    var restaurantData = require('../controller/restaurant.js');

    describe('restaurantData type', function() {
      it('restaurantData should return an Function', function(done) {
        restaurantData.should.be.a.Function();
        done();
      });
    });

    describe('restaurantData isOK', function() {
      it('restaurantData should getData', function(done) {
        supertest('https://m.ele.me/restapi/shopping/restaurants?latitude=31.20745&longitude=121.59842&offset=40&limit=1').get('/').expect(200,done);
      });
    });
