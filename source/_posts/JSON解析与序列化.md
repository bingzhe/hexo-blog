---
title: JSON解析与序列化
date: 2017-09-09 21:56:10
tags:
- JavaScript
- JSON
categories: 
- JavaScript
---
## JSON

JSON是JavaScript的一个严格的子集，利用JavaScript中的以小额模式来表示结构化数据,已经成为了互联网传输结构化数据的事实标准

## 语法

JSON可以表示三种类型的值：
- 简单值
使用与JavaScript相同的语法，可以在JSON中表示字符串，数值，布尔值和null。JSON不支持JavaScript中的特殊值
- 对象
复杂数据类型，表示一组无序的键值对
- 数组
复杂数据类型，表示一组有序的键值对

<!--more-->

## 和JavaScript的一些区别，需要注意的地方

1. JSON字符串必须使用双引号
2. JSON中对象的属性名任何时候都必须加上双引号

## JSON解析

早起的JSON解析器基本上使用的是JavaScript的`eval()`函数。
ECMAScript 5对解析JSON的行为进行了规范，定义了全局对象JSON,JSON对象有两个方法：`stringify()`和`parse()`;
旧版本浏览可以使用这个[shim](https://github.com/douglascrockford/JSON-js)

### JSON.stringify()
用于把JavaScript对象序列化成为JSON字符串。

语法：`JSON.stringify(value[, replacer [, space]])`
`value`: 将要序列化成 一个JSON 字符串的值。
`replacer`: 可选，过滤器，如果该参数是一个函数，则在序列化过程中，被序列化的值的每个属性都会经过该函数的转换和处理；如果该参数是一个数组，则只有包含在这个数组中的属性名才会被序列化到最终的 JSON 字符串中；如果该参数为null或者未提供，则对象所有的属性都会被序列化。
`space`: 可选， 指定缩进用的空白字符串，用于美化输出；如果参数是个数字，它代表有多少的空格；上限为10。该值若小于1，则意味着没有空格；如果该参数为字符串(字符串的前十个字母)，该字符串将被作为空格；如果该参数没有提供（或者为null）将没有空格。

### `JSON.parse()` 用于
解析一个JSON字符串，构造由字符串描述的JavaScript值或对象。
语法：`JSON.parse(text[, reviver])`
`text`：要被解析成JavaScript值的字符串。
`reviver =`: 如果是一个函数，则规定了原始值如何被解析改造，在被返回之前。



