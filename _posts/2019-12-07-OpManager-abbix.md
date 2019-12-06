---
layout: post
title: CentOS OpManager java 掛 zabbix 監控
date: 2019-12-07
tags: OpManager java zabbix CentOS
---

OpManager 的 java 版本抓 jdk-devel

看到 java 版本是 
```
java version "1.8.0_181"
Java(TM) SE Runtime Environment (build 1.8.0_181-b13)
Java HotSpot(TM) 64-Bit Server VM (build 25.181-b13, mixed mode)
```


jdk-devel 是
```
java-1.8.0-openjdk-devel-1.8.0.181-7.b13.el7.x86_64.rpm
```

我用 7zip 解 rpm 檔案再複製到 CentOS 再放到 jre 內
```
cp -ra java-1.8.0-openjdk-1.8.0.181-7.b13.el7.x86_64 /opt/ManageEngine/OpManager/jre
chmod +x /opt/ManageEngine/OpManager/jre/*
```

這一定要將 jstat 可 NOPASSWD sudo
```
visudo
zabbix  ALL=(ALL)       NOPASSWD: /opt/ManageEngine/OpManager/jre/bin/jstat
```
放入 zabbix agent

/etc/zabbix/zabbix_agentd.d/userparameter.conf
```
UserParameter=jstat.gc,cd /opt/ManageEngine/OpManager/jre/bin;sudo ./jstat -gcutil `ps -ef | grep java |grep -v grep |awk '{print $2}'` |awk '{print $4}'|grep -E '^[0-9]'
```
這邊只用第四個
O   — Heap上的 Old space 區已使用的百分比

這是一個大學問還沒研究 .....

