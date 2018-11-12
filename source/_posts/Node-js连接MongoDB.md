---
title: Node.js连接mongoDB
date: 2018-08-12 14:39:08
tags:
- mongoDB
- Node.js
categories: 
- mongoDB
---

## 安装mongodb

``` js
npm install mongodb --save
```

## 连接MongoDB

需要已经安装并启动mongoDB服务,创建app.js,添加下列代码。使用`node app.js`运行。

<!-- more -->
``` js
const MongoClient = require('mongodb').MongoClient;
const assert = require('assert');

// Connection URL
const url = 'mongodb://localhost:27017';

// Create a new MongoClient
const client = new MongoClient(url);

client.connect(function(err) {
  assert.equal(null, err);
  console.log("Connected successfully to server");

  const db = client.db(dbName);

  client.close();
});

```

[文档](http://mongodb.github.io/node-mongodb-native/)