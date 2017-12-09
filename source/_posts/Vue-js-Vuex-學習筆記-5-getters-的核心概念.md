---
title: '[Vue.js] Vuex 學習筆記 (5) - getters 的核心概念'
date: 2017-12-10 08:59:57
categories: Vue.js
tags:
- Vue
- Vue.js
- vue-cli
- Vuex
- getters
- mapGetters
---

# Getter

## 為什麼需要使用 getters ？

在某些時候我們需要 `computed` store` 中的 state ，例如在 to do list 內取得完成的數量。

<!-- more -->

```js
computed: {
  doneTodosCount () {
    return this.$store.state.todos.filter(todo => todo.done).length
  }
}
```

但是如果有多個組件要使用相同功能呢？我們要複製上面的程式碼到每個組件內嗎？還是要將他抽取出來成為一個 helper ？其實這兩個做法都不太理想。

## getters 使用方式

還好， Vuex 允許我們在 store 中定義 `getters` ，`getters` 就類似組件中的 `computed` ， `getters` 的返回值會根據他依賴的關係被緩存起來，只有當他依賴的值發生改變才會重新被計算。

### 使用 state

getters 可以將 state 作為第一個參數

```js store
const store = new Vuex.Store({
  state: {
    todos: [
      { id: 1, text: '...', done: true },
      { id: 2, text: '...', done: false }
    ]
  },
  getters: {
    doneTodos: state => {
      return state.todos.filter(todo => todo.done)
    }
  }
})
```

我們在組件中即可以下面方式來取得 doneTodos ：

```js component
computed: {
  doneTodos() {
    return this.$store.getters.doneTodos;
  },
}

// [ { "id": 1, "text": "...", "done": true } ]
```

### 使用其他 getters

getters 可以將其他 getters 作為第二個參數

```js store
const store = new Vuex.Store({
  state: {
    todos: [
      { id: 1, text: '...', done: true },
      { id: 2, text: '...', done: false }
    ]
  },
  getters: {
    doneTodos: state => {
      return state.todos.filter(todo => todo.done);
    }
    doneTodosCount: (state, getters) => {
      return getters.doneTodos.length;
    }
  }
})
```

在組件中可以用下面方式來取得 doneTodosCount ：

```js component
computed: {
  doneTodosCount () {
    return this.$store.getters.doneTodosCount
  }
}

// 1
```

### 返回函數

我們也可以傳參數到 getters 來取得返回結果，這是非常便利的方式查詢 store 中的陣列。

```js store
const store = new Vuex.Store({
  state: {
    todos: [
      { id: 1, text: '...', done: true },
      { id: 2, text: '...', done: false }
    ]
  },
  getters: {
    getTodoById: (state) => (id) => {
      return state.todos.find(todo => todo.id === id)
    }
  }
})
```

```js component
computed: {
  getTodoById() {
    return this.$store.getters.getTodoById(2);
  }
}

// { "id": 2, "text": "...", "done": false }
```

## magGetters 輔助函數

`mapGetters` 輔助函數與前面提到的 `mapState` 用法相近，可以簡化程式碼。

```html
<template>
  <div>
    <div>{{doneTodos}}</div>
    <div>{{doneTodosCount}}</div>
  </div>
</template>

<script>
import { mapGetters } from 'vuex';

export default {
  computed: {
    mapGetters([
      'doneTodos',
      'doneTodosCount',
    ]),
  },
};
</script>
```

> Output: [ { "id": 1, "text": "...", "done": true } ], 1

如果我們要將 getters 屬性取另外一個名稱，可以用物件的方式：

> 新名稱: 'getters 屬性名稱'

```js
computed: mapGetters({
  // 映射 `this.doneCount` 為 `store.getters.doneTodosCount`
  doneCount: 'doneTodosCount'
}),
```