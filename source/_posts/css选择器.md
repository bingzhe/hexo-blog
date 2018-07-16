---
title: CSS选择器
date: 2016-06-06 18:31:30
tags:
- CSS
categories:
- CSS
---
### 常见的css选择器

* `*`通用元素选择器

``` css
* {
    font-size:14px;
}
```

<!-- more -->      
* `#id`id选择器,匹配特定的id元素

``` css
#id{
    color:red:
}
```

* `.class`类选择器，匹配特定类的元素

``` css
.class{
    background-color:blue;
}
```

* element 标签选择器

``` css
p{
    line-height:20px;
}
```

* 组合选择器 
* 属性选择器
* 伪类选择器 

### 选择器的优先级

* 优先级从高到低分别为
 1. 在属性后面使用`!important`会覆盖页面内任何位置定义的元素样式 
 2. 作为style属性写在元素标签的内联样式
 3. id选择器
 4. 类选择器
 5. 伪类选择器
 6. 属性选择器
 7. 标签选择器
 8. 通配符选择器
 9. 浏览器默认

* 多个选择器时,对每个选择器的优先权值做加法运算
 1. 权重高的优先
 2. 相等时后面的优先

### class和id的使用场景

* id 页面中是独一无二的使用，例如页脚等
* class 一些有相同的特定样式的标签，可以给他们添加一个同样的class，将相同的部分定义出来。

### 使用css选择器时为什么要划定适当的命名空间

* 更好的匹配需要特定匹配的元素，避免样式污染，使后期的维护更加容易

### 以下选择器的意思？

* `#header{}` id选择器，选择为id名为header的元素。
* `.header{} ` class选择器，选择class名为header的元素。
* `.header .logo{}` 选择class名为header的元素中class名为logo的子元素。
* `.header.mobile{}`选择class为header和mobile的元素。
* `.header p, .header h3{}`选择class为header的子元素p和h3元素。
* `#header .nav>li{}`选择id为header的元素中class为nav的元素的直接子元素li。
* `#header a:hover{}` 选择id为header的元素中啊标签鼠标悬停时的样式。

### 列出你知道的伪类选择器？

* E:hover 匹配鼠标悬停其上的E元素。
* E:active 匹配鼠标已经在其上按下还没有放手的E元素。
* E:focus 匹配获得当前焦点的E元素。
* E:link 匹配所有未被点击的链接。
* E:visited 匹配所有已经被点击的链接。

### `:first-child`和`:first-of-type`的作用和区别

* `:first-child` 匹配父元素下的第一个子元素。
* `:first-of-type` 匹配父元素下同种标签的第一个子元素。

{% asset_img css1.png %}

### `text-align:center`的作用

* 定义行内内容（例如文字、span元素）如何相对它的块父元素对齐。作用是将文本在水平方向上居中。作用在块级元素上，让行内元素水平居中。