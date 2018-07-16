---
title: HTML查漏补缺（四）
date: 2016-06-03 23:05:43
tags:
- HTML
categories:
- HTML
---

### 有序列表、无序列表、自定义列表在语义上的区别

* 有序列表，使用数字标记项目
* 无序列表，使用黑点标记项目
* 自定义列表，用来定义和解释的列表
* 有序列表适合各项目之间有顺序关系的情况，无序列表适用于各项目之间没有顺序关系的情况，自定义列表适用于表示项目和其注释的组合。无序列表是使用最多的一种。

### 如何去除列表的默认样式
<!-- more -->

``` css
    li{
        list-style:none;
    }
```

### class 和 id 在语义上的区别

* class 是描述元素的身份的属性，同一个页面上不同元素可以命名为同一个class的属性名，在CSS中我们可以用.加上class属性名选择同一属性名的元素，进行样式设计。
* id 是描述元素唯一标识的属性，在同一个html文件中只有一个，是独一无二的。在CSS中可以用#加上id名选择这个元素。

### 块级元素、行内元素的区别

* 块级元素，上下都有换行，排列时是向下排列，独自占一行。
* 行内元素，行内元素可以与其他行内元素排列在同一行，排列时是从左向右排列，只有在空间位置不够时才会换行。
* 块级元素可以设padding margin width height 。行内元素设width 和 height是无效的，而设padding和margin时，左右是有效的，而上下是没有效果的。

### `display:block`、`display:inline`和`display:inline-block`的作用

* `dispaly:block`将元素显示为块级元素。
* `siaplay:inline`将元素显示为行内元素。
* `display:block-inline`将元素显示为行内块元素，不换行也具有块级元素的属性。不支持IE8以下。

### 如何理解 HTML CSS 语义化

* 用正确的标签做正确的事情。
* html语义化让页面的内容结构化，结构更清晰，便于对浏览器、搜索引擎解析;
* 即使在没有样式CSS情况下也以一种文档格式显示，并且是容易阅读的;
* 搜索引擎的爬虫也依赖于HTML标记来确定上下文和各个关键字的权重，利于SEO;
* 使阅读源代码的人对网站更容易将网站分块，便于阅读维护理解。

### form表单的作用

* form表单就是接受用户提交的数据，并在用户点提交时将数据传送给服务器，以便服务器端的程序处理数据。
* name 定义提交的数据的名称。
* action 提交到的地址。
* method 提交的数据的方式，有get和post两种。
* type:
    * text: 一行文字
    * textarea: 一段文字
    * password: 提交用户密码，显示为黑点
    * radio: 单选圆圈
    * checkbox: 复选选项
    * hidden：暂存一些数据，可以进行安全校验。
* max 规定input元素的最大值。

### post 和get的区别

* 都是指表单向服务器传送数据用的方式
* 区别有
    1. 数据提交方式不同，get把提交的数据url可以看到，post看不到。 
    2. get一般用于提交少量数据，post用于提交大量数据。
    3. get最多提交1k数据，浏览器的限制。post理论上无限制，受服务器限制。
    4. get提交的数据在浏览器历史记录中，安全性不好。

### 在input里面，name的作用

* 为input元素定义了唯一的名称 ，没有name就无法提交

###  `<button>提交</button>`,`<a class="btn" href="#">提交</a>`,`<input type="submit" value="提交">`的区别？

* `<button>提交</button>`是一个提交按钮
* `<a class="btn" href="#">提交</a>`是一个链接，点提交跳转
* `<input type="submit" value="提交">`是提交表单，点提交提交表单

## radio如何分组？

``` html
    <form>
        <p>性别：
            <input type="radio" name="sex" value="male">男</br>
            <input type="radio" name="sex" value="female">女</br>
        <input type="submit" value="提交">
        </p>
    </form>
```

* name相同时就能将radio分为一组，选项单选。

## placeholder 属性

* 在输入框中显示提示用户应该输入的内容。

## type=hidden隐藏域的作用

* 隐藏域在页面中对于用户是不可见的，在表单中插入隐藏域的目的在于收集或发送信息，以利于被处理表单的程序所使用。浏览者单击发送按钮发送表单的时候，隐藏域的信息也被一起发送到服务器。
* 用户提交一个表单上来时要确定用户的身份，可以用hidden里的信息核对确认。