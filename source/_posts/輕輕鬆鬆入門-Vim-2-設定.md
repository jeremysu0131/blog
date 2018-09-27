---
title: 輕輕鬆鬆入門 Vim (2) - 設定 
date: 2018-09-27 21:06:46
categories:
tags:
---

# Intro

相信安裝 Vim 不需要特別解釋了，所以這裡不會教大家如何安裝 Vim ，而是會告訴各位對 Vim 做一些設定， 讓後續學習更為輕鬆。

<!-- more -->

## Use Terminal

如果是使用終端機者，請在安裝好 Vim 後，繼續安裝下面的套件

### The Ultimate vimrc

`The Ultimate vimrc` 是一套  由使用 Vim 超過 10 年的老手所提供的設定，我們先不管其中的設定如何， 先安裝來使用，後面會針對如何設定 Vim 來做  更多介紹。

### Install

```bash
# 複製設定到本機的 .vim_runtime 底下
git clone --depth=1 https://github.com/amix/vimrc.git ~/.vim_runtime
# 執行安裝
sh ~/.vim_runtime/install_awesome_vimrc.sh
```

安裝好後，在終端機中輸入 Vim，如果看到以下的配色，就代表安裝成功嘍
![vim-homepage](vim-homepage.png)

## Use VS Code

由於我們是要用 Vim 指令來取代日常開發，所以如果有使用 VS Code 話，可以到套件管理搜尋 Vim ，就可以安裝下面網址的套件

> [https://marketplace.visualstudio.com/items?itemName=vscodevim.vim](https://marketplace.visualstudio.com/items?itemName=vscodevim.vim)

### Note

這款 Vim 並不會去讀取系統中的設定，所以記得到上面網址去看所需要的設定。
