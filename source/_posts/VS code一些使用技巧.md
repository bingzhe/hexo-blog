---
layout: vs
title: VSCode一些使用技巧
date: 2017-04-1 20:34:11
tags: 
- Tools
categories:
- Tools
---
最开始使用的编辑器是Atom，但有一段时间只要打开jQuery源码就挂掉，直接卡死，在公司的电脑和自己的电脑上都是如此，卸载重新安装也不行，只好换编辑器，之后也下载了传说中的神器Sublime Text3，但也没装什么插件，就换成了WebStorm，IDE果然很强大，一直用着，老大开始推VS code，就安装下来，试着用了一下，顺便记录一些插件在换机器的时候备用。  
### 安装

直接网站下载安装 [Visual Studio Code](https://code.visualstudio.com/)
<!--more-->
### 简介
* VScode是微软推出的一款轻量级的编辑器,采用了和VS相同的UI界面。
* 左侧是用于展示所要编辑的所有文件和文件夹的文件管理器，依次是`资源管理器`，`搜索`，`GIT`，`调试`，`插件`，右侧是打开文件的编辑区域，最多可同时打开三个编辑区域到侧边。

<!--![左侧布局](左侧.png)-->
{% asset_img 左侧.png 左侧布局%}

* 底栏部依次是`Git Branch`，`error&warning`，`编码格式`等。 
### 插件
* VScode-icons: 美化VSCode的界面，在文件名前面显示小图标，安装后每次打开自动启用。
* Git Easy: 增加了vscode中自带的git操作，安装后按F1调出控制台，输入git easy [options]完成git操作，代替git bash。
* Debugger for Chrome: 方便js调试的插件，前端项目在Chrome中运行起来之后，可以直接在VSCode中打断点、查看输出、查看控制台，需要配置launch.json,详情见插件说明。
* vue 2 Snippets: Vue文件高亮
* View In Browser: 从浏览器中查看html文件，使用系统的当前默认浏览器,只支持html文件,默认的快捷键Ctrl+F1
* Material-theme: 我觉的还好的主题吧

其他的一些插件

* jQuery Code Snippets：juqery提示插件
* Path Intellisense：自动路劲补全，默认不带这个功能的
* HTML Snippets：支持HTML5的标签提示
* HTML CSS support：css自动补齐
* ESLint：检测JS
* background：可以自己设置vsc的背景图
* Bootstrap 3 Snippets：bootstrap必备

### 常用快捷键
    打开一个新窗口： Ctrl+Shift+N
    关闭窗口： Ctrl+Shift+W
    新建文件 Ctrl+N
    历史打开文件之间切换 Ctrl+Tab，Alt+Left，Alt+Right
    切出一个新的编辑器（最多3个）Ctrl+\，也可以按住Ctrl鼠标点击Explorer里的文件名
    左中右3个编辑器的快捷键Ctrl+1 Ctrl+2 Ctrl+3
    3个编辑器之间循环切换 Ctrl+`
    编辑器换位置，Ctrl+k然后按Left或Right
格式调整

    代码行缩进Ctrl+[， Ctrl+]
    折叠打开代码块 Ctrl+Shift+[， Ctrl+Shift+]
    Ctrl+C Ctrl+V如果不选中，默认复制或剪切一整行
    代码格式化：Shift+Alt+F，或Ctrl+Shift+P后输入format code
    修剪空格Ctrl+Shift+X
    上下移动一行： Alt+Up 或 Alt+Down
    向上向下复制一行： Shift+Alt+Up或Shift+Alt+Down
    在当前行下边插入一行Ctrl+Enter
    在当前行上方插入一行Ctrl+Shift+Enter
光标相关

    移动到行首：Home
    移动到行尾：End
    移动到文件结尾：Ctrl+End
    移动到文件开头：Ctrl+Home
    移动到后半个括号 Ctrl+Shift+]
    选中当前行Ctrl+i（双击）
    选择从光标到行尾Shift+End
    选择从行首到光标处Shift+Home
    删除光标右侧的所有字Ctrl+Delete
    Shrink/expand selection： Shift+Alt+Left和Shift+Alt+Right
    Multi-Cursor：可以连续选择多处，然后一起修改，Alt+Click添加cursor或者Ctrl+Alt+Down 或 Ctrl+Alt+Up
    同时选中所有匹配的Ctrl+Shift+L
    Ctrl+D下一个匹配的也被选中(被我自定义成删除当前行了，见下边Ctrl+Shift+K)
    回退上一个光标操作Ctrl+U
重构代码

    跳转到定义处：F12
    定义处缩略图：只看一眼而不跳转过去Alt+F12
    列出所有的引用：Shift+F12
    同时修改本文件中所有匹配的：Ctrl+F12
    重命名：比如要修改一个方法名，可以选中后按F2，输入新的名字，回车，会发现所有的文件都修改过了。
    跳转到下一个Error或Warning：当有多个错误时可以按F8逐个跳转
    查看diff 在explorer里选择文件右键 Set file to compare，然后需要对比的文件上右键选择Compare with 'file_name_you_chose'.
查找替换
    查找 Ctrl+F
    查找替换 Ctrl+H
    整个文件夹中查找 Ctrl+Shift+F
    全屏：F11
    zoomIn/zoomOut：Ctrl + =/Ctrl + -
    侧边栏显/隐：Ctrl+B
    预览markdown Ctrl+Shift+V

###　相关文档
* 官方文档（英文版）：[code.visualstudio.com/docs](https://code.visualstudio.com/docs)
* 中文文档：[VScode中文文档](https://www.gitbook.com/book/jeasonstudio/vscode-cn-doc/details)
* 扩展：[扩展](https://marketplace.visualstudio.com/VSCode)
