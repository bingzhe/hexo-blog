---
title: html查漏补缺（二）
date: 2016-05-23 23:30:03
tags:
- 学习
- css
- 笔记
categories:
- 学习
- css
---

### 样式引入方式

样式有三种引入方式

* 外部样式表

``` html
<link rel="stylesheet" type="text/css" herf="index.css">`
```

* 内部样式表（位于标签内部）

``` html
<style　type="text/css">
    p {color:red;}
</style> 
```
<!-- more -->
* 内联样式(在HTML元素内部)

``` html
<p style="color: red;"></p>
```

### `link`和`@import`区别

1. 归属差别，`link`属于XHTML标签，而`@import`是CSS提供的一种方式。`link`标签除了可以加载css文件以外，还可以做其他的一些事情，比如声明页面链接属性，声明目录等。而`@import`只能用来加载CSS。
2. 加载顺序不同，当一个页面开始被加载时，`link`引用的CSS会同时被加载。而`@import`引用的CSS会在页面被全部加载完成之后在加载。
3. 兼容性的不同，`@import`是在CSS2.1的时候提出，所以老的浏览器不支持，只有在IE5以上才能识别，而`link`标签没有这个问题。
4. 使用DOM控制样式时候的差别，当使用JavaScript控制DOM去改变样式时，只能使用`link`标签。
5. `@import`可以在CSS样式表中引入其他的样式。

### 文件路径

* ../main.css 表示上一级文件夹下的main.css
* ./css/main.css 表示当前文件夹中的css文件夹中的main.css
* main.css和./main.css 表示当前文件夹的mian.css

### `console.log` 作用

* `console.log`主要是用于调试代码，他会将调试信息输出到console控制tail上，并且不会影响网页的正常解析和显示。

### `text-align`值的作用

* left 左对齐
* right 右对齐
* center 居中
* justity 两侧对齐


### `px`,`em`,`rem`区别

* `px`：像素
* `em`：相对于父容器
* `rem`：相对于HTML根节点，浏览器默认1em=16px。

### 对chrome 审查元素的功能

{% asset_img shencha.png %}

### 参考

[MDN-Web技术文档-CSS](https://developer.mozilla.org/zh-CN/docs/Web/CSS)