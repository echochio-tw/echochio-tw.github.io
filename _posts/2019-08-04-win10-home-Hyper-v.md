---
layout: post
title: win10家用版啟用 Hyper-v
date: 2019-08-04
tags: win10 home Hyper-v
---

cmd 管理員權限

```
C:\WINDOWS\system32> dir /b %SystemRoot%\servicing\Packages\*Hyper-V*.mum >hyper-v.txt
C:\WINDOWS\system32> for /f %i in ('findstr /i . hyper-v.txt 2^>nul') do dism /online /norestart /add-package:"%SystemRoot%\servicing\Packages\%i"
C:\WINDOWS\system32> del hyper-v.txt
C:\WINDOWS\system32> Dism /online /enable-feature /featurename:Microsoft-Hyper-V-All /LimitAccess /ALL
```
重開後去

控制台\程式集\程式和功能\開啟或關閉windows功能

就可看到 Hyper-v

尋找一下就可看到 Hyper-v 管理員
