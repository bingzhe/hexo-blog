---
title: json-server模拟数据
date: 2017-03-31 13:32:09
tags: 
- JavaScript
categories:
- JavaScript 
---
json-server可以模拟后端提供的数据，让前端流程走通，实现前后端分离
### 安装
    npm isntall json-server --save-dev
### 使用
    // server.js
    const jsonServer = require('json-server')
    const server = jsonServer.create()
    const router = jsonServer.router('db.json')
    const middlewares = jsonServer.defaults()
<!--more-->
    server.use(middlewares)
    server.use(router)
    server.listen(3000, () => {
    console.log('JSON Server is running')
    })
<!--![](json.png)-->
{% asset_img json.png %}
### 创建db.json文件
    {
    "posts": [
        { "id": 1, "title": "json-server", "author": "typicode" }
    ],
    "comments": [
        { "id": 1, "body": "some comment", "postId": 1 }
    ],
    "profile": { "name": "typicode" }
    }
### 创建后
<!--![](success.png)-->
{% asset_img success.png %}
### 代理
<!--![](1.png)-->
{% asset_img 1.png %}
<!--![](2.png)-->
{% asset_img 2.png %}
### 链接
[json-server](https://github.com/typicode/json-server)