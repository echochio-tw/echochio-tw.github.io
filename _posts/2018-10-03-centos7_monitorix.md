---
layout: post
title: centos7 monitorix
date: 2018-10-03
tags: monitorix
---

install monitorix
```
# yum install epel-release
# yum install monitorix , httpd-tools
# systemctl start monitorix
# systemctl enable monitorix
# 
# systemctl restart monitorix
# systemctl 
#
```

add htpasswd
```
htpasswd -c /var/lib/monitorix/htpasswd admin
```

/etc/monitorix/monitorix.conf

```
        host =
        port = 50088
        user = nobody
        group = nobody

```

enable htpasswd
```
        <auth>
                enabled = y
                msg = Monitorix: Restricted access
                htpasswd = /var/lib/monitorix/htpasswd
        </auth>
```

port list
```
<port>
        max = 9
        rule = 24000
        list = 25, 21, 80, 22, 110, 139, 3306, 53, 143
        <desc>
                25      = SMTP,    tcp, in, 0, 1000, L
                21      = FTP,     tcp, in, 0, 1000, L
                80      = HTTP,    tcp, in, 0, 1000, L
                22      = SSH,     tcp, in, 0, 1000, L
                110     = POP3,    tcp, in, 0, 1000, L
                139     = NETBIOS, tcp, in, 0, 1000, L
                3306    = MYSQL,   tcp, in, 0, 1000, L
                53      = DNS,     udp, in, 0, 1000, L
                143     = IMAP,    tcp, in, 0, 1000, L
        </desc>
        graphs_per_row = 3
</port>
```

install with ansible

```
https://github.com/echochio-tw/ansible-cnetos-monitorix-install
```
