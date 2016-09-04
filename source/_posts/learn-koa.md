---
title: 学习koa 1.0
date: 2016-08-09 08:36:08
tags:
  - koa
  - nodejs
---

参考资料 http://javascript.ruanyifeng.com/nodejs/koa.html
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
    	    { type: 'file', filename: 'log/app.log', category: 'siteName' }
    	  ]
    	});

    var logger = log4js.getLogger("siteName");

    module.exports = logger;

### 常用属性介绍
坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑坑

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
