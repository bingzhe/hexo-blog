---
title: rem布局实现思路
date: 2017-07-05 18:57:05
tags:
- rem
- 移动端布局
categories: 
- CSS
---

## rem

css3 中引入了新的长度单位，rem。  官方定义 font size of the root element, rem是相对单位，是相对于根元素html的font-size进行计算。

## 针对不同分辨率计算font-size

根据浏览器界面更改html的font-size

<!-- more -->
``` js
(function(doc, win) {
    var docEl = doc.documentElement,
        resizeEvt = 'orientationchange' in window ? 'orientationchange' : 'resize',
        recalc = function() {
            var clientWidth = docEl.clientWidth;
            if (!clientWidth) return;
            docEl.style.fontSize = 20 * (clientWidth / 320) + 'px';
        };
    if (!doc.addEventListener) return;
    win.addEventListener(resizeEvt, recalc, false);
    doc.addEventListener('DOMContentLoaded', recalc, false);
})(document, window);
```

## 配合css预处理工具sass可以计算 rem 值

``` js
//scss
$hfs: 16px;   //默认值可以更具需求来定义

@function pxTorem($px){//$px为需要转换的字号
    @return $px / $hfs * 1rem;
}

//使用
.class {
    font-size: pxTorem(12px);
}

//css
.class {
    font-size: 0.75rem;
}

```