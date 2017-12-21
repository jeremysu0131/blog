---
title: '[Tool] Mac + iTerm 2 + Oh My Zsh + Powerlevel9k 設定'
date: 2017-12-21 21:32:16
categories: Tools
tags:
- Mac
- iTerm 2
- Zsh
- Oh My Zsh
- Powerlevel9k
---

#  基本使用

## 安裝 iTerm 2

<!-- more -->

```bash
brew cask install iterm2
```

安裝完成後打開 iTerm 繼續執行以下指令

## 安裝 zsh

```bash
brew install zsh zsh-completions
```

## 安裝 Git

```bash
brew install git
```

## 安裝 Oh My Zsh

```bash
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

## 安裝字體 Nerd-Fonts

```bash
brew tap caskroom/fonts #字體安裝軟體，只需執行一次以後就可以去下載其他字體
brew cask install font-hack-nerd-font # 安裝字體
```

## iTerm 設定字體

進入 iTerm 的設定 > Profiles > Text > Change Font >  選擇 `Knack Nerd Font` 字體 Regular 大小 18pt，完成後重新啟動 iTerm ，設定完成。

![oh-my-zsh](https://cloud.githubusercontent.com/assets/2618447/6316862/70f58fb6-ba03-11e4-82c9-c083bf9a6574.png)

# 進階使用

如果想要讓終端機有更多功能，我們可以使用 Powerlevel9K 來使其更豐富

## 安裝 Powerlevel9K

```bash
git clone https://github.com/bhilburn/powerlevel9k.git ~/.oh-my-zsh/custom/themes/powerlevel9k
```

### 設定

執行完上面指令後可以在 $HOME 下找到 `.zshrc`，進入設定：

```bash
vim ~/.zshrc
```

1. 找到 `ZSH_THEME="robbyrussell"` ，將它改為 `ZSH_THEME="powerlevel9k/powerlevel9k"`，啟用 Powerlevel9K 主題
2. 接下來貼上下面程式碼來做調整：

```bash
POWERLEVEL9K_MODE='nerdfont-complete' # 調整字體，必須設定
```

下面是我自己更改的設定，有需要可以自行複製貼上

```bash
POWERLEVEL9K_ALWAYS_SHOW_USER=true
POWERLEVEL9K_CONTEXT_TEMPLATE="%n"
POWERLEVEL9K_PROMPT_ON_NEWLINE=true
POWERLEVEL9K_RPROMPT_ON_NEWLINE=true
POWERLEVEL9K_LEFT_PROMPT_ELEMENTS=(user dir vcs)
POWERLEVEL9K_RIGHT_PROMPT_ELEMENTS=(status time)
POWERLEVEL9K_STATUS_VERBOSE=false
POWERLEVEL9K_MULTILINE_FIRST_PROMPT_PREFIX=""
POWERLEVEL9K_MULTILINE_LAST_PROMPT_PREFIX="%{%b%f%k%F{blue}%} %{%f%}"
```

啟用新設定：

```bash
source ~/.zshrc
```

執行完後會發現畫面會有所改變，檢查正常無誤就代表設定成功了。

![powerlevel9k](snapshot.png)

>  如果主題或字體看不習慣，[官方](https://github.com/bhilburn/powerlevel9k)還有提供其他的設定方式
