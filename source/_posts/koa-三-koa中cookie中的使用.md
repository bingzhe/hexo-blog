---
title: koa(三) koa中Cookie中的使用
date: 2018-11-13 10:44:56
tags:
- koa
- Node.js
categories: 
- koa
---

## 用法

* koa中设置Cookie

`ctx.cookies.set(name, value, [options])`

* koa中获取Cookie的值

`ctx.cookies.get(name)`

<!-- more -->

## Options

* domain

cookie所在的域名

* path

cookie所在的路径,默认'/'

* maxAge

有效时长

* expires

失效时间

* secure

安全cookie，默认false，设置为true时只有https可以访问

* httpOnly

是否只是服务器可访问cookie, 默认true

* overwrite

是否允许重写

## 设置中文Cookie

``` js
new Buffer('hello, world!').toString('base64'); //转换成base64

new Buffer('aGVsbG8sIHdvcmxkIQ==', 'base64').toString() //还原base64
```