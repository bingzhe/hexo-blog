---
title: ios9一些JavaScript兼容
date: 2018-12-26 14:26:49
tags:
- 笔记
categories:
- 笔记
---

ios9浏览器上一些坑。

1. ios9不支持let
2. promise

<!-- more -->

``` js
//安装
npm install --save @babel/polyfill es6-promise
//使用
import "@babel/polyfill";
import Es6Promise from "es6-promise";
Es6Promise.polyfill();
```
3. 不支持URLSearchParams方法

``` js
npm install --save url-search-params-polyfill

import "url-search-params-polyfill";
```