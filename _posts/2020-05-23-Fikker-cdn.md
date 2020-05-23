---
layout: post
title: Fikker cdn
date: 2020-05-23
tags: Fikker cdn nginx
---

Fikker cdn ...

<img src="/images/posts/cdn/a.png">


```
FikkerInstallDir="/root" # default installation directory
FikkerNewVersion="fikkerd-3.8.1-linux-x86-64" # package name
service iptables stop 2> /dev/null 
chkconfig iptables off 2> /dev/null 
service httpd stop 2> /dev/null 
service nginx stop 2> /dev/null 
chkconfig httpd off 2> /dev/null
chkconfig nginx off 2> /dev/null 
systemctl stop firewalld.service 2> /dev/null 
systemctl disable firewalld.service 2> /dev/null
systemctl stop httpd.service 2> /dev/null 
systemctl stop nginx.service 2> /dev/null 
systemctl disable httpd.service 2> /dev/null 
systemctl disable nginx.service 2> /dev/null 
yum install -y wget tar
cd $FikkerInstallDir 
rm -rf $FikkerNewVersion.tar.gz 
wget -c --no-check-certificate https://www.fikker.com/dl/$FikkerNewVersion.tar.gz 
tar zxf $FikkerNewVersion.tar.gz 
rm -rf $FikkerNewVersion.tar.gz
cd $FikkerNewVersion
./fikkerd.sh install
./fikkerd.sh start
cd $FikkerInstallDir 
sleep 5 
echo 'finished!'
```

```
http://your-fikker-ip:6780/
管理员/监控员的初始密码：123456
```

如是 ubuntu 就將yum 換 apt-get 
