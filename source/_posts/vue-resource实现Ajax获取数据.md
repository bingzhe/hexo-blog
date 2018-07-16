---
title: vue-resource实现Ajax获取数据
date: 2017-03-31 13:13:07
tags: 
- Vue
categories:
- Vue
---

vue-resource是Vue提供的Ajax请求插件

### 安装
    npm install vue-resource
### 引入
    import VueResource from 'vue-resource'
    Vue.use(VueResource)
<!--more-->
### 使用
``` js
    // GET /someUrl
    this.$http.get('/someUrl').then(response => {
        // get body data
        this.someData = response.body;
        }, response => {
        // error callback
        });
```

### 更新

vue官方推荐使用[axio](https://github.com/axios/axios)，请查看。

### 链接
* [vue-resource](https://github.com/pagekit/vue-resource)
* [Vue-resource实现ajax请求和跨域请求](http://blog.csdn.net/wcslb/article/details/55057010)