---
title: '[axios] 處理 x-www-form-urlencoded 格式問題'
date: 2018-11-26 10:46:32
categories:
  - frontend
tags:
  - axios
  - json
---

# axios

axios 是基於 Promise 的瀏覽器與 node.js 的客戶端。使用的方式非常簡單清楚，安裝與使用[官方網站](https://github.com/axios/axios)都寫的非常詳細。

<!-- more -->

## x-www-form-urlencoded

這是 HTTP POST 中很常見的提交數據的方式，通過 `&`與`=`來分離 key 與 value，瀏覽器的原生`<form>`表單如果不設置 `enctype` 屬性，那麼就會以`x-www-form-urlencoded` 來提交數據，範例如下：

```http
POST http://www.example.com HTTP/1.1
Content-Type: application/x-www-form-urlencoded;charset=utf-8

firstName=Jeremy&lastName=&age=18
```

但 `axios` 輸出的數據是 `json` 格式，若我們要轉換成 `x-www-form-urlencoded` 格式，則需要安裝 [**`qs`**](https://github.com/ljharb/qs) 這個額外套件。

## 使用方式（這邊以 Vue.js 來做為安裝範例，使用方式都是一樣的）

1.安裝套件

```bash
npm install qs
```

2. 引用套件

```js
import Qs from 'qs';

Vue.prototype.Qs = Qs;
```

3. 使用方式

```js
const data = { firstName: 'Jeremy', lastName: '', age: '18' };
console.log(this.Qs.stringify(data));
//firstName=Jeremy&lastName=&age=18
```

## 處理 JSON 中的物件

上面的方式使用起來很簡單，只要通過一個轉換的方式就可以將數據提交出去，但是如果我們要提交的 json 是像下面這樣呢？

```js
const data = {
  person: { firstName: 'Jeremy', lastName: '', age: '18' },
};
```

### 錯誤方式

我們會以為一樣轉換就可以產生我們要的格式，但是這是不對的

```js
console.log(this.Qs.stringify(data));
//person%5BfirstName%5D=Jeremy&person%5BlastName%5D=&person%5Bage%5D=18
```

如上面 `x-www-form-urlencoded` 的格式介紹， key 與 value 要使用 `&` 或是 `=` 來區隔，我們來看看上面轉換出的格式會發現 person 後面並沒有正常顯示。

### 正確方式

我們要先將 `JSON` 的物件轉換成 `string` 再做轉換，這樣才是正確的轉換方式:

```js
const data = {
  person: { firstName: 'Jeremy', lastName: '', age: '18' },
};
console.log(
  this.Qs.stringify({
    person: JSON.stringify(data.person),
  })
);
//person=%7B%22firstName%22%3A%22Jeremy%22%2C%22lastName%22%3A%22%22%2C%22age%22%3A%2218%22%7D
```
