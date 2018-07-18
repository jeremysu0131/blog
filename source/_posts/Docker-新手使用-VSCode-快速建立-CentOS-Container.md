---
title: '[Docker] 新手使用 VS Code 快速建立 CentOS Container'
date: 2018-07-18 10:45:55
categories: Docker
tags: 
- Docker
- VS Code
- Node.js
---

# 前言

最近開始學習 Docker ，但是在指令操作上總是有點不是那麽熟悉，尤其是在攥寫 DockerFile 時，常常會忘東忘西，實在有點困擾，好在最近 `Microsoft` 針對 VS Code 開發了一個 `Docker`套件來讓我們方便操作。

<!-- more -->

## 安裝

1.  安裝 Docker （安裝方式請針對自己的 OS 來查詢如何安裝）
2.  安裝 Visual Studio Code
3.  開啓 Visual Studio Code ，點選右邊的 Extension 搜尋 `Docker` 並安裝

![installdockerextension](installdockerextension.png)

4.  安裝完成後重啓 VS Code

## 操作

![installdockerextension](dockerextension.png)

重啓 VS Code 後就可以看到右側多了一個 Docekr 的 Icon 點選後即可查看我們現在有多少 Image 、 Container 已經他們現在的狀態。

### Pull CentOS Image

這邊我們來拉取一個 CentOS 的 Image 做操作，首先在終端機中輸入

```bash
$ docker pull centos

Using default tag: latest
latest: Pulling from library/centos
7dc0dca2b151: Pull complete
Digest: sha256:b67d21dfe609ddacf404589e04631d90a342921e81c40aeaf3391f6717fa5322
Status: Downloaded newer image for centos:latest
```

這樣我們就可以在 Docker 瀏覽器中看到我們剛剛拉取下來的 Image

![dockerimage](dockerimage.png)

### Run CentOS Image

接下來對著它按右鍵，點選 `Run Interactive` 這是 VS Code 就會跳出一串 docker 執行指令，並且已經進入這個 container 中的 bash。

```bash
$ docker run --rm -it centos:latest # 這行就是docker的啓動指令

[root@1963c65890f5 /] uname # 這邊已經進入 centos 這個 container 了
Linux
```

我們可以輸入 exit 或 `ctrl+d` 離開，這時你就會發現右側在 Containers 的 centos:latest 被移除了，這個原因是因爲 VS Code 的 docker run 指令中包含了 `--rm` 這個參數，這個意思是：

> Automatically remove the container when it exits
> 當執行結束時自動移除這個容器

我在想微軟會這樣設計應該是方便讓開發者測試這樣容器是否可以正常運作，沒問題後就可以再做更多設定，所以我們只要把參數中的 `--rm` 刪除，在終端機中執行。

```bash
docker run -it centos:latest
```

然後我們輸入 exit 離開，這樣在結束執行容器時我們也可以保留這個容器的資料，在 Docker 瀏覽器中也可以看到現在這個容器顯示紅色 icon，停止的狀態，如果要再次使用這個容器只要右鍵選擇 `Restart Container` 就可以重啓這個容器，要進入這個容器就右鍵選擇 `Attach Shell` ，非常簡單。

## 結語

我覺得微軟提供這個套件可以然新手簡單的瞭解 Docker 大概是如何運作，Image 與 Container 的觀念也可以快速釐清，而且每一個操作都會有相對應的指令出來，試著去點選操作容器，應該可以提升學習的成效。
