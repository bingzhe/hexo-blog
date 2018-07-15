---
title: vuex数据本地储存
date: 2017-08-27 23:47:04
tags: 
- 学习
- vue
- vuex
categories: 
- 学习
- vue
---
## 基本逻辑

vuex 是 vue 的数据管理插件，存储的数据是响应式的。但是并不会保存起来，刷新之后就回到了初始状态，思路就是在vuex里数据改变的时候把数据拷贝一份保存到localStorage里面，刷新之后，如果localStorage里有保存的数据，取出来在替换store里的state。

## 实现

localstore 存储

<!--more-->

``` js
//pageStore.js
export const ls = {
    //本地存数据，days 有效时间（天）
    setItem: function(key, value, days) {
        let Days = Days || 7; //有效时间默认7天
        let exp = new Date();
        let expires = exp.getTime() + Days * 24 * 60 * 60 * 1000;

        localStorage.setItem(key, JSON.stringify({
            value,
            expires
        }));

    },
    getItem: function(key) {
        let o = JSON.parse(localStorage.getItem(key));

        if (!o || o.expires < Date.now()) {
            return null
        } else {
            return o.value
        }
    },
    removeItem: function(key) {
        localStorage.removeItem(key)
    }
}
```

vuex 插件

```js
import { ls } from './pageStore'

function copy(obj) {
    var copy = Object.create(Object.getPrototypeOf(obj));
    var propNames = Object.getOwnPropertyNames(obj);

    propNames.forEach(function(name) {
        var desc = Object.getOwnPropertyDescriptor(obj, name); //获取指定对象的自身属性描述符
        Object.defineProperty(copy, name, desc);
    });

    return copy;
}


export const vuexToLocalStorage = store => {
    // 当 store 初始化后调用
    const savedState = ls.getItem('storeClone');

    if (savedState) {
        store.replaceState(savedState);
    }

    store.subscribe((mutation, state) => {
        // 每次 mutation 之后调用
        // mutation 的格式为 { type, payload }

        let storeClone = copy(state);
        ls.setItem('vuex', storeClone);

    })
}
```
store 引入后 使用

```js
export default new Vuex.Store({
    state,
    getters,
    actions,
    mutations,
    plugins: [vuexToLocalStorage]
})
```

本地测试过两份数据都是刷新之后数据还保存