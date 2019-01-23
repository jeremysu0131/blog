---
layout: posts
title: "[JS] Get Image Size Before Upload"
date: 2019-01-23 13:38:44
categories:
  - Javascript
tags:
  - JavaScript
  - upload
  - image
---

最近收到一個需求是要讓使用者在上傳前檢查圖片大小有沒有符合規定的尺寸，在查詢了一些資料後歸納出了以下的解決方法。

<!-- more -->

## Sync Version

在 \<input type="file" /\> 中傳入的檔案的型態是一個 file ，所以我們要用 `Image` 來讓使用者上傳的圖片可以被程式讀取。

```js
// file 是從 input 取得的 value
function getSize(file) {
  var _URL = window.URL || window.webkitURL; // 檢查各個瀏覽器的 method
  var img = new Image();
  img.onload = function() {
    console.log(this.height, this.width);
  };
  img.src = _URL.createObjectURL(file);
}
```

## Async Version

在上面的範例中，我們可以取得圖片的尺寸了，不過如果有很多地方要使用的話，我們必須把這個 funciton 封裝起來，但是 `img.onload` 這個方法是要在 `img.src = _URL.createObjectURL(file);` 這行程式碼執行後才能得到資料，所以我們必須將其封裝成**非同步**的方式來等待這個 function 執行完。

```js
function getSize(file) {
  return new Promise((resolve, reject) => {
    var _URL = window.URL || window.webkitURL;
    var img = new Image();

    img.onload = () => resolve({ height: img.height, width: img.width });
    img.onerror = reject;

    img.src = _URL.createObjectURL(file);
  });
}
```
