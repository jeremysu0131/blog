---
title: '[Vue.js] Vuex 學習筆記 (11) - 插件'
date: 2017-12-19 23:34:29
categories: Vue.js
tags:
- Vue
- Vue.js
- vue-cli
- Vuex
- plugins
---

# 插件（Plugin）

Vuex 的 store 接受 `plugins` 選項，這個選項露出每個 mutation 的鉤子 (hook)， Vuex 插件就是一個函數，他接收 store 作為唯一參數，我們用以下範例來解釋。

<!-- more -->

使用插件的方式：

```js
const store = new Vuex.Store({
  // ...
  plugins: [myPlugin]
});
```

插件的內容：

```js
const myPlugin = store => {
  // 當 store 初始化後調用
  store.subscribe((mutation, state) => {
    // 每次 mutation 之後調用
    // mutation 的格式為 { type, payload }
  });
};
```

## 在插件內提交 mutation

在插件中不允許直接修改狀態，這類似於組件，只能通過提交 mutation 來觸發變化。

通過提交 mutation ，插件可以用來同步數據到 store ，例如同步 websocket 數據到 store。

```js
export default function createWebSocketPlugin(socket) {
  return store => {
    socket.on("data", data => {
      // 同步數據到 store
      store.commit("receiveData", data);
    });
    store.subscribe(mutation => {
      if (mutation.type === "UPDATE_DATA") {
        socket.emit("update", mutation.payload);
      }
    });
  };
}
```

```js
const plugin = createWebSocketPlugin(socket);

const store = new Vuex.Store({
  state,
  mutations,
  plugins: [plugin]
});
```
