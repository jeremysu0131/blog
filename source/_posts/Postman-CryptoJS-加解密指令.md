---
title: '[Postman] 使用 CryptoJS 來操作 MD5 或 Base64'
date: 2018-05-22 14:17:04
categories: Postman
tags:
- Postman
- CryptoJS
- Base64
- MD5
---

Postman 是一個很方便測試 API 的工具，但有時我們會需要使用 base64, MD5 等功能。
<!-- more -->

## Base64

```js
//Encode
var words = CryptoJS.enc.Utf8.parse('Hello CryptoJS');
var base64 = CryptoJS.enc.Base64.stringify(words);

//Decode
let parsedWords = CryptoJS.enc.Base64.parse(base64);
let parsedString = parsedWords.toString(CryptoJS.enc.Utf8);
```

## MD5

```js
// Encode
var hash = CryptoJS.MD5(str).toString();
```

## 參考資料

1.  [crypto-js](https://github.com/brix/crypto-js)
