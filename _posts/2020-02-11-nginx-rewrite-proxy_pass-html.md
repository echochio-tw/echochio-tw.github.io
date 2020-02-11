---
layout: post
title: nginx rewrite proxy_pass html
date: 2020-02-11
tags: nginx rewrite proxy_pass
---

就是想用 nginx 做 rewrite & proxy_pass 的動作 

```
server {
        listen 80;
        server_name haha.vip;

        charset utf-8;
        access_log  /home/wwwlogs/haha.vip.access.log;
        location = / {
            rewrite ^ /zzz/tgt.html permanent;
        }
        location / {
            proxy_pass http://g.gang.com:80;
        }
}
```

這是會將 
```
http://haha.vip
```

轉到
```
http://haha.vip/zzz/tgt.html
```

加上
```
proxy_pass http://g.gang.com:80;
```

所以是
```
http://g.gang.com/zzz/tgt.html
```
的畫面

但是 404 還沒有試出來
