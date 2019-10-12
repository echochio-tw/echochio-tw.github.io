---
layout: post
title: windows aotologin
date: 2019-10-04
tags: windows teamviewer aotologin
---

teamviewer 要有 console 設定

windows 自動登入 

login.reg 檔,按右鍵 / Merge 來匯入,

```
Windows Registry Editor Version 5.00
 
[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon]
"DefaultUserName"="administrator"
"DefaultPassword"="*8Ncxjck[e5W-Y=$"
"AutoAdminLogon"="1"
```
