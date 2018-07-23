---
title: '[Docker] Build A Node.js Image'
date: 2018-07-23 10:48:03
categories: Docker
tags: 
- Docker
- Node.js
- Docker Image
---

# Docker + Node.js

最近開始接觸容器化技術後，開始覺得這是未來的趨勢，於是開始參加與報名了許多課程，而上完課後就是要來實作，所以這邊來紀錄如何將 Node.js 專案建置成 Docker Image。

<!-- more -->

## Install

這邊假設你已經安裝並設定好 Docker ，專案我使用 express-generator 來建置 express 做一個簡單的範例：

```bash
$ yarn global add express-generator
$ express --no-view --git express-docker #這裡不使用任何 View 模板
$ cd express-docker
$ yarn #安裝 package
$ node bin\www #啟動專案
```

接著打開瀏覽器輸入 `http://localhost:3000`，就可以  看到我們的專案正常啟動了。

![express-index](express-index.png)

## 建立 DockerFile

測試沒問題後，我們就可以了來建立 Dockerfile 了，在專案根目錄中新增 `Dockerfile` 檔案並貼上以下內容

```docker
# 使用 Node 的版本
FROM node:8.9-alpine

# Node 環境設定為 production
ENV NODE_ENV production

# Node 在容器內的位置
WORKDIR /usr/src/app

# 複製 package 設定
COPY ["package.json", "yarn.lock", "./"]

# 安裝必要的套件並移動到專案的 node_modules 底下
RUN yarn --production --silent && mv node_modules ../

# 第一個 . 是我們本地位置，第二個是 docker 裡面專案的位置，就是將我們專案的程式碼全部複製進去
COPY . .

# 開放對外的 port
EXPOSE 3000

# 執行專案
CMD yarn start
```

> 這邊使用 `node:8.9-alpine` 是因為這個版本是官方基於 Alpine Linux 來建置的專案，整個容器大小不會超過 5MB（完整版本大約有 600MB）

##  建立 image

當我們 Dockerfile 建立好後，就可以來建立 image，開啟你的 terminal 並切換到專案的根目錄下，然後執行：

```bash
$ docker build -t jeremy/express-docker:1.0.0 .

Sending build context to Docker daemon  2.121MB
Step 1/8 : FROM node:8.9-alpine
 ---> 406f227b21f5
Step 2/8 : ENV NODE_ENV production
 ---> Using cache
 ---> a5f4608e1b01
Step 3/8 : WORKDIR /usr/src/app
 ---> Using cache
 ---> 77f3a6f6ba55
Step 4/8 : COPY ["package.json", "yarn.lock", "./"]
 ---> Using cache
 ---> e382743b8956
Step 5/8 : RUN yarn --production --silent && mv node_modules ../
 ---> Using cache
 ---> 1b8ea95fa231
Step 6/8 : COPY . .
 ---> Using cache
 ---> 802418a0f972
Step 7/8 : EXPOSE 3000
 ---> Using cache
 ---> 6a6649b4f0b8
Step 8/8 : CMD yarn start
 ---> Using cache
 ---> a9658953386f
Successfully built a9658953386f
Successfully tagged jeremy/express-docker:1.0.0
```

通常在建立是我們會用 -t 這個參數來設定 container 的名稱以及標籤來  簡單的做辨識，而用法是 `[名稱]:[標籤]`。
而從上面的輸出結果我們可以看到，dockr 在建置的時候會按照 `Dockerfile` 裡面的設定順序來執行，每一行執行成功才會往下執行，最後結果出現編譯成功，再來我們在 terminal 內輸入 `docker image ls`  來查看剛剛建立的 image。

```bash
$ docker image ls
REPOSITORY            TAG   IMAGE ID     CREATED        SIZE
jeremy/express-docker 1.0.0 a9658953386f 14 seconds ago 74.2MB
```

## Run

接下來我們要來將剛剛建立好的 image 執行起來， 所以執行一下指令：

```bash
$ docker run -d -p 3000:3000 jeremy/express-docker:1.0.0
7501cc988eaaac5f38cab9c5d5e1a0dd3d1e5292411c370b46185ce4f6de6751
```

> 因為正式在操作時可能會遇到不同版本的 image ， 所以要指定執行哪一個 tag 比較好
> -d 讓 container 在背景執行，如果拿掉這個參數就會立即在 terminal 上執行
> -p 指定要使用那個 port ， 第一個是指定 container 對外的 port，第二個是指定對內要到哪一個 port
> 其他設定可以使用 `docker run --help` 來查看更多設定

最後，我們打開瀏覽器， 網址輸入 `http://localhost:3000`，就可以看到 express 正常執行了！

## 結語

像這樣的作法算是比較簡單的方式，不過在設定上可能需要下達許多指令，今天先初步了解 Dockerfile 的功能，之後會再介紹如何使用 `docker compose` ，來對 container 做更細部的設定，讓我們可以針對 port, memory, cpu 等做調整。
