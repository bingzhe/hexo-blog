---
title: html查漏补缺（一）
date: 2016-05-22 16:24:11
tags:
- 学习
- html
- 笔记
categories:
- 学习
- html
---

### 网页是什么
* 网页=html+CSS+JavaScropt
* html 网页元素内容
* css 控制网页样式
* JsvsScript 操作网页内容，实现功能和效果

### 网页乱码的原因是怎样产生的，如何解决？
<!-- more -->
写网页流程有下面几步

* 用编辑器编写HTML文件
* 保存编写的HTML文件
* 用浏览器打开HTML文件
* 浏览器展示HTML文件

乱码是因为保存文件是用的编码方式和浏览器展示文件时用的解码方式不一致造成的。解决的话是在html文件`<head>`里添加`<mate charset="utf-8">`。乱码是一般在非英文以外的字符上出现的。告诉浏览器你采用的编码方式。utf-8 是编码方式，还有gbk等

### unicode 和 utf-8,gbk有什么区别

* UNICODE

只用2个字节(16位)就可以编码地球上所有地区的文字。但是，UNICODE只是理论上的编码方式，相当于给世界上每个文字打了个编号，但这编号具体如何在计算机里面存储，可以有多种实现方式。比如utf-8和gbk。

* utf-8

utf-8（8-bit Unicode Transformation Format）是一种针对Unicode的可变长度字符编码，又称万国码。UTF-8用1到6个字节编码UNICODE字符。用在网页上可以同一页面显示中文简体繁体及其它语言（如英文，日文，韩文）。

* gbk

中国制定的一套自己的规则，用2字节表示一个汉字，覆盖2万多个汉字。

### 颜色有几种写法?

* 三种
    * 颜色名字：“red,biue,green”
    * 十六进制颜色：#ffffff #cccccc 等
    * rgb : rgb(225,225,225) rgb(0,0,0)等。还有一种rgba,如rgba(0,225,0,0.3),0.3指的是透明度

### `<!DOCTYOE html>` 有什么作用？

* 一种声明，告诉浏览器文档采用的是html5规范，如果不写，浏览器可能用他自己的规范渲页面，可能会由于浏览器的不同渲染出来的页面也有所不同。

### 严格模式和混杂模式有什么区别？

* 严格模式有声明`<!DOCTYOE html>` ，严格采w3school 标准
* 混杂模式没有申明`<!DOCTYOE html>` 。

### `mate` 有什么作用，常见的值有哪些？

* name 属性

    1. Keywords（关键字）：告诉搜索引擎你的网页关键字。
    2. description（网站内容描述）：告诉搜索引擎你的网站主要内容。
    3. robots（机器人向导）：告诉搜索引擎那些页面需要被搜索，那些不需要。
    4. 还有author generator COPYRIGHT revisit-after等。

* http-equiv属性

    1. Expires(期限)：可以用于设定网页的到期时间。一旦网页过期，必须到服务器上重新传输
    2. Pragma(cache模式)：禁止浏览器从本地计算机的缓存中访问页面内容。
    3. Refresh(刷新)：自动刷新并指向新页面。
    4. Set-Cookie(cookie设定)：如果网页过期，那么存盘的cookie将被删除。
    5. Window-target(显示窗口的设定)：强制页面在当前窗口以独立页面显示。
    6. content-Type(显示字符集的设定)：设定页面使用的字符集。
    7. content-Language（显示语言的设定）
    8. http-equiv="imagetoolbar"：指定是否显示图片工具栏。
    9. Content-Script-Type：W3C网页规范，指明页面中脚本的类型。

### 常见的浏览器有哪些，什么内核？

 * Internet Explorer 内核：Trident
 * safari 内核：Webkit
 * Chrome 内核：Webkit
 * Firefox 内核：Gecko
 * Opera 内核：Pesto
