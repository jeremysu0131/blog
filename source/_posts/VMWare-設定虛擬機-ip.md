---
title: '[VMWare] 設定虛擬機 ip'
date: 2018-02-07 10:23:15
categories: Tool
tags:
- VMWare
- net
- ip
- CentOS
---

## 確定虛擬機網路設定

這邊要先確定虛擬機的網路設定是 NAT 。

<!-- more -->

![vm net setting](vm.png)

## VMnet8

到網際網路中找到 VMnet8 ，看看是否有啟動，另外進入設定 IP ，這裡的 IP 位置就是未來虛擬機中的網段。

![vm net 8 setting](vmnet8.png)

## 到虛擬機中的作業系統設定 ip

這邊以 CentOS 7 為例：

1. 檢查 Network Manager 是否啟動

```bash
systemctl status NetworkManager.service
```

2. 檢查網卡，綠色顯示的為使用中的，我們要設定的就是這張

```bash
nmcli dev status
```

3. 進入 nmtui 設定網路，若沒有則安裝 `yum install NetworkManager-tui`

```bash
sudo nmtui 你的網路卡名稱
```

將 `IPv4 CONFIGURATION` 的 `Automatic` 改成 `Manual` ，然後按下旁邊的 `Show` ，即可設定固定 ip ，記得網段要跟先前 VMnet8 中設定的一樣。
![vm net setting](nmtui.png)

4. 重新啟動網路

```bash
systemctl restart network.service
```

5. 檢查設定是否生效

```bash
ip add
```

### 參考來源

> * [VMWare 虚拟机下为 Ubuntu 12.04.1 配置静态 IP（NAT 方式）](http://www.cnblogs.com/objectorl/archive/2012/09/27/vmware-ubuntu-nat-static-ip-settings.html)
> * [RHEL / CentOS 7 設定網路固定 IP](https://www.phpini.com/linux/rhel-centos-7-setup-static-ip)
