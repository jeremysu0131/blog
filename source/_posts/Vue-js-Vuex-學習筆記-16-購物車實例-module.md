---
title: '[Vue.js] Vuex 學習筆記 (16) - 購物車實例 module'
date: 2017-12-24 13:21:50
categories: Vue.js
tags:
- Vue
- Vue.js
- vue-cli
- Vuex
- shopping cart
- module
---

## module

由於購物車與商品各自有許多不同的函數，為了未來維護上方便，我們將其拆成獨立的檔案，這樣在 store 中就不會一次出現很長一串的程式碼，這裡我們以 products 來做範例。

### products

```js
import shop from "../../api/shop";
// 這邊定義了購物車會用到的功能，這樣 IDE 就可以提供 intellisense ，且 linter 之類的工具可以使用
// 並且未來只要到 mutation-types 裡面查看就可以知道整個應用裡面有哪些功能
import * as types from "../mutation-types";

// initial state
const state = {
  all: []
};

// getters
const getters = {
  allProducts: state => state.all
};

// actions
const actions = {
  getAllProducts({ commit }) {
    shop.getProducts(products => {
      commit(types.RECEIVE_PRODUCTS, { products });
    });
  }
};

// mutations
const mutations = {
  [types.RECEIVE_PRODUCTS](state, { products }) {
    state.all = products;
  },

  [types.ADD_TO_CART](state, { id }) {
    state.all.find(p => p.id === id).inventory--;
  }
};

// 將各項定義好的結果輸出
export default {
  state,
  getters,
  actions,
  mutations
};
```

## 引用

模組設定好之後，我們只要在 store 的 modules 引用，就可以使用了。

```js
import Vue from "vue";
import Vuex from "vuex";
// 公用的部分可以抽取出來放在 store 的根目錄
import * as actions from "./actions";
import * as getters from "./getters";
import cart from "./modules/cart";
import products from "./modules/products";
import createLogger from "../../../src/plugins/logger";

Vue.use(Vuex);

const debug = process.env.NODE_ENV !== "production";

export default new Vuex.Store({
  actions,
  getters,
  modules: {
    cart,
    products
  },
  strict: debug,
  plugins: debug ? [createLogger()] : []
});
```
