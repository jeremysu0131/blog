---
layout: posts
title: '[JS] 檢查使用者輸入的任何可能為空的結果'
date: 2019-05-17 18:33:18
categories:
  - Javascript
tags:
  - JavaScript
  - string
  - null
  - empty
  - undefined
---

在檢查使用者輸入資料時，常常需要檢查字串是否為空或 null, undefined 等，於是搜尋並整合了許多人的解決方法，可以完全解決這個問題。

<!-- more -->

## Solution

```js
function isEmpty(str) {
    return (!str || str.trim().length === 0);
}

```

1. 我們先用 `!str` 來檢查輸入結果是否為 null, undefined
2. 再來我們用 `str.trim().length === 0` 來解決輸入為 `''` 或 `' '` 的結果
