---
title: '[Vue.js] Vuex 學習筆記 (1) - 基礎介紹'
date: 2017-12-06 14:23:41
categories: Vue.js
tags:
- Vue
- Vue.js
- vue-cli
- Vuex
---

# 什麼是 Vuex ？

Vuex 是一個專為 Vue.js 來做使用的**狀態管理模式**與文庫，它作為應用程序中所有組件的集中存儲區，並在規則下確保只能以可預測的方式對狀態進行變更。

<!-- more -->

## **狀態管理模式**（State Management Pattern）是什麼？

```js
new Vue({
  // state
  data() {
    return {
      count: 0,
    };
  },
  // view
  template: `<div>{{ count }}</div>`,
  // actions
  methods: {
    increment() {
      this.count++;
    },
  },
});
```

以上程式碼有下列幾個部分：

- **state** ：這是驅動我們的應用程式的來源。
- **view** ：用聲明的方式將`state`繪製到 view。
- **actions** ：使用者在 view 上的輸入會使得`state`產生變化。

下面是“單向數據流（one-way data flow）”概念的流程圖：
![State Management Pattern](flow.png)

單個組件的狀態非常好管理，但當遇到多個組件共享 state 時，單向數據流的簡潔性就很容易被破壞，例如：

- 多個 view 依賴同一個 state
- 來自不同 view 的 actions 需要變更同一個 state

所以這個時候我們為什麼不把組件共享的狀態抽取出來，以一個全局的方式來管理，在這個方式下，我們的組件樹會變成一個巨大的 view，任何時候任何的組件可以存取 state 或是觸發 actions。

另外，通過定義和隔離狀態管理中的各種概念並強制遵守一定的規則，我們程式碼將會變得更結構化且易維護。

這就是 Vuex 背後的基本概念，借鑒了[Flux](https://facebook.github.io/flux/docs/overview.html)、[Redux](http://redux.js.org/)和[The Elm Architecture](https://guide.elm-lang.org/architecture/)

Vuex 架構圖：
![Vuex](vuex.png)

## 什麼時候需要使用 Vuex？

基本上我們在使用 Vue 時，使用 [global event bus](https://cn.vuejs.org/v2/guide/components.html#非父子组件通信)就足夠滿足開發的需求了，但如果今天要構建的是中大型的 SPA ，這樣就很可能需要如何好好的管理組件的外部 state，所以自然而然 Vuex 就會是最好的選擇。

## 參考資料

> [Vuex intro](https://vuex.vuejs.org/en/intro.html)
