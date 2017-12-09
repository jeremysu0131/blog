---
title: '[Vue.js] Vuex 學習筆記 (6) - mapState 與 mapGetters 合併使用'
date: 2017-12-11 09:42:23
categories: Vue.js
tags:
- Vue
- Vue.js
- vue-cli
- Vuex
- state
- getters
- mapState
- mapGetters
---


## 合併使用

我們在實際開發時， computed 內容並不會像前兩章的範例那樣簡單，有時我們需要將本地組件的 computed 或是 mapState 與 mapGetters 混合使用。通常我們要使用一個工具函數將多個物件合併為一個，以使我們可以將最終的物件傳給 computed 屬性，但自從有了**物件展開運算符**，我們就可以將寫法大大的簡化。

<!-- more -->

> mapState 與 mapGetters 函數返回的是一個物件

## 物件展開運算符 (Object Spread Operator)

使用物件展開運算符之前我們要先知道[展開運算符 (Spread syntax)](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Operators/Spread_operator)，了解後再看下面的程式碼就會很清楚的知道使用方式。

```js Vuex
const store = new Vuex.Store({
  state: {
    count: 0,
    todos: [{
        id: 1,
        text: '...',
        done: true
      },
      {
        id: 2,
        text: '...',
        done: false
      }
    ]
  },
  getters: {
    doneTodos: state => {
      return state.todos.filter(todo => todo.done)
    },
    doneTodosCount: (state,getters) => {
      return getters.doneTodos.length
    },
    getTodoById: (state) => (id) => {
      return state.todos.find(todo => todo.id === id)
    }
  }
});
```

```html component
<template>
  <div>
    <div>{{count}}</div>
    <div>{{countAlias}}</div>
    <div>{{countPlusLocalState}}</div>
    <div>{{doneTodos}}</div>
    <div>{{doneTodosAlias}}</div>
    <div>{{doneTodosCount}}</div>
    <div>{{getTodoById}}</div>
  </div>
</template>

<script>
import { mapState, mapGetters } from 'vuex';

export default {
  data() {
    return {
      localCount: 2,
    };
  },
  computed: {

    // 本地 computed
    getTodoById() {
      return this.$store.getters.getTodoById(2);
    },

    // 使用展開運算符將 mapState 混合到外部物件中
    ...mapState([
      'count',
    ]),
    ...mapState({
      countAlias: 'count',
      countPlusLocalState(state) {
        return state.count + this.localCount;
      },
    }),

    // 使用展開運算符將 mapGetters 混合到外部物件中
    ...mapGetters([
      'doneTodos',
      'doneTodosCount',
    ]),
    ...mapGetters({
      doneTodosAlias: 'doneTodos',
    }),
  },
};
</script>
```