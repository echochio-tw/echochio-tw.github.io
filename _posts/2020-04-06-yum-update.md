---
layout: post
title: yum 失败 proxy vpn
date: 2020-04-06
tags: yum proxy ssh vpn
---

先换 dns /etc/resolv.conf 測試
```
nameserver 1.1.1.1
```

先测 VPN 在 /etc/yum.conf 加入
```
proxy=socks5h://localhost:1080
```

在 terminal 打
```
ssh -D 1080 YOUR_USER@YOUR_SERVER_WITH_FULL_WEB_ACCESS 
```

就是 .....
```
ssh -D 1080 root@212.219.12.5
```

輸入 password ....

到 proxy 那边 sshd 找/etc/ssh/sshd_config 去找 Forwarding
```
AllowAgentForwarding yes
AllowTcpForwarding yes
X11Forwarding yes
```

yum 看看 proxy 會不會通
```
yum update
```


不行就大招 换 yum 移除 ....
```
rpm -e yum-plugin-fastestmirror-1.1.31-52.el7.noarch --nodeps
rpm -e yum-metadata-parser-1.1.4-10.el7.x86_64 --nodeps
rpm -e yum-utils-1.1.31-52.el7.noarch --nodeps
```

抓安装包
```
wget http://mirror01.idc.hinet.net/CentOS/7/os/x86_64/Packages/libxml2-python-2.9.1-6.el7_2.3.x86_64.rpm                    
wget http://mirror01.idc.hinet.net/CentOS/7/os/x86_64/Packages/python-ipaddress-1.0.16-2.el7.noarch.rpm  
wget http://mirror01.idc.hinet.net/CentOS/7/os/x86_64/Packages/lvm2-python-libs-2.02.185-2.el7.x86_64.rpm
wget http://mirror01.idc.hinet.net/CentOS/7/os/x86_64/Packages/python-kitchen-1.1.1-5.el7.noarch.rpm     
wget http://mirror01.idc.hinet.net/CentOS/7/os/x86_64/Packages/yum-3.4.3-163.el7.centos.noarch.rpm
wget http://mirror01.idc.hinet.net/CentOS/7/os/x86_64/Packages/python-2.7.5-86.el7.x86_64.rpm                                
wget http://mirror01.idc.hinet.net/CentOS/7/os/x86_64/Packages/python-libs-2.7.5-86.el7.x86_64.rpm       
wget http://mirror01.idc.hinet.net/CentOS/7/os/x86_64/Packages/yum-metadata-parser-1.1.4-10.el7.x86_64.rpm
wget http://mirror01.idc.hinet.net/CentOS/7/os/x86_64/Packages/python-backports-ssl_match_hostname-3.5.0.1-1.el7.noarch.rpm  
wget http://mirror01.idc.hinet.net/CentOS/7/os/x86_64/Packages/python-pycurl-7.19.0-19.el7.x86_64.rpm    
wget http://mirror01.idc.hinet.net/CentOS/7/os/x86_64/Packages/yum-plugin-aliases-1.1.31-52.el7.noarch.rpm
wget http://mirror01.idc.hinet.net/CentOS/7/os/x86_64/Packages/python-chardet-2.2.1-3.el7.noarch.rpm                         
wget http://mirror01.idc.hinet.net/CentOS/7/os/x86_64/Packages/python-setuptools-0.9.8-7.el7.noarch.rpm  
wget http://mirror01.idc.hinet.net/CentOS/7/os/x86_64/Packages/yum-plugin-fastestmirror-1.1.31-52.el7.noarch.rpm
wget http://mirror01.idc.hinet.net/CentOS/7/os/x86_64/Packages/python-devel-2.7.5-86.el7.x86_64.rpm                          
wget http://mirror01.idc.hinet.net/CentOS/7/os/x86_64/Packages/python-urlgrabber-3.10-9.el7.noarch.rpm   
wget http://mirror01.idc.hinet.net/CentOS/7/os/x86_64/Packages/yum-plugin-protectbase-1.1.31-52.el7.noarch.rpm
wget http://mirror01.idc.hinet.net/CentOS/7/os/x86_64/Packages/python-iniparse-0.4-9.el7.noarch.rpm                          
wget http://mirror01.idc.hinet.net/CentOS/7/os/x86_64/Packages/rpm-python-4.11.3-40.el7.x86_64.rpm       
wget http://mirror01.idc.hinet.net/CentOS/7/os/x86_64/Packages/yum-utils-1.1.31-52.el7.noarch.rpm
wget http://mirror01.idc.hinet.net/centos/7/os/x86_64/Packages/rpm-libs-4.11.3-40.el7.x86_64.rpm
```

安装 ...
```
rpm -Uvh --replacepkgs lvm2-python-libs*.rpm --nodeps --force
rpm -Uvh --replacepkgs libxml2-python*.rpm --nodeps --force
rpm -Uvh --replacepkgs python*.rpm --nodeps --force
rpm -Uvh --replacepkgs rpm-python*.rpm yum*.rpm --nodeps --force
rpm -Uvh  rpm-libs-4.11.3-40.el7.x86_64.rpm --nodeps --force
```

删除  repo
```
rm -rf /etc/yum.repos.d/
vim /etc/yum.repos.d/CentOS-Base.repo
```

设定 /etc/yum.repos.d/CentOS-Base.repo
```
# CentOS-Base.repo
#
# The mirror system uses the connecting IP address of the client and the
# update status of each mirror to pick mirrors that are updated to and
# geographically close to the client. You should use this for CentOS updates
# unless you are manually picking other mirrors.
#
# If the mirrorlist= does not work for you, as a fall back you can try the 
# remarked out baseurl= line instead.
#
#

[base]
name=CentOS-$releasever - Base
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os
baseurl=http://mirror01.idc.hinet.net/CentOS/$releasever/os/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

#released updates 
[updates]
name=CentOS-$releasever - Updates
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=updates
baseurl=http://mirror01.idc.hinet.net/CentOS/$releasever/updates/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

#additional packages that may be useful
[extras]
name=CentOS-$releasever - Extras
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=extras
baseurl=http://mirror01.idc.hinet.net/CentOS/$releasever/extras/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

#additional packages that extend functionality of existing packages
[centosplus]
name=CentOS-$releasever - Plus
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=centosplus
baseurl=http://mirror01.idc.hinet.net/CentOS/$releasever/centosplus/$basearch/
gpgcheck=1
enabled=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

#contrib - packages by Centos Users
[contrib]
name=CentOS-$releasever - Contrib
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=contrib
baseurl=http://mirror01.idc.hinet.net/CentOS/$releasever/contrib/$basearch/
gpgcheck=1
enabled=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
```
