---
title: '[MySQL] 記錄執行過的query'
date: 2017-11-27 14:55:49
categories: MySQL
tags:
- MySQL
- log
---

最近接手了個專案，需要將不同的 CRM 系統來做同步，由於系統架構龐大且較為複雜，但
我們只需要取裡面的一些資料，所以可以設定 MySQL 來偵測系統執行了那些 query。

### 修改 MySQL 設定

```bash
vim /etc/my.cf #CentOS 7
```

### 設定檔中加入下面這行

```vim
log = /tmp/mysql.log
```

> 設定完後記得重新啟動 MySQL

### log 位置

```bash
/tmp/your_mysql_file/tmp/mysql.log
```

> MySQL 每次重新啟動後 tmp 底下的資料夾會刪除並重新建立，如果要保留 log 檔記得要複製出來
