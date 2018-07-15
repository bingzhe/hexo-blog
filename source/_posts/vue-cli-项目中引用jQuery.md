---
title: vue-cli 项目中引用jQuery
date: 2017-05-29 21:12:21
tags: 
- 学习 
- vue
- jquery
categories:
- 学习
- vue
---

需要在vue-cli项目中引入jquery,做法

全局引入

npm下载jquery

```
npm install jquery --save
```
<!--more-->
在webpack.base.conf.js里加入

```
var webpack = require("webpack")
```

在module.exports的最后加入
```
plugins: [
 new webpack.optimize.CommonsChunkPlugin('common.js'),
 new webpack.ProvidePlugin({
     jQuery: "jquery",
     $: "jquery"
 })
]
```
这样就已经全局引入了


webpack.base.conf.js的修改
``` javascript
// 在开头引入webpack，后面的plugins那里需要
var webpack = require('webpack')
// resolve

module.exports = {
   // 其他代码...
   resolve: {
      extensions: ['', '.js', '.vue'],
      fallback: [path.join(__dirname, '../node_modules')],
      alias: {
          'src': path.resolve(__dirname, '../src'),
          'assets': path.resolve(__dirname, '../src/assets'),
          'components': path.resolve(__dirname, '../src/components'),

          // webpack 使用 jQuery，如果是自行下载的
          // 'jquery': path.resolve(__dirname, '../src/assets/libs/jquery/jquery.min'),
          // 如果使用NPM安装的jQuery
          'jquery': 'jquery' 
      }
   },

   // 增加一个plugins
   plugins: [
      new webpack.ProvidePlugin({
          $: "jquery",
          jQuery: "jquery"
      })
   ],

   // 其他代码...
}
```
装上之后效果
{% asset_img jquery1.png %}
{% asset_img jquery2.png %}
