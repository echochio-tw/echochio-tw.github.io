---
layout: post
title: 大陆地区 php server monotor 用 telegram
date: 2019-02-14
tags: linux php server monotor
---

首先要安装 phpservermonotor ... 废话 

https://github.com/phpservermon/phpservermon

看图说故事 ... 装起来

装好后找 api.telegram.org 字眼
```
[root@localhost phpservermon]#grep -r api.telegram.org
 src/includes/functions.inc.php:                 $this->_url = 'https://api.telegram.org/bot'.urlencode($this->_token).
src/includes/functions.inc.php:                 $this->_url = 'https://api.telegram.org/bot'.urlencode($this->_token).'/getMe';
```

将 phpservermonotor 内的 https://api.telegram.org/ 改成 http://223.27.XXX.XX:8880
```
[root@localhost phpservermon]# grep -r 223.27.XXX.XX
src/includes/functions.inc.php:                 $this->_url = 'http://223.27.XXX.XX:8880/bot'.urlencode($this->_token).
src/includes/functions.inc.php:                 $this->_url = 'http://223.27.XXX.XX:8880/bot'.urlencode($this->_token).'/getMe';
```

在那台 223.27.XXX.XX 开 nginx 反向代理 .....
223.27.XXX.XX (这台在台湾 )

```
worker_processes 1;
error_log /var/log/nginx/error.log;
events {
    worker_connections 1024;
}
http {
        access_log  /var/log/nginx/access.log;
        resolver 8.8.8.8 ipv6=off;
    server {
        listen 8880;
        location ~* ^/bot {
        proxy_buffering off;
            proxy_pass https://api.telegram.org$request_uri;
        }
    }
}
```

开网页测试 代理 .... 
```
http://207.148.73.58:16888/bot61011111:AAGBzrJ5dYipW0DB97mgXkdYFB6Rss11ss/getUpdates
```

去 phpservermonitor 网页测试 telegram ....
