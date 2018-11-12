---
title: 封装操作mongoDB的CRUD操作类
date: 2018-08-12 15:33:37
tags:
- mongoDB
- Node.js
categories: 
- mongoDB
---

建议使用更加成熟的node.js操作mongoDB库，例如 [mongoose](https://mongoosejs.com/) 等，本文指在学习交流。

## 文件目录

```
module
|--config.js  //数据库配置
|--db.js      //操作类
```

## `config.js`数据库配置

``` js
const app = {
    dbUrl: 'mongodb://localhost:27017',
    dbName: 'koa'
}

module.exports = app;
```

## `db.js`操作类

``` js
const MongoClient = require('mongodb').MongoClient;
const Config = require('./config.js');

class DB {

    //单例模式
    static getInstance() {
        if (!DB.instance) {
            DB.instance = new DB();
        }
        return DB.instance;
    }  

    //构造函数
    constructor() {
        this.dbClient = null;
        this.connect();
    }

   //根据配置连接数据库
    connect() {
        return new Promise((resolve, reject) => {
            if (!this.dbClient) {
                const client = new MongoClient(Config.dbUrl);
                client.connect(err => {
                    if (err) {
                        reject(err);
                    } else {
                        this.dbClient = client.db(Config.dbName)
                        resolve(this.dbClient);
                    }
                })
            } else {
                resolve(this.dbClient);
            }
        });
    }
    
    /**
     * 插入数据
     * @param {String} collectionName 表名
     * @param {*} json 插入数据
     */
    insert(collectionName, json) {
        return new Promise((resolve, reject) => {
            this.connect().then(db => {
                db.collection(collectionName).insertOne(json, (err, result) => {
                    if (err) {
                        reject(err);
                        return;
                    }
                    resolve(result);
                });
            });
        });
    }

    /**
     * 查找
     * @param {String} collectionName 表名
     * @param {*} json 查找条件
     */ 
    find(collectionName, json) {
        return new Promise((resolve, reject) => {
            this.connect().then(db => {
                const collection = db.collection(collectionName);

                collection.find(json).toArray((err, docs) => {
                    if (err) {
                        reject(err);
                        return
                    }

                    resolve(docs);
                });
            });
        });
    }

    /**
     * 更新
     * @param {String} collectionName 表名
     * @param {*} json1 查找条件
     * @param {*} json2 更新的数据
     */
    update(collectionName, json1, json2) {
        return new Promise((resolve, reject) => {
            this.connect().then(db => {
                db.collection(collectionName).updateOne(json1, { $set: json2 }, (err, result) => {
                    if (err) {
                        reject(err);
                        return;
                    }
                    resolve(result);
                });
            });
        });
    }    

    /**
     * 删除
     * @param {String} collectionName 表名
     * @param {*} json 查找条件
     */
    remove(collectionName, json) {
        return new Promise((resolve, reject) => {
            this.connect().then(db => {
                db.collection(collectionName).removeOne(json, (err, result) => {
                    if (err) {
                        reject(err);
                        return;
                    }

                    resolve(result);
                });
            });
        });
    }    
}

module.exports = DB.getInstance();
```


## 使用
例如在`koa`中使用

``` js
const koa = require('koa');
const router = require('koa-router')();

const DB = require("./module/db.js");

const app = new koa();

router.get('/add', async (ctx) => {
    let data = await DB.insert('user', { 'username': "赵柳", 'sex': '女', 'age': 29 });
    ctx.body = "add";
});

router.get('/login', async (ctx, next) => {
    let result = await DB.find('user', {'username': '赵柳'});
    ctx.body = result;

});

router.get('/edit', async (ctx) => {
    let data = await DB.update('user', { 'username': '赵柳' }, { 'age': 32 });
    ctx.body = "edit";
});

router.get('/delete', async (ctx) => {
    let data = await DB.remove('user', { 'username': '赵柳' });
    ctx.body = "edit";
});
```