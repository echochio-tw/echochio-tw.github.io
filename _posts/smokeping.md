


Centos 7 安装 smokeping
```
yum install https://dl.fedoraproject.org/pub/epel/7/x86_64/Packages/e/epel-release-7-11.noarch.rpm
yum install http://dl.fedoraproject.org/pub/epel/7/x86_64/Packages/s/smokeping-2.6.10-3.el7.noarch.rpm
```

config 改 全区 .... 再重启 apache
```
/etc/httpd/conf.d/smokeping.conf
Require all granted
........
........
service httpd restart
```
