---
layout: post
title: Django SQLite 3.8.3 or later
date: 2020-08-16
tags: python django
---

Centos 7 

遇到太多次了 ... 寫一下
```
yum groupinstall 'Development Tools'
wget https://www.sqlite.org/2020/sqlite-autoconf-3330000.tar.gz
tar zxvf sqlite-autoconf-3330000.tar.gz
cd sqlite-autoconf-3330000
./configure
make && make install
mv /usr/bin/sqlite3 /usr/bin/sqlite3.bak
cp /usr/local/bin/sqlite3 /usr/bin/sqlite3
echo "/usr/local/lib"> /etc/ld.so.conf.d/sqlite3.conf
ldconfig
```
