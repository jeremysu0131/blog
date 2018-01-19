---
title: '[Vue.js] Vuex 學習筆記 (19) - 實用插件 plugin'
date: 2018-01-19 20:43:42
categories: Vue.js
tags:
- Vue
- Vue.js
- vue-cli
- Vuex
- plugin
---

# Plugin

前面有稍微提到如何使用 plugin ，不過我們有時候並不需要自己來建輪子， GitHub 上已經有很多人開發中出方便實用的工具，我們可以善加利用來減少開發的時間，下面列出我覺得比較實用的套件：

<!-- more -->

## 1. vuex-persistedstate

[vuex-persistedstate](https://github.com/robinvdvleuten/vuex-persistedstate) 是一個將 Vuex 的狀態保存在 localStorage 的套件。

## 2. vuex-i18n

[vuex-i18n](https://github.com/dkfbasel/vuex-i18n) 這是一個很好用的多語言套件，它可以將我們編輯好的多語系檔案儲存在 Vuex 中，方便系統取用。

## 3. vuex-loading

[vuex-loading](https://github.com/f/vuex-loading) 是一個可以管理複雜 loading 的套件，當我們有多個 state 需要在頁面多個地方使用及同時讀取及更新時，我們可以使用這個套件來做管理。

## 4. vuex-cache

[vuex-cache](https://github.com/superwf/vuex-cache) 這個套件可以將 action 執行完取得的結果暫存起來，這樣當下次我們需要調用這個套件時， vuex-cache 會直接到 cache 內取得先前的結果。
