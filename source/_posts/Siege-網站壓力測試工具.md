---
title: '[Siege] 網站壓力測試工具'
date: 2018-06-08 09:47:08
categories: 
- Test Tool
tags:
- test
- tool
- load test
---

[Siege](https://github.com/JoeDog/siege) 是一個簡單易用的網頁負載測試工具，通過設定我們可以在 Mac / Linux 上發送出大量的 request ，來測試開發好的網站穩定度。

<!-- more -->

## 安裝

- Mac

```bash
brew install siege
```

- Linux
  Ubuntu 可以使用 `apt-get` ，CentOS 使用 `yum` 來安裝

## 測試單一連結

```bash
siege -c 100 -r 200 -d1 'http://google.com.tw'
```

- -c 同時執行多少個執行緒
- -r 每個執行緒發出的請求
- -d 每個執行緒延遲的時間，預設為 3 秒，如果要測試網站效能建議將時間設定為 1 秒(-d1)

## HTTP Method

我們可以使用 `POST` `PUT` `DELETE` 來測試 API ，以下我們以 POST 來測試網站登錄：

```bash
siege -c 100 -r 200 -d1 'http://yourwebsite.com POST username=jeremy&password=test123'
```

HTTP Method 寫在網址後面，參數寫在最後面

## URLs File

有時我們想要跟加模擬實際使用者使用方式，siege 提供建立一個 .txt 檔來儲存所有要測試的網址：

```bash
# GET 測試
http://homer.whoohoo.com/index.shtml
http://homer.whoohoo.com/howto.jsp
http://homer.whoohoo.com/cgi-bin/hello.pl?first=bart&last=simpson

# 其他 Method 測試
http://homer.whoohoo.com/cgi-bin/hello.pl POST name=homer
http://homer.whoohoo.com/haha.jsp POST word=doh!&scope=ALL
```

建立完檔案後，我們執行以下指令即可運行測試：

```bash
siege -c 100 -r 200 -d1 -f yourfile.txt
```

## 測試報告

```text
Transactions:               507 hits  總共處理次數
Availability:               100.00 %  可用性
Elapsed time:             24.41 secs  運行時間
Data transferred:            0.10 MB  數據傳輸量
Response time:             4.33 secs  響應時間
Transaction rate:    20.77 trans/sec  每秒平均請求次數
Throughput:              0.00 MB/sec  傳輸速度
Concurrency:                   90.04  最高并發連線數
Successful transactions:         507  成功傳輸次數
Failed transactions:               0  失敗傳輸次數
Longest transaction:            5.82  處理傳輸所花最長時間
Shortest transaction:           1.40  處理傳輸最短時間
```
