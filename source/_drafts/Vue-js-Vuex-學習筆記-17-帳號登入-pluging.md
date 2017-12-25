---
title: '[Vue.js] Vuex 學習筆記 (17) - 帳號登入 pluging'
date: 2017-12-24 14:28:05
categories:
tags:
---

# 前言

使用 Vuex 時我們一定會想到説可以使用在帳號登入，紀錄一些使用者的資訊，或是暫時的 token。

但 Vuex 紀錄的是當前網頁的狀態，如果重新整理網頁或是開新分頁，則狀態不會被同步，所以我們可以使用 `localStorage` 來將狀態保存在瀏覽器。

```js
const localStoragePlugin = store => {
  store.subscribe((mutation, { user }) => {
    window.localStorage.setItem("user", JSON.stringify(user));
  });
};

const store = new Vuex.Store({
  // ...
  plugins: [localStoragePlugin]
});
```
