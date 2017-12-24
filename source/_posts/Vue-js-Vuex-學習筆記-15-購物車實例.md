---
title: '[Vue.js] Vuex 學習筆記 (15) - 購物車實例'
date: 2017-12-24 13:13:02
categories:
tags:
---

## Mock API

在 api/shop.js 的文件中我們可以看到模擬的 API ，要注意在 buyProducts 這個函數中，因為要模擬失敗或是成功的結果，所以會用亂數來決定結果。

購買成功時則清空商品，購買失敗時則保留商品。

```js
/**
 * Mocking client-server processing
 */
const _products = [
  { id: 1, title: "iPad 4 Mini", price: 500.01, inventory: 2 },
  { id: 2, title: "H&M T-Shirt White", price: 10.99, inventory: 10 },
  { id: 3, title: "Charli XCX - Sucker CD", price: 19.99, inventory: 5 }
];

export default {
  getProducts(cb) {
    setTimeout(() => cb(_products), 100);
  },

  buyProducts(products, cb, errorCb) {
    setTimeout(() => {
      // simulate random checkout failure.
      Math.random() > 0.5 || navigator.userAgent.indexOf("PhantomJS") > -1
        ? cb()
        : errorCb();
    }, 100);
  }
};
```
