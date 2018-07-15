---
title: vue-resource实现Ajax获取数据
date: 2017-03-31 13:13:07
tags: 
- 学习
- vue
categories:
- 学习
- vue
---
vue-resource是Vue提供的Ajax请求插件
### 安装
    npm install vue-resource
### 引入
    import VueResource from 'vue-resource'
    Vue.use(VueResource)
<!--more-->
### 使用
        // GET /someUrl
    this.$http.get('/someUrl').then(response => {
            // get body data
        this.someData = response.body;
        }, response => {
            // error callback
        });
### 链接
* [vue-resource](https://github.com/pagekit/vue-resource)
* [Vue-resource实现ajax请求和跨域请求](http://blog.csdn.net/wcslb/article/details/55057010)