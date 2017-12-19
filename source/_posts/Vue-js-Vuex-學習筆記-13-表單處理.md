---
title: '[Vue.js] Vuex 學習筆記 (13) - 表單處理'
date: 2017-12-19 23:35:05
categories: Vue.js
tags:
- Vue
- Vue.js
- vue-cli
- Vuex
- strict
- strict model
- form
- v-model
- setter
---

# 表單處理

當在嚴格模式中使用 Vuex 時，state 使用在 `v-model` 時會遇到問題：

<!-- more -->

```html
<input v-model="obj.message">
```

假設 `obj` 是在 computed 屬性中返回一個屬於 Vuex store 的物件，當用戶輸入時 `v-model` 會試圖直接修改 `obj.message`，但是在嚴格模式中，由於這個修改不是在 mutation 函數中執行的，所以會拋出一個錯誤。

我們要解決這個問題，需要用 **Vuex 的邏輯** 來處理，在 `<input>` 中綁定 value ， 然後監聽 `input` 或 `change` 事件，並且事件中調用 action 。

```html
<template>
  <input :value="message" @input="updateMessage">
</template>
<script>
  // ...
  computed: {
    ...mapState({
      message: state => state.obj.message
    })
  },
  methods: {
    updateMessage (e) {
      this.$store.commit('updateMessage', e.target.value)
    }
  }
</script>
```

在 store 中的 mutation：

```js
// ...
mutations: {
  updateMessage (state, message) {
    state.obj.message = message
  }
}
```

## 雙向綁定的計算屬性

上面的作法比起簡單地使用 `v-model + 局部狀態` 要麻煩許多，並且也損失了一些 `v-model` 中很有用的特性。還好，我們可以用另一種方式，使用帶有 setter 的雙向綁定計算屬性：

```html
<template>
  <input v-model="message">
</template>

<script>
// ...
computed: {
  message: {
    get () {
      return this.$store.state.obj.message
    },
    set (value) {
      this.$store.commit('updateMessage', value)
    }
  }
}
</script>
```
