---
title: TypeScript中使用ESlint
date: 2019-04-29 20:22:29
tags:
  - eslint
  - TypeScript
categories:
  - TypeScript
---

TypeScript 已经不再维护 TSlint，转投 ESlint，所以代码检查也切到 ESlint。

<!--more-->

## 安装依赖

```js
yarn add eslint @typescript-eslint/parser @typescript-eslint/eslint-plugin --dev
```

## 添加.eslintrc.js 配置文件

```js
module.exports = {
  parser: "@typescript-eslint/parser", // Specifies the ESLint parser
  extends: [
    // 'plugin:react/recommended',  // Uses the recommended rules from @eslint-plugin-react
    // 'plugin:@typescript-eslint/recommended',  // Uses the recommended rules from @typescript-eslint/eslint-plugin
  ],
  parserOptions: {
    ecmaVersion: 2018, // Allows for the parsing of modern ECMAScript features
    sourceType: "module", // Allows for the use of imports
    ecmaFeatures: {
      // 不允许 return 语句出现在 global 环境下
      globalReturn: false,
      // 开启全局 script 模式
      impliedStrict: true,
      jsx: true // Allows for the parsing of JSX
    }
  },
  env: {
    browser: true,
    node: true,
    commonjs: true,
    es6: true
  },
  // 以当前目录为根目录，不再向上查找 .eslintrc.js
  root: true,
  //指定你所要使用的全局变量，true代表允许重写、false代表不允许重写
  globals: {
    describe: false,
    it: false,
    expect: false
  },
  rules: {
    //   定义规则
  },
  settings: {
    react: {
      version: "detect" // Tells eslint-plugin-react to automatically detect the version of React to use
    }
  }
};
```

## 启用保存时候自动修复（VScode）

```js
"eslint.autoFixOnSave":  true,
"eslint.validate":  [
  "javascript",
  "javascriptreact",
  {"language":  "typescript",  "autoFix":  true  },
  {"language":  "typescriptreact",  "autoFix":  true  }
],
```

如果无效的时候，检查下是否安装`eslint-plugin-html`
