---
title: '[Vue.js] Vuex 學習筆記 (14) - 購物車實例 component'
date: 2017-12-24 12:23:21
categories: Vue.js
tags: 
- Vue
- Vue.js
- vue-cli
- Vuex
- shopping cart
- component
---

# 前言

前面我們已經把 Vuex 的特性都解說完畢，應該對 Vuex 有初步的了解，但是如果沒有實作我們還是沒有辦法很熟悉，官方這邊提供了一個購物車的[範例](https://github.com/vuejs/vuex/tree/dev/examples/shopping-cart)，我們就以此來看看要如何將 Vuex 應用在購物車中。

<!-- more -->

## 組件

範例中共有 3 個組件分別是：

1. APP.vue - 作為 layout 導入 Cart 與 ProductList 組件。
2. ProductList.vue - 商品清單。
3. Cart.vue - 購物車，呈現新增到購物車的商品。

### ProductList

```html
<template>
  <ul>
    <li v-for="p in products">
      {{ p.title }} - {{ p.price | currency }}
      <br>
      <button
        :disabled="!p.inventory"
        @click="addToCart(p)">
        Add to cart
      </button>
    </li>
  </ul>
</template>

<script>
import { mapGetters, mapActions } from 'vuex'

export default {
  computed: mapGetters({
    // 在 store 中，取得全部商品
    products: 'allProducts'
  }),
  methods: mapActions([
    // 使用 action 將選定的商品新增到購物車，並且將庫存減1
    'addToCart'
  ]),
  created () {
    // 呼叫取得商品的 action
    this.$store.dispatch('getAllProducts')
  }
}
</script>
```

### Cart

```html
<template>
  <div class="cart">
    <h2>Your Cart</h2>
    <p v-show="!products.length"><i>Please add some products to cart.</i></p>
    <ul>
      <li v-for="p in products">
        {{ p.title }} - {{ p.price | currency }} x {{ p.quantity }}
      </li>
    </ul>
    <p>Total: {{ total | currency }}</p>
    <p><button :disabled="!products.length" @click="checkout(products)">Checkout</button></p>
    <p v-show="checkoutStatus">Checkout {{ checkoutStatus }}.</p>
  </div>
</template>

<script>
import { mapGetters } from 'vuex'

export default {
  computed: {
    // 使用 mapGetters 簡化程式碼並取得資料
    ...mapGetters({
      products: 'cartProducts',
      checkoutStatus: 'checkoutStatus'
    }),
    // 將取得的商品金額總計
    total () {
      return this.products.reduce((total, p) => {
        return total + p.price * p.quantity
      }, 0)
    }
  },
  methods: {
    // 購買
    checkout (products) {
      this.$store.dispatch('checkout', products)
    }
  }
}
</script>
```
