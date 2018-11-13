---
title: koa(二) koa-static
date: 2018-11-13 10:17:10
tags:
- koa
- Node.js
categories: 
- koa
---

koa静态资源中间件。

<!-- more -->
## 安装

``` js
npm install --save koa-static
```

## 使用

``` js
const Koa = require('koa');
const app = new Koa();
const static = require('koa-static');

// app.use(static(root, opts));
// 配置路径可以配置多个
app.use(static(__dirname + './www'))

```