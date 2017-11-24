---
title: '[Javascript] 將含有斜杠的json轉換成物件'
date: 2017-11-24 07:43:02
categories:
- Javascript
tags: 
- javascript
- api
- slashe
- json
- object
- JSON.parse()
---

## 帶有斜杠的json

有時候我們在與其他人做API對接時，會發現有些json的格式會如下：

```json
{
    "item": "[{\"Authors\":[{\"NameID\":624,\"NameTypeID\":1,\"LastName\":\"Daans\",\"ForeName\":\"Melina\",\"Initials\":\"M\",\"DisplayName\":\"Daans, Melina\",\"SequenceNo\":0},{\"NameID\":625,\"NameTypeID\":1,\"LastName\":\"Lories\",\"ForeName\":\"Rik Ju\",\"Initials\":\"R J\",\"DisplayName\":\"Lories, Rik Ju\",\"SequenceNo\":1},{\"NameID\":626,\"NameTypeID\":1,\"LastName\":\"Luyten\",\"ForeName\":\"Frank P\",\"Initials\":\"F P\",\"DisplayName\":\"Luyten, Frank P\",\"SequenceNo\":2}],\"UserID\":\"813\",\"ItemID\":179,\"SequenceNo\":0,\"ItemTypeID\":4,\"FlagID\":0,\"Title\":\"Dynamic activation of bone morphogenetic protein signaling in collagen induced arthritis supports their role in joint homeostasis and disease.\",\"Title2\":\"Arthritis research & therapy\",\"Author\":\"Daans, Melina|Lories, Rik Ju|Luyten, Frank P|\",\"Affiliation\":\"\",\"Edition\":\"\",\"Volume\":\"10\",\"Volume2\":\"5\"}]"
}
```

其實`\"`這個是為了讓後面的引號`"`顯示出來，我們在**解析時不需要特別去注意**，使用`JSON.parse()`一樣可以進行轉換。

## JSON.parse() 介紹

> [JSON.parse()](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/JSON/parse) 方法把會把一個JSON字串轉換成 JavaScript的數值或是物件。另外也可選擇使用reviver函數讓這些數值或是物件在被回傳之前做轉換。

## Example

```javascript Use JSON.parse() convert to object
var jsonStr = `{\"LastName\":\"Daans\",\"ForeName\":\"Melina\",
\"Initials\":\"M\",\"DisplayName\":\"Daans, Melina\"}`;

console.log(JSON.parse(jsonStr));
// { LastName: 'Daans', ForeName: 'Melina', Initials: 'M', DisplayName:
//  'Daans, Melina' }

console.log(JSON.parse(jsonStr).LastName); // Daans



```