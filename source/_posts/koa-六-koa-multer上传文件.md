---
title: koa(六) koa-multer上传文件
date: 2018-11-22 14:52:50
tags:
- koa
- Node.js
categories: 
- koa
---

koa-multer是一个node.js中间件，用于处理` multipart/form-data`类型的表单数据，主要用于上传文件。

[koa-multer](https://www.npmjs.com/package/koa-multer)是基于[multer](https://github.com/expressjs/multer)这个模块。

## 安装

`npm install --save koa-multer`

<!-- more -->

## 配置

``` js
const multer = require('koa-multer');

let storage = multer.diskStorage({
    destination: function (req, file, cb) {
        cb(null, 'public/upload'); //配置图片上传的目录
    },
    filename: function (req, file, cb) {   /*图片上传完成重命名*/
        var fileFormat = (file.originalname).split(".");
        cb(null, Date.now() + "." + fileFormat[fileFormat.length - 1]);
    }
});

let upload = multer({ storage: storage });
```

## 使用

``` js
router.post('/upload', upload.single('face'), async (ctx, next) => {
    ctx.body = {
    filename: ctx.req.file.filename,//返回文件名
    body:ctx.req.body
    }
})
```