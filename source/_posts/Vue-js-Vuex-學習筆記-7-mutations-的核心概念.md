---
title: '[Vue.js] Vuex 學習筆記 (7) - mutations 的核心概念'
date: 2017-12-12 17:55:51
categories: Vue.js
tags:
- Vue
- Vue.js
- vue-cli
- Vuex
- state
- mutations
- mapMutations
---

# Mutations

**提交 mutations 是改變 Vuex 中 store 的唯一方式**。 mutations 非常類似於組件中的事件（event），每個 mutation 都有一個字串的  事件**類型** (type) 和一個**回調函數** (handler)， handler 就是我們實際進行狀態更改的地方，並且他會接受 state 作為第一個參數。

<!-- more -->

```js
const store = new Vuex.Store({
  state: {
    count: 1
  },
  mutations: {
    increment(state) {
      // 變更狀態
      state.count++;
    }
  }
});
```

我們不能  像調用 state 一樣調用 mutations ，而是要以 `store.commit` 的方法來做呼叫。

```js
store.commit('increment');
```

## 提交 PayLoad

我們可以在 `store.commit` 傳入額外的參數

```js
mutations: {
  increment (state, n) {
    state.count += n
  }
}
```

```js
store.commit('increment', 10)
```

在多數情況下， payload 應該是一個物件，這樣就可以包含更多的資料。

```js
mutations: {
  increment (state, payload) {
    state.count += payload.amount
  }
}
```

```js
store.commit('increment', {
  amount: 10
})
```

## 物件風格的 commit 方式

我們可以在物件中使用 `type` 來指定對應的 mutations。

 ```js
  store.commit({
    type: 'increment',
    amount: 10
  })
 ```

## Mutations 遵守 Vue 的響應規則

因為 Vuex 中的 store 的 state 是響應式的，那麼當我們變更 state 時，監控 state 的組件也會自動更新，這也意味著 Vuex 中的 mutations 也需要與使用 Vue 一樣遵守下列的注意事項：

1. 最好提前在 store 中初始化好所有所需的屬性。
2. 當新增新的屬性到物件時，你應該：
    * 使用 `Use Vue.set(obj, 'newProp', 123)`
    * 或是以新物件替換舊物件，例如我們可以利用`物件展開運算符`來寫：

```js
  state.obj = { ...state.obj, newProp: 123 }
```

## 使用常量提供 mutations 類型

在各種 Flux 實現中，使用常量用於 mutations 類型是一種常見的模式，這樣可以使 IDE 中的各式 linter 工具發揮作用，並將所有常量放在一個文件中，讓你的協作者可以快速瀏覽整個應用程序中可能發生的變化。

```js
// mutation-types.js
export const SOME_MUTATION = 'SOME_MUTATION'
// store.js
import Vuex from 'vuex'
import { SOME_MUTATION } from './mutation-types'

const store = new Vuex.Store({
  state: { ... },
  mutations: {
    // 我們可以使用 ES2015 風格的計算屬性命名功能來使用一個常量作為函數名
    [SOME_MUTATION] (state) {
      // mutate state
    }
  }
})
```

用不用常量取決於自己，但在需要多人協作的大型項目中使用會很有幫助。

## mutations 必須是同步函數

> **mutations 必須是同步函數**，這非常重要，我們用以下的例子來說明：

```js
mutations: {
  someMutation (state) {
    api.callAsyncMethod(() => {
      state.count++
    })
  }
}
```

假設我們正在 debug 一個 app 並且觀察 devtool 中的 mutation 日誌，當每一條 mutations 被記錄時，devtools 都需要捕捉到前一和後一個 state 的快照。
然而在上面的例子中， mutations 裡使用異步函數回調，devtools 不會知道什麼時候回調函數實際上被調用，因為當 mutations 被觸發的時候，回調函數還沒有被調用，因此這樣在任何回調函數中改變 state ，state 的狀態是無法追蹤的。

## 在組件中提交 mutations

我們可以在組件中使用 `this.$store.commit('xxx')` ，提交 mutations 或者使用 `mapMutations` 輔助函數將組件中的 methods 映射為 `store.commit` 來調用。

```js
export default {
  // ...
  methods: {
    ...mapMutations([
      'increment', // 將 `this.increment()` 映射為 `this.$store.commit('increment')`

      // `mapMutations` 也支援 payloads:
      'incrementBy' // 將 `this.incrementBy(amount)` 映射為 `this.$store.commit('incrementBy', amount)`
    ]),
    ...mapMutations({
      add: 'increment' // 將 `this.add()` 映射為 `this.$store.commit('increment')`
    })
  }
}
```

## 備註

在 mutation 中混合異步調用會導致你的程式很難被測試。例如，當你調用了兩個包含異步回調的 mutation 來改變狀態，你如何知道什麼時候回調和哪個先回調呢？這就是為什麼我們要區分這兩個概念。在 Vuex 中，mutation 都是同步事務。

```js
store.commit('increment')
// 任何由 "increment" 導致的狀態變更都應該在此刻完成。
```