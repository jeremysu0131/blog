---
title: '[Vue.js] Vuex 學習筆記 (8) - actions 的核心概念'
date: 2017-12-13 12:00:58
categories: Vue.js
tags:
- Vue
- Vue.js
- vue-cli
- Vuex
- state
- mutations
- actions
- mapActions
- async
- await
---

# Actions

Actions 類似於 mutations，但仍然有些不同：

* Action 提交（commit）的是 mutations ，而不是直接變更 state。
* Actions 可以包含任意異步操作。

## 使用 Actions

我們這邊來註冊一個簡單的 action

```js
const store = new Vuex.Store({
  state: {
    count: 0,
  },
  mutations: {
    increment(state) {
      state.count++;
    },
  },
  actions: {
    increment (context) {
      context.commit('increment')
    },
  },
});
```

Action 函數接受一個與 store 實例具有相同方法和屬性的 `context` 物件，因此你可以調用 `context.commit` 提交一個 mutation，或是透過 `context.state` 或 `context.getters` 來取得 state 和 getters 。
下一章我們介紹 Modules 時，就會對 context 有更多介紹，到時就會知道 context 物件為什麼不是 store 實例本身了。

## 簡化程式碼

在實際情況中，我們常會使用 ES2015 中的 [**參數解構**](https://github.com/lukehoban/es6features#destructuring) 來簡化程式碼：

```js
actions: {
  increment ({ commit }) {
    commit('increment')
  }
}
```

## 調用 Actions

Action 透過 `store.dispatch` 方法觸發：

```js
store.dispatch('increment')
```

這樣看起來跟 mutations 的用法差不多，但是我們要記得 **mutations 必須是同步執行**，而 actions 則可以**異步執行**。

```js
actions: {
  incrementAsync ({ commit }) {
    setTimeout(() => {
      commit('increment')
    }, 1000)
  }
}
```

## Payload

actions 同樣可以使用 payload 或物件的形式：

```js
// 以 payload 形式分發
store.dispatch('incrementAsync', {
  amount: 10
})

// 以物件形式分發
store.dispatch({
  type: 'incrementAsync',
  amount: 10
})
```

我們用一個更加實際的例子來**調用異步API**與**分發多重 mutations**：

```js
actions: {
  checkout ({ commit, state }, products) {
    // 把當前購物車的物品備份起來
    const savedCartItems = [...state.cart.added]
    // 發出結賬請求，然後清空購物車
    commit(types.CHECKOUT_REQUEST)
    // 購物API，接受一個成功和失敗回調
    shop.buyProducts(
      products,
      // 成功操作
      () => commit(types.CHECKOUT_SUCCESS),
      // 失敗操作
      () => commit(types.CHECKOUT_FAILURE, savedCartItems)
    )
  }
}
```

上面的例子我們正在進行一系列的異步操作，並透過提交 mutations 來記錄 actions 產生的轉態變更。

## mapActions

我們可以在組件中使用 `this.$store.dispatch('xxx')` 分發 action，或者 使用 `mapActions` 輔助函數將組件的 methods 映射為 `store.dispatch` 調用。

```js
import { mapActions } from 'vuex'

export default {
  // ...
  methods: {
    ...mapActions([
      'increment', // 將 `this.increment()` 映射為 `this.$store.dispatch('increment')`

      // `mapActions` 也支持载荷：
      'incrementBy' // 將 `this.incrementBy(amount)` 映射為 `this.$store.dispatch('incrementBy', amount)`
    ]),
    ...mapActions({
      add: 'increment' // 將 `this.add()` 映射為 `this.$store.dispatch('increment')`
    })
  }
}
```

## 組合 actions

actions 通常是異步的，那我們如何知道 Action 什麼時候結束呢？更重要的是，我們如何才能組合多個 action ，來處理更加複雜的異步流程？

首先我們需要先明白，當一個 action 返回 **`Promise`** 時，`store.dispatch` 可以處理返回（return）的 **`Promise`** ：

```js
actions: {
  actionA ({ commit }) {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        commit('someMutation')
        resolve()
      }, 1000)
    })
  }
}
```

```js
store.dispatch('actionA').then(() => {
  // ...
})
```

在其他 action 中也可以這樣使用：

```js
actions: {
  // ...
  actionB ({ dispatch, commit }) {
    return dispatch('actionA').then(() => {
      commit('someOtherMutation')
    })
  }
}
```

## 在 actions 中使用 async / await

```js
// 假設 getData() 和 getOtherData() 返回的是 Promise

actions: {
  async actionA ({ commit }) {
    commit('gotData', await getData())
  },
  async actionB ({ dispatch, commit }) {
    await dispatch('actionA') // 等待 actionA 完成
    commit('gotOtherData', await getOtherData())
  }
}
```

> 一個 store.dispatch 在不同模塊中可以觸發多個 action 函數。在這種情況下，只有當所有觸發函數完成後，返回的 Promise 才會執行。