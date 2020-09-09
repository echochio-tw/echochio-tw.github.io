---
layout: post
title: windows 10 WSL 的 /root 在哪邊 ?
date: 2020-09-09
tags: Windows 10 WSL
---

我有需求 讀取 WSL 內的 /root/

直接用 vscode 編譯 python

在admin 的 powershell 

```
cd ${env:appdata}\..\local\packages\canonical*\localstate\rootfs
```

然後那個目錄就是 OS 目錄 ....
