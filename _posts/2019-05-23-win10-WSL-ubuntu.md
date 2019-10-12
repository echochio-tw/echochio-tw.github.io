---
layout: post
title: win10 WSL linux ubuntu 1804
date: 2019-05-23
tags: win10 WSL linux ubuntu
---
win 10 子系統 ubuntu 1804 安裝

以下都用 powershell admin 權限

启用 WSL

```
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux 
```

重開 win10
```
shutdown -r -t 0
```

下載 ubuntu 1804
```
Invoke-WebRequest -Uri https://aka.ms/wsl-ubuntu-1804 -OutFile ~/Ubuntu.appx -UseBasicParsing
```

安裝 Ubuntu
```
Add-AppxPackage -Path ~/Ubuntu.appx
```

安裝 root 目錄
```
Ubuntu1804 install --root
```

升級 package
```
Ubuntu1804 run apt update
Ubuntu1804 run apt upgrade -y
```

進入 shell
```
bash
```

