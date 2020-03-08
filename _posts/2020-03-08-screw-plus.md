---
layout: post
title: php 代碼加密 screw-plus
date: 2020-03-08
tags: php screw-plus screw
---

#先裝好 php7.1 套件 .... 這邊就省略...

裝需要的套件
```
yum-config-manager --enable remi-php71
yum install php-devel
yum install php-pear
yum install gcc gcc-c++ autoconf automake
yum install zlib-devel
yum install libtool libxml2-devel libxslt-devel perl-devel perl-ExtUtils-Embed pcre-devel
```
裝 Xdebug
```
pecl install Xdebug
```
編輯 /etc/php.ini  /etc/php.ini
```
[xdebug]
zend_extension="/usr/lib64/php/modules/xdebug.so"
xdebug.remote_enable = 1
```
抓 screw-plus
```
git clone https://github.com/del-xiong/screw-plus.git
```
編譯前先 phpize
```
/usr/bin/phpize
```
加密 CAKEY 在 screw-plus/php_screw_plus.h 可去修改
```
#define CAKEY  "FwWpZKxH7twCAG4JQMO"
```
開始編譯 screw-plus
```
cd screw-plus
./configure --with-php-config=/usr/bin/php-config
make && make install
cd tools
make
cp screw /usr/bin/screw-plus
```
/etc/php.ini i 加入 php_screw_plus.so
```
extension=/usr/lib64/php/modules/php_screw_plus.so
```
重啟 php-fpm
```
systemctl restart php-fpm.service
```
將所有 php 加密
```
cd /www/webroot/
find ./ -name "*.php" -print | xargs -n1 /usr/bin/screw-plus
```

如要解密
```
/usr/bin/screw-plus -d /www/webroot/
```
