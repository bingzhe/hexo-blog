---
title: css常见技巧
date: 2016-06-26 19:14:46
tags:
- css
categories:
- 学习
- css
---

### CSS Sprite(雪碧图|精灵图)

雪碧图指将许多很小的icon合成一张大图，通过`background-position`去定位不同的图片来展示不同的icon。

### img标签和CSS背景使用图片在使用场景

页面上固定不变的图片使用CSS背景图，对于可变的内容，图片是和内容相关的使用img图片（验证码，用户头像，广告图片等）。

### `title`和`alt`属性作用
<!-- more -->

* title是鼠标悬浮停留时出现的文字提示。

{% asset_img title.png title %}

* alt是图片链接打不开时替换成的文字，对搜索引擎优化有好处。

{% asset_img alt.png alt%}

### `background:url(abc.png) o o no-repeat;`

以abc.png为背景图片，位置不偏移，图片不重复。

### `background-size`

`background-size`规定背景图片的尺寸。

* cover  把背景图像扩展至足够大，以使背景图像完全覆盖背景区域。背景图像的某些部分也许无法显示在背景定位区域中。
* contain 把图像图像扩展至最大尺寸，以使其宽度和高度完全适应内容区域。
* px 给背景图片制定宽高。
* 百分比 原图像大小的倍数。

### div水平居中,图片水平居中

* div 水平居中 `margin:0 auto;`
* `text-align：center`设置在图片的父容器上使得图片水平居中 

{% asset_img center.png center %}

### `opacity`和`ragb`都可以设置透明度，他们的区别

opacity使元素整体透明，是元素的属性；rgba将颜色设置为透明，是颜色的属性，不涉及子元素。
