---
title: '[Tool] Putty - 使用PSCP在Linux與Windows間傳送檔案'
date: 2017-11-30 16:28:13
categories: Tools
tags:
- putty
- pscp
- windows
- linux
---

## PSCP 介紹

PSCP 是一套使用命令提示列的軟體，它提供 SCP client 的功能。當我們僅需要將一個或少數檔案從 pc 端 upload 到 server 端時，這套軟體就提供非常安全的方法，使得所傳送的內容不會被其他人給竊聽。倘若遠端有提供 SSH 2 建議還是使用 PSFTP 會比較好。

<!-- more -->

## 準備流程

1. 下載 `pscp.exe` [(32-bit)](https://the.earth.li/~sgtatham/putty/latest/w32/pscp.exe) [(64-bit)](https://the.earth.li/~sgtatham/putty/latest/w64/pscp.exe)
1. 將 `pscp.exe` 放置到你想要的資料夾
1. 打開 terminal 切換到 `pscp.exe` 位置
1. 輸入 pscp 時會出現以下訊息，這樣代表可以使用了

```text
yourfilelocation>
λ pscp
PuTTY Secure Copy client
Release 0.70
Usage: pscp [options] [user@]host:source target
       pscp [options] source [source...] [user@]host:target
       pscp [options] -ls [user@]host:filespec
Options:
  -V        print version information and exit
  -pgpfp    print PGP key fingerprints and exit
  -p        preserve file attributes
  -q        quiet, don't show statistics
  -r        copy directories recursively
  -v        show verbose messages
  -load sessname  Load settings from saved session
  -P port   connect to specified port
  -l user   connect with specified username
  -pw passw login with specified password
  -1 -2     force use of particular SSH protocol version
  -4 -6     force use of IPv4 or IPv6
  -C        enable compression
  -i key    private key file for user authentication
  -noagent  disable use of Pageant
  -agent    enable use of Pageant
  -hostkey aa:bb:cc:...
            manually specify a host key (may be repeated)
  -batch    disable all interactive prompts
  -proxycmd command
            use 'command' as local proxy
  -unsafe   allow server-side wildcards (DANGEROUS)
  -sftp     force use of SFTP protocol
  -scp      force use of SCP protocol
  -sshlog file
  -sshrawlog file
            log protocol details to a file
```

## 下載資料

```bash
#pscp [options] user@host:source target
#pscp [選項] 使用者名稱@host ip或名稱:要下載檔案的位置 本機存儲的位置
pscp jeremy@8.8.8.8:/home/user/picture.jpg c:/document/picture.jpg
```

## 上傳資料

```bash
#pscp [options] target [user@]host:source
#pscp 要上傳檔案的位置 [選項] 使用者名稱@host ip或名稱:遠端存儲的位置
pscp C:/document/picture.jpg jeremy@8.8.8.8:/home/user/picture.jpg
```

## 參考資料

* [PSCP 介紹](https://bcc16.ncu.edu.tw/6/putty/pscp.html)
