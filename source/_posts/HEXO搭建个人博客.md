---
title: HEXO搭建个人博客
date: 2017-03-27 16:38:06
tags: 
- Tools
categories:
- Tools
---
突然心血来潮，想搭建一个博客，就在网上查了一些资料，决定用hexo+Github来搭建。
记录一下搭建过程。
## 安装前提
确保电脑安装下列程序：
* Node.js
* Git

## 安装Hexo
输入下面命令安装Hexo
```
npm install -g hexo-cli
```
<!--more-->
## 建站
输入下面命令初始化

    hexo init <folder>
    cd <folder>
    npm install
新建完成后的文件夹目录

<!--![文件夹目录](1.png)-->
{% asset_img 1.png 文件夹目录%}

生成静态文件

    hexo generate

启动本地服务器，进行文章调试预览

    hexo server

浏览器中输入http://localhost:4000 预览

## 部署
安装hexo-deployer-git。

    npm install hexo-deployer-git --save

修改_config.yml配置

     deploy:
      type: git
      repo: <repository url> //库（Repository）地址
      branch: [branch]
      message: [message]

部署

    hexo deploy
每次部署步骤

    hexo clean
    hexo generate
    hexo deploy
一些常用命令

    hexo new "postName" #新建文章
    hexo new page "pageName" #新建页面
    hexo generate #生成静态页面至public目录
    hexo server #开启预览访问端口（默认端口4000，'ctrl + c'关闭server）
    hexo deploy #将.deploy目录部署到GitHub
    hexo help  #查看帮助
    hexo version  #查看Hexo的版本
## 修改主题
下载

    $ git clone https://github.com/giscafer/hexo-theme-cafe.git themes/cafe

使用

    修改博客配置文件 `_config.yml` 主题属性 theme 为 `cafe`.

更新升级

    cd themes/cafe
    git pull

## 踩到得坑

刚开始用markdown语法插入图片，但是只能在文章中查看到正确的图片，首页无法显示图片，查了下官网文档，可以用
```
{% asset_img 1.png 文件夹目录%}

```
来插入图片，这样都会显示正常