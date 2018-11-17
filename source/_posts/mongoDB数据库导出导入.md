---
title: mongoDB数据库导出导入
date: 2018-11-17 18:09:48
tags:
- mongoDB
categories: 
- mongoDB
---
---

## mongoDB数据库导出/备份

``` js
// 语法
mongodump -h dbhost -d dbname -o dbdirectory
// eg
mongodump -h 127.0.0.1 -d koa_db -o C:\Users\Administrator\Desktop\koabase
```

参数：
1. -h mongoDB所在服务器的地址，例如：127.0.0.1，或者指定端口号：127.0.0.1:27017
2. -d 需要导出的数据库，例如：koa_db
3. -o 备份的数据存放位置，例如：C:\Users\Administrator\Desktop\koabase

<!-- more -->

## mongoDB数据库导入/恢复

``` js
// 语法
mongorestore -h dbhost -d dbname path
// eg
mongorestore -h 127.0.0.1 -d koa_demo C:\Users\Administrator\Desktop\koabase\koa_db
```

参数：
1. -h mongoDB所在服务器的地址，例如：127.0.0.1，或者指定端口号：127.0.0.1:27017
2. -d 需要导导入的数据库，例如：koa_demo
3. path 备份的数据存放位置，例如：C:\Users\Administrator\Desktop\koabase


