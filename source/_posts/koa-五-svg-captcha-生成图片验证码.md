---
title: koa(五) svg-captcha 生成图片验证码
date: 2018-11-15 14:05:03
tags:
- koa
- Node.js
categories: 
- koa
---

使用`svg-captcha`可以比较方便的生成图片验证码

* [https://github.com/lemonce/svg-captcha](https://github.com/lemonce/svg-captcha)

## 安装

`npm install --save svg-captcha`

## 使用

``` js
const svgCaptcha = require('svg-captcha');

const c = svgCaptcha.create();
console.log(c);
// {data: '<svg.../svg>', text: 'abcd'}
```
<!-- more -->

1. 生成普通验证码

{% asset_img code.png 普通验证码 %}

2. 生成算式验证码

``` js
let captcha = svgCaptcha.createMathExpr();
```

{% asset_img code1.png 算式验证码 %}

## options参数

* size: 4 // 验证码长度
* ignoreChars: '0o1i' // 验证码字符中排除 0o1i
* noise: 1 // 干扰线条的数量
* color: true // 验证码的字符是否有颜色，默认没有，如果设定了背景，则默认有
* background: '#cc9966' // 验证码图片背景颜色
* fontSize: 50
* width: 100
* height:40


## 在koa中使用

前端请求一个URL返回一个验证码

``` js

//验证码
router.get('/code', async (ctx) => {
    let captcha = svgCaptcha.create({
        size: 4,
        fontSize: 50,
        noise: 3,
        width: 120,
        height: 34,
        background: "#cc9966"
    });

    ctx.session.code = captcha.text;
    //设置响应头
    ctx.response.type = "image/svg+xml";
    ctx.body = captcha.data;
});
```



