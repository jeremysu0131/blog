---
title: '[Vue.js] watch 物件屬性'
date: 2017-12-28 13:58:33
categories: Vue.js
tags:
- Vue
- Vue.js
- watch
- object
---

# Watch

使用 watch 是用來監控指定的資料變化時，觸發相對應的行為，要監控一般的參數很簡單，但是要監控物件裡面的屬性呢？以下是解決的方式：

<!-- more -->

```js
export default {
  data() {
    return {
      form: {
        type: 'document'
      }
    };
  },
  watch: {
    //當 form 裡面的 type 改變時則會觸發
    'form.type': function () {
      // Do anything ...
      this.doSomething();
      console.log('change');
    }
  }
};
```

> 在這個程式碼中，我們不能將 `'form.type': function () {}` 這個函數改成箭頭函數 `'form.type' () => {}` ，因為箭頭函數會綁定 `parent` 的內容，這樣會導致 `this.doSomething()` 這行程式碼出現錯誤。
