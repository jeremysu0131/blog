---
title: '[Vue.js] 父子組件溝通 - $emit / $on'
date: 2018-04-29 20:58:44
categories: Vue.js
tags:
- Vue
- Vue.js
- props
- components
- parent
- child
- communication
---

每次都記不起來要如何在子組件中呼叫父組件，這邊來做個備忘錄

<!-- more -->

父組件：

```html
<template>
  <div id="app">
    <child v-on:childMethod="parentMethod"></child>
  </div>
</template>

<script>
import Child from './components/Child';
export default {
  name: 'App',
  components: {
    Child,
  },
  methods: {
    parentMethod() {
      console.log('Hello World');
    },
  },
};
</script>
```

子組件：

```html
<template>
  <button @click="handleClick">Emit</button>
</template>

<script>
export default {
  methods: {
    handleClick() {
      this.$emit('childMethod');
    },
  },
};
</script>
```
