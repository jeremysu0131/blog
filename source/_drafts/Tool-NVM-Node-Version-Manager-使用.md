---
title: '[Tool] NVM - Node Version Manager 使用'
date: 2017-12-23 15:08:57
categories:
tags:
---

## 升級 node

升級其實就是安裝新的 node 且把舊的移除，但如果只用安裝新的會發現先前的 package 都要重新安裝，所以我們執行一下指令來安裝 node 與 package 。

```bash
nvm install NEW_VERSION --reinstall-packages-from=OLD_VERSION

# nvm install 8.9.3 --reinstall-packages-from=6.12.2
```

最後移除舊版本即完成：

```bash
nvm uninstall OLD_VERSION

# nvm uninstall 6.12.2
```
