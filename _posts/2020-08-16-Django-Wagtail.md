---
layout: post
title: Django Wagtail
date: 2020-08-16
tags: python django Wagtail
---

Centos 7 裝 Wagtail


裝 SQLlite
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

裝 python3
```
yum install python3 python3-devel
```

```
adduser wagtail
su - wagtail
python3 -m venv app
cd app
source bin/activate
pip install wagtail
wagtail start mysite
cd mysite
pip install -r requirements.txt
python manage.py migrate
python manage.py createsuperuser
python manage.py runserver 0.0.0.0:8000

```
