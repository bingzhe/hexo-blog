---
title: koa(一) koa-bodyparser
date: 2018-11-12 18:39:50
tags:
- koa
- Node.js
categories: 
- koa
---

koa中解析body的中间件，支持`json`,`form`,`text`,可以用来获取post提交的数据

## 原生Node.js获取post提交的数据

``` js
function parsePostData(ctx) {
    return new Promise((resolve, reject) => {
        try {
            let postdata = "";
            ctx.req.on('data', data => {
                postdata += data;
            });
            ctx.req.on('end', () => {
                resolve(postdata);
            });
        } catch (error) {
            reject(error);
        }
    });
}

```
<!-- more -->
## 安装

``` js
npm install --save koa-bodyparser
```

## 使用

``` js
const Koa = require('koa');
const bodyParse = require('koa-bodyparser');

const app = new Koa();

app.use(bodyParse());

app.use(async ctx => {
  // the parsed body will store in ctx.request.body
  // if nothing was parsed, body will be an empty object {}
  ctx.body = ctx.request.body;
});
```

## 链接

[文档](https://www.npmjs.com/package/koa-bodyparser)