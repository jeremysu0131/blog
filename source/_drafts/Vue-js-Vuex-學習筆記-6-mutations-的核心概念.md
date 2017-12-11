---
title: '[Vue.js] Vuex 學習筆記 (6) - mutations 的核心概念'
tags:
---

# Mutations

**提交 mutations 是改變 Vuex 中 store 的唯一方式**。 mutations 非常類似於組件中的事件（event），每個 mutation 都有一個字串的事件**類型** (type) 和一個**回調函數** (handler)， handler 就是我們實際進行狀態更改的地方，並且他會接受 state 作為第一個參數。

```js
const store = new Vuex.Store({
  state: {
    count: 1
  },
  mutations: {
    increment (state) {
      // 變更狀態
      state.count++;
    }
  }
});
```

我們不能像調用 state 一樣調用 mutations ，而是要以 `store.commit` 的方法來做呼叫。

```js

```