---
title: '[Vue.js] Vuex 學習筆記 (9) - modules 的核心概念'
date: 2017-12-19 11:15:30
categories: Vue.js
tags:
- Vue
- Vue.js
- vue-cli
- Vuex
- modules
---

# Modules

由於使用單一狀態樹（single state tree)，應用的所有狀態會集中到一個比較大的物件，當應用變得非常複雜時， store 物件可能變得非常臃腫。

<!-- more -->

為了解決這樣的問題， Vuex 允許我們將 store 分割成模塊（module），每個模塊擁有自己的 state、mutation、action、getter，甚至我們可以嵌套子模塊：

```js
const moduleA = {
  state: { ... },
  mutations: { ... },
  actions: { ... },
  getters: { ... }
}

const moduleB = {
  state: { ... },
  mutations: { ... },
  actions: { ... }
}

const store = new Vuex.Store({
  modules: {
    a: moduleA,
    b: moduleB
  }
})

store.state.a // -> moduleA 的狀態
store.state.b // -> moduleB 的狀態
```

## 模塊的局部狀態

### mutation / getter

對於模塊內部的 mutation 和 getter，接受第一個參數是**此模塊的局部 state**。

```js
const moduleA = {
  state: { count: 0 },
  mutations: {
    increment(state) {
      // 這裡的 state 物件時模塊的局部狀態
      state.count++;
    }
  },

  getters: {
    doubleCount(state) {
      return state.count * 2;
    }
  }
};
```

模塊內部的 getter ，根節點狀態會作為第三個參數曝露出來：

```js
const moduleA = {
  // ...
  getters: {
    sumWithRootCount(state, getters, rootState) {
      return state.count + rootState.count;
    }
  }
};
```

### action

對於模塊內部的 action ，局部狀態通過 `context.state` 曝露出來，根節點狀態則為 `context.rootState`。

```js
const moduleA = {
  // ...
  actions: {
    incrementIfOddOnRootSum({ state, commit, rootState }) {
      if ((state.count + rootState.count) % 2 === 1) {
        commit("increment");
      }
    }
  }
};
```

## 命名空間

在前面的範例中，模塊內部的 action 、 mutation 與 getter 是註冊在全局命名空間，這樣使得多個模塊能夠對同一個 mutation 或是 action 做出響應。

如果我們希望模塊可以有更高的封裝度與重用性，我們可以通過新增 `namespaced: true` 的方式使其成為命名空間模塊。當模塊被註冊後，它所有的 getter 、 action 與 mutation 都會自動根據模塊註冊的路徑調整命名。

```js
const store = new Vuex.Store({
  modules: {
    account: {
      namespaced: true,

      // 模塊內容（module assets）
      state: { ... }, // 模塊內的 state 已經是嵌套的，且不被 `namespaced` 属性影響
      getters: {
        isAdmin () { ... } // -> getters['account/isAdmin']
      },
      actions: {
        login () { ... } // -> dispatch('account/login')
      },
      mutations: {
        login () { ... } // -> commit('account/login')
      },

      // 嵌套模塊
      modules: {
        // 繼承父模塊的 `namespaced`
        myPage: {
          state: { ... },
          getters: {
            profile () { ... } // -> getters['account/profile']
          }
        },

        // 進一步嵌套 `namespaced`
        posts: {
          namespaced: true,

          state: { ... },
          getters: {
            popular () { ... } // -> getters['account/posts/popular']
          }
        }
      }
    }
  }
})
```

啟用了命名空間的 getter 與 action 會收到局部化的 getter 、 dispatch 和 commit，這代表我們在使用模塊內容是不需要在同一模塊內而外添加 namespace 前綴，更改 namespace 屬性后不需要修改模塊內的代碼。

### 在命名空間模塊內訪問全局內容

如果你希望使用全局 state 和 getter ， rootState 和 rootGetter 會作為第三和第四個參數傳入 getter ，也會通過 context 物件的屬性傳入 action 。

若需要在全局 namespace 空間內分發 action 或提交 mutation ，將 `{ root: true }` 作為第三參數傳給 dispatch 或 commit 即可。

```js
modules: {
  foo: {
    namespaced: true,

    getters: {
      // 在這個模塊中， dispatch 和 commit 也被局部化了
      // 你可以使用 getter 的第四個參數來調用 `rootGetters`
      someGetter (state, getters, rootState, rootGetters) {
        getters.someOtherGetter // -> 'foo/someOtherGetter'
        rootGetters.someOtherGetter // -> 'someOtherGetter'
      },
      someOtherGetter: state => { ... }
    },

    actions: {
      // 在這個模塊中， dispatch 和 commit 也被局部化了
      // 他們可以接受 `root` 屬性以訪問根 dispatch 或 commit
      someAction ({ dispatch, commit, getters, rootGetters }) {
        getters.someGetter // -> 'foo/someGetter'
        rootGetters.someGetter // -> 'someGetter'

        dispatch('someOtherAction') // -> 'foo/someOtherAction'
        dispatch('someOtherAction', null, { root: true }) // -> 'someOtherAction'

        commit('someMutation') // -> 'foo/someMutation'
        commit('someMutation', null, { root: true }) // -> 'someMutation'
      },
      someOtherAction (ctx, payload) { ... }
    }
  }
}
```

### 帶命名空間的綁定函數

當使用 `mapState`, `mapGetters`, `mapActions` 和 `mapMutations` 這些函數來綁定命名空間模塊時，寫起來可能比較繁瑣：

```js
computed: {
  ...mapState({
    a: state => state.some.nested.module.a,
    b: state => state.some.nested.module.b
  })
},
methods: {
  ...mapActions([
    'some/nested/module/foo',
    'some/nested/module/bar'
  ])
}
```

為了簡化這樣的情況，我們可以將模塊的空間名稱字串作為第一個參數傳遞給上述函數：

```js
computed: {
  ...mapState('some/nested/module', {
    a: state => state.a,
    b: state => state.b
  })
},
methods: {
  ...mapActions('some/nested/module', [
    'foo',
    'bar'
  ])
}
```

此外，我們可以通過使用 `createNamespacedHelpers` 建立命名空間 helpers：

```js
import { createNamespacedHelpers } from "vuex";

const { mapState, mapActions } = createNamespacedHelpers("some/nested/module");

export default {
  computed: {
    // look up in `some/nested/module`
    ...mapState({
      a: state => state.a,
      b: state => state.b
    })
  },
  methods: {
    // look up in `some/nested/module`
    ...mapActions(["foo", "bar"])
  }
};
```

模塊的概念這邊看完可能會比較不清楚到底如何使用，未來有時間我會再新增一些範例來讓大家更清楚明白，礙於最近案子比較忙，未來有時間一定會把它寫的更清楚。
