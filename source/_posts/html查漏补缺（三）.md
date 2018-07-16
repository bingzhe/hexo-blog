---
title: HTML查漏补缺（三）
date: 2016-05-29 11:34:37
tags:
- HTML
- CSS
categories:
- HTML
---
### `line-height`作用

 设置行高。

### 检查CSS的兼容性

* 可以在[can i use ](http://caniuse.com) 上查看各浏览器对CSS的兼容情况。
* 在caniuse.com查询`inline-block`的兼容情况。

<!-- more -->

{% asset_img caniuse.png %}

### a标签的herf,title,target属性

* 例子`<a herf="http://www.baidu.com title="百度" target="_blank">百度</a>`
* `herf`属性规定链接的目标，可以是有效文档的相对和绝对URL。
* `title`属性规定链接的额外信息，鼠标放在链接上时会显示出来。
* `target`属性规定在何处打开链接，`target="_blank`会在新窗口打开链接。
* `alt`属性是在图片不能打开时在图片位置显示的文本。
* `target="_blank"`在新窗口打开链接。

### `display:none`,`visibility:hidden`和`opacity:0`

* `display:none`是将元素完全隐藏，而且不占用页面，所占的页面会被其他的元素占据，功能完全消失，就是看不见也摸不着。
* `visibility:hidden`是将元素隐藏，所占空间还在，只是不显示，就是说看不见但摸得着。
* `opacity:0`设置元素的透明级别，0时元素完全透明但依然存在。

### 如何去除 a 链接的默认样式

* 去除a链接默认样式 `a{text-decoration: none;}`
* a链接不能继承父容器color属性
