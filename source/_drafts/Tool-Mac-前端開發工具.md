---
title: '[Tool] Mac 前端開發工具'
date: 2017-12-22 14:46:13
categories:
tags:
---

## Homebrew

Homebrew 是一個類似於在 Linux 中 Ubuntu 的 apt-get 或是 CentOS 的 yum ，是一個可以讓我們安裝缺少的套件管理工具

> [安裝教學](https://brew.sh/index_zh-tw.html)

## Git

內件的 Git 版本較為老舊，建議安裝最新版

## NVM

NVM 是一個管理 Node.JS 版本的工具，使用 NVM 後我們可以很容易的切換任意版本來做開發測試。

執行下列程式碼安裝 NVM ：

```bash
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.8/install.sh | bash
```

複製以下程式碼到 `~/.bash_profile`, `~/.zshrc`, `~/.profile`, or `~/.bashrc`

```bash
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh" # This loads nvm
```

> 備註： 只要複製到其中一種在使用的設定檔即可，例如我使用 zsh ，只要把檔案複製到 `~/.zshrc` 就可以了。

## iTerm + Oh My Zsh

## Visual Studio Code

## Postman
