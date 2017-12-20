---
title: '[Vue.js] Vuex 學習筆記 (10) - 專案結構'
date: 2017-12-19 17:49:07
categories: Vue.js
tags:
- Vue
- Vue.js
- vue-cli
- Vuex
- structure
- modules
---

# 專案結構

Vuex 並不限制你的程式碼結構，但仍然有規定了一些需要遵守的規則：

<!-- more -->

1. 應用層級的 state 應該要集中到單個 store 物件中。
2. 提交 mutation 是更改狀態餓唯一方法，並且這個過程好似同步的。
3. 異步邏輯都應該封裝到 action 裡面。

只要遵守以上規則，如何組織程式碼都是可以的，但是如果把全部程式碼放在 store 的文件中，會顯得有些雜亂，所以我們可以將 action 、 mutation 和 getter 分割到單獨的檔案。

而對於大型應用，我們會希望把 Vuex 相關程式碼分割到模塊中。

以下是一個[購物車](https://github.com/vuejs/vuex/tree/dev/examples/shopping-cart)專案結構範例：

```shell
├── index.html
├── main.js
├── api
│   └── ... # 抽取出 API 請求
├── components
│   ├── App.vue
│   └── ...
└── store
    ├── index.js          # 組裝模塊並導出 store 的地方
    ├── actions.js        # 根級別的 action
    ├── mutations.js      # 根級別的 mutation
    └── modules
        ├── cart.js       # 購物車模塊
        └── products.js   # 產品模塊
```
