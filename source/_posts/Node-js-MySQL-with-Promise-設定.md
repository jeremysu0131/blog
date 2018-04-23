---
title: '[Node.js] MySQL with Promise 處理非同步問題'
date: 2018-04-22 17:40:06
categories: Node.js
tags: 
- Node.js 
- MySQL
- Javascript
- Promise
- ES6
- Asynchronous
---

## Callback

在 Node.js 中使用 callback 來處理 MySQL Query 會使程式碼看起來很凌亂，也是被人詬病的 `Callback Hell`：

<!-- more -->

```js
// ./configs/mysql.js
var mysql = require('mysql');

const pool = mysql.createPool({
  connectionLimit: 10,
  host: 'localhost',
  user: 'user',
  password: 'user',
  database: 'database',
});

const queryUserByCity = (city, callback) => {
  pool.query(
    'SELECT name FROM users WHERE city = ? ;',
    city,
    (err, rows, fields) => {
      if (err) callback(err);
      else callback(null, rows);
    }
  );
};
module.exports = { queryUserByCity };
```

```js
// ./routes/index.js
var express = require('express');
var router = express.Router();
var db = require('../configs/mysql');

router.get('/', function(req, res, next) {
  var data = [];

  db.queryUserByCity('高雄', (err, results) => {
    if (err) res.status(500); // 每一次都要做錯誤處理
    data.push(results);
    db.queryUserByCity('台南', (err, results) => {
      if (err) res.status(500); // 每一次都要做錯誤處理
      data.push(results);
      db.queryUserByCity('屏東', (err, results) => {
        if (err) res.status(500); // 每一次都要做錯誤處理
        data.push(results);
        res.json(data);
      });
    });
  });
});

module.exports = router;
```

## Promise

上面的程式碼看起來很亂，可讀性比較不佳，但如果用 Promise 的方式來做，則程式碼看起來會比較整齊，並且在 catch 錯誤時，只要寫一次就可以了，不用再一一確認。

```js
// ./configs/mysql.js
var mysql = require('mysql');
const pool = mysql.createPool({
  connectionLimit: 10,
  host: 'localhost',
  user: 'user',
  password: 'user',
  database: 'database',
});

const queryUserByCity = city => {
  return new Promise((resolve, reject) => {
    pool.query(
      'SELECT name FROM users WHERE city = ? ;',
      city,
      (err, rows, fields) => {
        console.log(rows);
        if (err) reject(err);
        else resolve(rows);
      }
    );
  });
};

module.exports = { queryUserByCity };
```

```js
// ./routes/index.js
var express = require('express');
var router = express.Router();
var db = require('../configs/mysql');

router.get('/', function(req, res, next) {
  var data = [];
  db
    .queryUserByCity('高雄')
    .then(results => {
      data.push(results);
      return db.queryUserByCity('台南');
    })
    .then(results => {
      data.push(results);
      return db.queryUserByCity('屏東');
    })
    .then(results => {
      data.push(results);
      res.json(data);
    })
    // Promise 只要使用一次即可 catch 所有錯誤
    .catch(err => {
      res.status(500).json({ message: 'Server error' });
    });
});

module.exports = router;
```
