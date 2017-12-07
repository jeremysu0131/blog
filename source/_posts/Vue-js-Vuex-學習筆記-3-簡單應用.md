---
title: '[Vue.js] Vuex 學習筆記 (3) - 簡單應用'
categories: Vue.js
tags:
- Vue
- Vue.js
- vue-cli
- Vuex
---

當我們用 vue-cli 建立好專案後，我們在 src/main.js 內修改程式碼：

<!-- more -->

```js
import Vue from 'vue';
import Vuex from 'vuex';
import App from './App';
import router from './router';

Vue.config.productionTip = false;

Vue.use(Vuex);

//建立store
const store = new Vuex.Store({
  state: {
    count: 0,
  },
  mutations: {
    increment(state) {
      state.count++;
    },
  },
});

new Vue({
  el: '#app',
  router,
  template: '<App/>',
  components: {
    App,
  },
  created() {
    // 使用 store.state 來取得state的物件
    console.log(store.state.count); // 0
    // 使用 store.commit 觸發 mutation 來改變 state
    store.commit('increment');
    console.log(store.state.count); // 1
  }
});
```

建立好後打開瀏覽器的DevTools，可以在 Console中查看到數據的變化，這樣一個簡單的 Vuex 應用就實現了

![result](https://lh6.googleusercontent.com/WF_q6ifP6JYe47MxTAn6Q-jWIi6AEa4qBlTjME0a2rrDKjHTsXq3wZPH7_6T1PUwk1culhL-77JAJWu__D_k=w1368-h664)

* **store** ：是組件中的共享狀態，而改變 state 的方法是 `mutations`
* **mutation** ：我們只能通過 mutation 來改變 state 的狀態，不能使用 `store.state.count = 5` 這樣的方式來修改。
> 雖然這樣的方式仍然可以修改，但非常不建議這樣使用，不通過 mutation 來改變 state ，狀態不會被同步。

由於 store 中的 state 是響應式的，在組件中調用 store 中的狀態僅需要在 computed 屬性中返回即可。觸發變化也僅僅是在組件的 methods 中提交 mutation。

這邊 Vue 官方提供了一個[計數範例](https://jsfiddle.net/n9jmu5v7/1269/)，參考一下就可以知道 Vuex 的運作方式。