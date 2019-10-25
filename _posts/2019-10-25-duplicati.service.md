---
layout: post
title: 備份好軟體 duplicati linux安裝
date: 2019-10-25
tags: backup
---

OS CentOS7 ... install (2.0.4.32, 31 版本有問題 :(....)

```
yum install yum-utils

rpm --import "http://keyserver.ubuntu.com/pks/lookup?op=get&search=0x3FA7E0328081BFF6A14DA29AA6A19B38D3D831EF"

yum-config-manager --add-repo http://download.mono-project.com/repo/centos7/

yum install mono-devel

yum install epel-release

yum install libappindicator

yum install -y https://github.com/duplicati/duplicati/releases/download/v2.0.4.30-2.0.4.30_canary_2019-09-20/duplicati-2.0.4.30-2.0.4.30_canary_20190920.noarch.rpm
```
手動安裝也可
```
libmono-llvm0-6.4.0.198-0.xamarin.3.epel7.x86_64.rpm
mono-core-6.4.0.198-0.xamarin.3.epel7.x86_64.rpm
mono-data-6.4.0.198-0.xamarin.3.epel7.x86_64.rpm
mono-devel-6.4.0.198-0.xamarin.3.epel7.x86_64.rpm
mono-llvm-tools-6.0+mono20190708165219-0.xamarin.1.epel7.x86_64.rpm
mono-mvc-6.4.0.198-0.xamarin.3.epel7.x86_64.rpm
mono-wcf-6.4.0.198-0.xamarin.3.epel7.x86_64.rpm
mono-web-6.4.0.198-0.xamarin.3.epel7.x86_64.rpm
mono-winforms-6.4.0.198-0.xamarin.3.epel7.x86_64.rpm
```
set firewall
```
firewall-cmd --add-port=8200/tcp --permanent
firewall-cmd --reload

```
vi startup config
```
/etc/systemd/system/duplicati.service
```

duplicati.service
```
[Unit]
Description=Duplicati Backup software
[Service]
ExecStart=/usr/bin/mono /usr/lib/duplicati/Duplicati.Server.exe --webservice-interface=any
Restart=on-failure
RestartSec=30
[Install]
WantedBy=multi-user.target
```

startup service
```
systemctl enable duplicati
systemctl start duplicati
```
manage backup
```
 http://ipaddress:8200/
```
