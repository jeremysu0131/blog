---
title: '[Vue.js] 使用 babel-polyfill 讓 IE 正常運作'
date: 2018-04-24 16:19:39
categories: Vue.js
tags:
- Vue
- Vue.js
- vue-cli
- Webpack
- babel-polyfill
- axios
---

IE 是許多前端工程師的夢靨，在 Vue 中我們會使用 axios 來存取 api ，但是 axios 會使用到 ES6 的 Promise ，所以 IE 會沒有辦法支援，因此我們要用 babel-polyfill 來將 ES6 的語法轉到 ES5。

<!-- more -->

# 安裝 babel-polyfill

## 注意

因爲 polyfill 要在編譯你的程式碼之前執行，所以要安裝為 `dependency` 而不是 `devDependency`

```bash
npm install --save babel-polyfill #yarn add babel-polyfill
```

# 在 Webpack 中設定

開啟 `./build/webpack.base.conf.js`，在下面的 `entry` 中加入一行程式碼 `polyfill: 'babel-polyfill'`

```js
 entry: {
    app: './src/main.js',
  },
```

更改如下：

```js
entry: ['babel-polyfill', './src/main.js'];
```

這樣就可以在 IE 中使用 axios 了！
