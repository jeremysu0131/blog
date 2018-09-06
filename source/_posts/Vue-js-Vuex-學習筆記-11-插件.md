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
  plugins: [myPlugin],
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

Plugin 與 Component 一樣不允許直接修改狀態，我們只能通過提交 mutation 來觸發 store 的變化。

### 範例

我們以 socket 的方式來模擬一個聊天室

```js
export default function createWebSocketPlugin(socket) {
  return store => {
    // 當 socket 收到新資料時
    // 假設有其他用戶發送新訊息
    socket.on('data', data => {
      // 同步數據到 store
      // 將新訊息放到 store 中
      store.commit('receiveData', data);
    });

    // 當我們做任何 store.commit 時，會觸發一下程式碼
    // 如發送訊息、回收訊息等
    store.subscribe(mutation => {
      // 當我們想取消一個已經發送的訊息時
      // 檢查如果是 store.commit('deleteMessage)，則觸發一下動作
      if (mutation.type === 'deleteMessage') {
        // 這是 socket 的行為，表示 socker 向後端發送出需要更新資料
        // 像後端發送需要刪除的訊息
        socket.emit('update', mutation.payload);
      }
    });
  };
}
```

```js
const plugin = createWebSocketPlugin(socket);

const store = new Vuex.Store({
  state: {
    data: {},
  },
  mutations: {
    updateData(data) {
      state.data = data;
    },
  },
  plugins: [plugin],
});
```
