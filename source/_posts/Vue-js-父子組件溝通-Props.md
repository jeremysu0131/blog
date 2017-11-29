---
title: '[Vue.js] 父子組件溝通 - Props'
date: 2017-11-29 16:50:01
categories: Vue.js
tags:
- Vue
- Vue.js
- props
- components
- parent
- child
- communication
---

# 使用 `props` 傳送數據

每一個 `component` 都作用域是各自獨立的，這也意味著不能（也不應該）在子組件內直接
引用父組件的數據。父組件的數據需要透過 `props` 才能下發到子組件中。

<!-- more -->

## 基本使用

``` html Parent
<template>
  <div>
    <child message='From parent'></child>
  </div>
</template>

<script>
import Child from '@/components/Child';

export default {
  components: {
    Child,
  },
  name: 'app',
};
</script>
```

```html Child
<template>
  <div>
    <span>Message: {{message}}</span>
  </div>
</template>

<script>
export default {
  props: {
    message: 'From child'
  },
};
</script>
```

## camelCase vs. kebab-case

若在js使用 camelCase 則html要使用 kebab-case

```html Parent
<template>
  <div>
    <child my-message='From parent'></child>
  </div>
</template>
```

``` js Child
export default {
  props: {
    myMessage: 'From child'
  },
};
```

## 動態 Props

與綁定到任何普通的HTML特性相似，我們可以使用 `v-bind` 來動態將 `props` 綁定到父組件的數據，這樣當父組件的數據變化時，該變化也會傳給子組件。

```html Parent - input的數據會傳給child
<div>
  <input v-model="parentMsg">
  <br>
  <child v-bind:my-message="parentMsg"></child>
</div>
```

## 單項數據流

`props` 是單項綁定的，當父組件的數據改變時，將其傳給子組件，但子組件不能傳給父組件，這是為了避免無意間修改了父組件的數據，避免數據流變得難以理解
> 不建議在子組件內改變 `props` ，因為每次父組件更新時，子組件的 `props` 會更新為最新值，若要改變使用記得另外宣告一個變量來記錄

## Props 驗證

我們可以指定組件的 `props` 驗證規則，如果傳入的數據不符合要求Vue會發出警告，這樣對於開發給他人使用組件時非常有用。

``` js Child
export default {
   props: {
    // 基礎型別檢測 (`null` 允許任何類型)
    propA: Number,
    // 可能是多種型別
    propB: [String, Number],
    // 必須要傳送的數據及`String`型別
    propC: {
      type: String,
      required: true
    },
    // 數據有預設值及`Number`型別
    propD: {
      type: Number,
      default: 100
    },
    // 陣列/物件的的預設值由function提供
    propE: {
      type: Object,
      default: function () {
        return { message: 'hello' }
      }
    },
    // 自定義驗證函數
    propF: {
      validator: function (value) {
        return value > 10
      }
    }
  }
};
```

## 注意事項

> 1. 在 JavaScript 中物件和陣列是引用類型，指向同一個記憶體空間，如果 `props` 是一個物件或陣列，在子組件內部改變它會影響父組件的狀態。
> 2. 注意 props 會在組件實例創建之前進行校驗，所以在 `default` 或 `validator` 函數里，諸如 `data` 、 `computed` 或 `methods` 等實例屬性還無法使用。

## 參考資料

* [Vue 官方手冊](https://vuejs.org/v2/guide/components.html#Props)