---
layout: post
title: Windows install zabbix agent
date: 2019-05-17
tags: zabbix
---

Advance CMD
```
netsh advfirewall firewall add rule name=" Zabbix_Agent_TCP(10050) Port" protocol=TCP dir=in localport=10050 action=allow
netsh advfirewall firewall add rule name=" Zabbix_Agent_UDP(10050) Port" protocol=UDP dir=in localport=10050 action=allow
```
GET file 
```
https://www.zabbix.com/downloads/3.4.0/zabbix_agents_3.4.0.win.zip
```
unzip
```
unzip zabbix_agents_3.4.0.win -d C:/zabbix 
```

zabbix_agentd.win.conf
```
LogFile=C:\zabbix\zabbix_agentd.log
Server=192.168.1.X
ServerActive=192.168.1.X
Hostname=Win-server
```
Advance CMD
```
C:\zabbix\bin\win64\zabbix_agentd.exe -i -c C:\zabbix\conf\zabbix_agentd.win.conf
C:\zabbix\bin\win64\zabbix_agentd.exe -c C:\zabbix\conf\zabbix_agentd.win.conf -s
```
check service
```
netstat -ano | findstr "10050"
```
