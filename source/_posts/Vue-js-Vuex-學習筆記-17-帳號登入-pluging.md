---
title: '[Vue.js] Vuex 學習筆記 (17) - 帳號登入 plugin'
date: 2018-01-10 23:21:22
categories: Vue.js
tags:
- Vue
- Vue.js
- vue-cli
- Vuex
- login
- plugin
---

# 前言

使用 Vuex 時我們一定會想到説可以應用在帳號登入，紀錄一些使用者的資訊，這邊我們來試著做一個帳號登入的功能。

<!-- more -->

## Login Page

首先建立一個登入的頁面：

```html
<template>
  <div>
    <h2>Login</h2>
    <div>
      <div>
        <input type="text"
               id="account"
               placeholder="Account"
               v-model="user.account">
      </div>
      <div>
        <input type="password"
               id="password"
               placeholder="Password"
               v-model="user.password">
      </div>
      <div>
        <button @click="login">Login</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      user: {
        account: '',
        password: '',
      },
    };
  },
  methods: {
    login() {
      this.$store.commit({
        type: 'setUserData',
        userData: this.user,
      });
      this.$router.push('/');
    },
  },
};
</script>
```

Vuex store 的設定：

```js
const store = new Vuex.Store({
  state: {
    user: {
      account: "",
      password: "",
      isLogin: false
    }
  },
  mutations: {
    setUserData(state, { userData }) {
      state.user.account = userData.account;
      state.user.password = userData.password;
      state.user.isLogin = true;
    }
  }
});
```

再來建立一個登入成功後取得 User Data 的畫面：

```html
<template>
  <div>
    <h2>Login Success</h2>
    <p>Hi,
      <strong>{{user.account}}</strong> !</p>
  </div>
</template>

<script>
export default {
  created() {
    if (!this.user.account) {
      this.$router.push('login');
    }
  },
  computed: {
    user() {
      return this.$store.state.user;
    },
  },
};
</script>
```

這樣一個簡單的登入功能就完成了，實際情況如下：

登入：
![Login](https://jeremysu0131.github.io/Vue-js-Vuex-%E5%AD%B8%E7%BF%92%E7%AD%86%E8%A8%98-17-%E5%B8%B3%E8%99%9F%E7%99%BB%E5%85%A5-pluging/login.png)

登入成功畫面：
![Login Success](https://jeremysu0131.github.io/Vue-js-Vuex-%E5%AD%B8%E7%BF%92%E7%AD%86%E8%A8%98-17-%E5%B8%B3%E8%99%9F%E7%99%BB%E5%85%A5-pluging/login-success.png)

## Plugins

雖然登入的功能看似很順利，但如果我們重新整理網頁來看一下，是不是回到登入畫面了，因為 Vuex 紀錄的是當前網頁的狀態，如果重新整理網頁或是開新分頁，則狀態不會被同步。要解決這個方法，我們可以使用 Vuex 中的 `plugins` 來與 `localStorage` 結合將狀態保存在瀏覽器。接下來我們在 Vuex store 上面加入一下程式碼：

```js
const localStoragePlugin = store => {
  store.subscribe((mutation, { user }) => {
    // 當執行 setUserData 時才執行以下程式碼
    if (mutation.type === "setUserData") {
      window.localStorage.setItem("user", JSON.stringify(user));
    }
  });
};

const store = new Vuex.Store({
  // ...

  //將 localStoragePlugin 引用進來
  plugins: [localStoragePlugin]
});
```

前面有提到， `plugins` 可以監控 mutaion 的行為，所以當我們執行 `setUserData` 這個功能時，將使用者的資料轉換成物件並儲存到 Local Storage 中。

登入成功畫面修改：

```js
export default {
  created() {
    if (!this.user.account) {
      this.$router.push("login");
    }
  },
  computed: {
    user() {
      // 這裡將取得方式改變成從 local storage 中取得使用者資訊
      const userData = JSON.parse(localStorage.getItem("user"));
      if (userData) {
        return userData;
      }
      return "";
    }
  }
};
```

設定好後，我們再次登入，到登入成功的畫面後再試著從新整理看看，會發現瀏覽器已經講使用者訊息記錄起來了。

![localstorage](https://jeremysu0131.github.io/Vue-js-Vuex-%E5%AD%B8%E7%BF%92%E7%AD%86%E8%A8%98-17-%E5%B8%B3%E8%99%9F%E7%99%BB%E5%85%A5-pluging/localstorage.png)

> 備註：以上範例是一個簡單展示的登入功能，所以並沒有在安全性上做任何設定，要注意儲存在瀏覽器中的資料不能是機敏資訊，否則會有安全性上的問題。
