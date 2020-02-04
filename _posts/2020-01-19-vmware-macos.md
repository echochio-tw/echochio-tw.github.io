---
layout: post
title: vmware workstation macos
date: 2020-01-19
tags: macos
---

下載 ios
https://www.geekrar.com/download-macos-mojave-iso-for-vmware-virtualbox/

我選 one full driver
```
https://drive.google.com/drive/folders/1NncaD1qpJYmMSvTz8vn5oze6Yt7KgkKE
```

解壓密碼網站有寫是 Geekrar.com

vmware unlocker .... 請自行 google ...

我找到 ....

https://github.com/oxygen-TW/unlocker

下載 unlocker.exe

執行前將 vmware 的服務都停掉 ....

用 admin 去執行

這就 unlock vmware 了

也下載 gettools.py

這要用 python 去執行會下載 darwin.ios 這個 vmtool


開 vmware ... 

guest os 用 Apple Mac OS 10.14 

其他我選 4C4G80GSATA

macOS Mojave的安裝程式 -> 繁體中文

macOS的工具程式選單 -> 磁碟工具程式

選 vmware stat -> 清除

虛擬硬碟的名稱自己取。格式 -> APFS。 架構-> GUID 分割驅配置表

關閉磁碟工具程式，回到macOS的工具程式選單。「安裝 macOS」

第一次進入安裝完成的macOS，會需要進行地區、鍵盤、Apple ID的設定。

進入 Recovery Mode 關閉 System Integrity Protection 安裝 vmtools

重新開機虛擬機器，在出現灰色 VMware 字樣時按住 Alt 進入設定

Enter Setup -> Boot from a file -> Recovery -> 只有一個可選 -> boot.efi

進入 Recovery mode 後從工具程式 -> 終端機

打 csrutil disabled; reboot

重新啟動 後可用 darwin.ios 這個 vmtool

解析度無法調整
```
sudo defaults write /Library/Preferences/com.apple.windowserver DisplayResolutionEnabled -bool NO
```
