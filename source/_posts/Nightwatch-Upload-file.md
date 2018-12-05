---
title: '[Nightwatch] Upload file'
date: 2018-12-05 17:11:37
categories: Nightwatch
tags:
  - test
  - nightwatch
  - upload
---

# Nightwatch 上傳

最近在學習如何使用 Nightwatch 來做 E2E 測試，不過發現到上傳功能並沒有出現在官方網站，這邊紀錄一下上傳的方式

<!-- more -->

```js
"Upload File": browser => {
    browser
      .setValue(
        "input[type=file]",
        require("path").resolve(__dirname + "/test.jpg")
      )
  },
```

其實上傳方式很簡單，就像輸入文字框一樣，只不過這邊把輸入的內容改為檔案的路徑，這樣就可以成功上傳了