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
cd screw-plus
/usr/bin/phpize
```
加密 CAKEY 在 php_screw_plus.h 可去修改
```
#define CAKEY  "FwWpZKxH7twCAG4JQMO"
```
開始編譯 screw-plus
```
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

同樣在php_screw_plus.h裡修改，把STRICT_MODE後面的值改為1，然後make clean && make重新編譯並重啟php

然後開啟之前加過密的網站，執行正常，但是我們隨意上傳個明文的php檔案，結果是一片空白。

原因是未加密的php檔案頭部不包含識別key，擴充套件會返回空內容，就算黑客獲取了key並加入也沒用，內容會被解密成亂碼仍然無法執行。

經過screw plus的保護，即使網站整站被下載或被上傳了php 惡意程式碼，也無法對網站造成損失。
