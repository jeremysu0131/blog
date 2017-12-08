---
title: '[Vue.js] Vuex 學習筆記 (2) - Vuex 安裝方式'
date: 2017-12-07 21:43:23
categories: Vue.js
tags:
- Vue
- Vue.js
- vue-cli
- Vuex
---
以下列出幾種安裝 Vuex 的方式：
<!-- more -->

## 直接下載或是使用CDN

```html
<script src="/path/to/vue.js"></script>
<script src="/path/to/vuex.js"></script>
```

## NPM / Yarn

```bash
npm install vuex --save
```

```bash
yarn add vuex
```

使用vue-cli模組的話，安裝後記得使用 Vue.use() 來安裝 Vuex

```js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)
```
