---
title: '[Vue.js] Vuex 學習筆記 (12) - 嚴格模式'
date: 2017-12-19 23:34:52
categories: Vue.js
tags:
- Vue
- Vue.js
- vue-cli
- Vuex
- strict
- strict mode
---

# 嚴格模式 Strict Mode

開啟嚴格模式只需要在建立 store 時傳入 `strict: true` 。

<!-- more -->

```js
const store = new Vuex.Store({
  // ...
  strict: true
});
```

在嚴格模式下，不管何時發生了狀態變更且不是由 mutation 函數引起的，將會拋出錯誤，這樣能保證所有的狀態變更都能被調適工具追蹤到。

## 開發與發佈環境

**不要在發佈環境下啟用嚴格模式**，嚴格模式會深度檢測狀態樹來檢測不符合規定的狀態變更，要記得在發佈時關閉嚴格模式，以免降低效能，我們可以使用以下方式來處理這種情況：

```js
const store = new Vuex.Store({
  // ...
  strict: process.env.NODE_ENV !== "production"
});
```
