---
title: '[Vue.js] ESLint 忽略錯誤方式'
date: 2017-12-04 16:45:38
categories: Vue.js
tags:
- Vue
- Vue.js
- vue-cli
- ESLint
---

在使用 `vue-cli` 建立專案時，也會詢問是否要使用 `ESLint` ，基於開發時避免一些JS的較為特殊的錯誤，建議還是使用 `ESLint` 來規範程式碼。但是開發中總是會有比較特殊的需求 `ESLint` 是沒辦法符合的，所以這時我們要加入一些設定來忽略掉錯誤，以下使用程式碼來做教學。

<!-- more -->

```javascript
function (person){
  person.name='Jeremy';
}
```

> [eslint] Assignment to property of function parameter 'to'. (no-param-reassign)

當我們貼上上面的程式碼後會發現，ESLint會出現上述錯誤警告，告訴我們這個錯誤需要處理[（詳情請點擊）](https://eslint.org/docs/rules/no-param-reassign)，但我們仍然需要這樣做時，該怎麼辦？以下有3種方式可以處理。

## 1. 在`.eslintrc.js`中禁用這項規則

打開 `.eslintrc.js` ，在 `rules` 內新增禁用規則

```javascript
rules: {
    //...上略
     "no-param-reassign": [2, { props: false }]
  }
```

## 2. 在整個`function`內禁用

```javascript
/*eslint-disable no-param-reassign*/
function (person){
  person.name='Jeremy';
}
/*eslint-enable no-param-reassign*/
```

## 3. 禁用指定的程式碼

```javascript
function (person){
  person.name='Jeremy'; // eslint-disable-line no-param-reassign
}
```