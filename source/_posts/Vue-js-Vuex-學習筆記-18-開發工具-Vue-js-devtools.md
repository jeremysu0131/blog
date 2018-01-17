---
title: '[Vue.js] Vuex 學習筆記 (18) - 開發工具 Vue.js devtools'
date: 2018-01-17 17:15:41
categories: Vue.js
tags:
- Vue
- Vue.js
- vue-cli
- Vuex
- devTool
---

# Vue.js devtools

**Vue.js devtools** 是一個在開發時很實用的 Chrome 套件，我們可以在裡面檢查組件的資料，也可以查看 Vuex 的事件與資料變化。
<!-- more -->
![preview](Vuejs-devtools.PNG)

## 安裝

使用方式也很簡單，我們使用 Chrome 開啟下面鏈接後安裝完即可使用
[下載位置](https://chrome.google.com/webstore/detail/vuejs-devtools/nhdogjmejiglipccpnnnanhbledajbpd)

## 在 Vuex 中使用

我們可以開啟 Chrome 的 Developer Tools 之後就可以在最上面的選單列看到 `Vue` 這個欄位，點擊後就可以使用了。

1. Component 中我們可以查看組件的參數在操作中的變化，省去使用 console 的時間。
2.  Vuex 中的功能更是強大，每一個 mutation 呼叫時都會被記錄在其中，而點選其中的 mutation 時，還會顯示當下的數據，所以當要查看資料的變化時，是一個非常實用的功能。

![demo](https://raw.githubusercontent.com/vuejs/vue-devtools/master/media/demo.gif)