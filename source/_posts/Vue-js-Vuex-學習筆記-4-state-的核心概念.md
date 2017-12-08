---
title: '[Vue.js] Vuex 學習筆記 (4) - state 的核心概念'
date: 2017-12-09 23:09:34
categories: Vue.js
tags:
- Vue
- Vue.js
- vue-cli
- Vuex
- state
- mapState
---

# State

## 單一狀態樹 (Single State Tree)

Vuex 使用單一狀態樹，這是一個物件包含了全部應用層的狀態與供應`單一數據來源` ([Single source of truth](https://en.wikipedia.org/wiki/Single_source_of_truth))，這代表通常你每個應用只會有一個 store 。單一狀態樹讓我們可以直接指定 state 其中的一項並讓我們能夠輕鬆的得到目前應用程式的狀態與快照 (snapshots)。

<!-- more -->

> 單一狀態樹與模組化並不會產生衝突

## 在組件中取得 Vuex 的 state

上一篇中我們已經知道利用 `store.state.count` 來取得 state 中的 count 值。

但先前的做法只能夠在當前宣告的檔案中使用，我們要怎麼讓所有組件來使用呢？還好 Vuex 提供了一個機制可以將 store 從根組件**注入**到每一個子組件中，我們將上一章的 src/main.js 再新增一行程式碼：

```js
new Vue({
  el: '#app',
  router,
  store, // 新增這一行
  template: '<App/>',
  components: {
    App,
  }
});
```

這樣我們任何的子組件就可以用下面的方式來取得 store 內的 state :

```js any component
export default {
  created() {
    console.log(this.$store.state.count);
  }
};
```

之後打開瀏覽器的 DevTools ，就可以看到 count 的值了。

但如果我們要如何更好的顯示狀態呢？ 用 `computed` 來將 state 的狀態存起來會是一個很棒的方式。

```html
<template>
  <div>{{count}}</div>
</template>

<script>
export default {
  computed: {
    count() {
      return this.$store.state.count;
    },
  },
};
</script>
```

執行上面的程式碼後，就可以在頁面上出現取到的值。

## mapState 輔助函數

當一個組件需要獲取多個 state 的時候，如果每次都要宣告為 computed 會很麻煩，為了解決這個問題 Vuex 讓我們可以使用 mapState 輔助函數來幫助我們，將繁瑣的流程簡化。

### 使用方式

```html
<template>
  <div>
    <div>{{count}}</div>
    <div>{{countAlias}}</div>
    <div>{{countPlusLocalState}}</div>
  </div>
</template>

<script>
import { mapState } from 'vuex';

export default {
  data() {
    return {
      localCount: 2,
    };
  },
  computed: mapState({

    // 箭頭函數可以使程式碼更簡潔
    count: state => state.count,

    // 直接傳送字串'count'，等同於 `state => state.count`
    countAlias: 'count',

    // 為了使用 `this` 來取得組件內的狀態，必須要使用下列特殊的格式
    countPlusLocalState(state) {
      return state.count + this.localCount;
    },
  }),
};
</script>
```

> Output：1, 1, 3

### 更簡單的使用方式：

```js
computed: mapState([
  // 將 this.count 映射為 store.state.count
  'count'
])
```

## 組件仍然可以有本地的狀態

使用 Vuex 並不代表要將所有狀態放入 Vuex ，如果有些狀態只有屬於某個組件，這樣的話我們就不需要將他放入 Vuex ，實際使用時我們還是要進行權衡和確認，不要把事情搞得太複雜。