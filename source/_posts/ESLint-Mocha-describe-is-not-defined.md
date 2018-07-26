---
title: '[ESLint] Mocha describe is not defined'
date: 2018-07-26 10:52:18
categories: ESLint
tags:
- ESLint
- Mocha
---

# ESLint for Test

ESLint 是在團隊中非常重要的程式碼規範工具，可以規範每個人的寫作風格，所以在團隊開發時我會規範大家在專案中全面使用，包括撰寫 test，但在使用`mocha`, `jasmine`, `mongo` 等我們會發現到如下的錯誤：

<!-- more -->

```bash
$ yarn run eslint test/**

yarn run v1.7.0
$ /Users/jeremysu/Documents/Projects/express-test/node_modules/.bin/eslint test/routes

/Users/jeremysu/Documents/Projects/express-test/test/routes/index.test.js
   4:1  error  'describe' is not defined  no-undef
   5:3  error  'it' is not defined        no-undef
  12:1  error  'describe' is not defined  no-undef
  13:3  error  'it' is not defined        no-undef
✖ 4 problems (4 errors, 0 warnings)

error Command failed with exit code 1.
info Visit https://yarnpkg.com/en/docs/cli/run for documentation about this command.
```

## no-undef 問題

`no-undef`  這個錯誤代表說，ESLint 不允許使用未宣告的參數，以避免錯誤，詳細內容參考[這裡](https://eslint.org/docs/rules/no-undef)，可是在測試中，預設就是用 `describe` 與 `it` 來做宣告，這樣要如何解決問題呢？

## 解法

其實解法很簡單，只要在你的 eslint 設定檔中加入如下設定即可使用：

```js
{
    "env": {
        "mocha": true
    }
}
```

`env` 代表的是你這個專案開發環境的全局變量，所以我們只要把 `mocha` 設定好，這樣系統就會應用一些 mocha 專用的程式碼，而如果你還有其他套件要使用在專案裡，如 `jasmine`, `jquery` 等需要用到一些特別的全域變數，就可以把他放進 env 來讓 ESLint 去解析，更多詳細介紹請看[這裡](https://eslint.org/docs/user-guide/configuring#specifying-environments)。
