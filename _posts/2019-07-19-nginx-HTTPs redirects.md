---
layout: post
title: Forcing HTTPs redirects on non-standard ports
date: 2019-07-19
tags: nginx
---

看到文章不錯留下來

nginx 將流向某port(ex:1234)的http 重新導成https(port1234) 問題

Forcing HTTPs redirects on non-standard ports

Nginx has created a custom HTTP status code to allow you to force a redirect for **anyone browsing a vhost via HTTP to the HTTPs version, called 497**. This is only needed if you're running SSL on a custom port, since otherwise you can use the config shown above for redirecting.

To force the browser to redirect from HTTP to the HTTPs port, do the following.

```
server {
  listen      1234 ssl;
  server_name your.site.tld;
  ssl         on;
  ...
  error_page  497 https://$host:1234$request_uri;
  ...
}
```

當發生 SSL 錯誤時導向到 https

任何瀏覽器用 http 去瀏覽 https 時會發生 ssl 錯誤

這個錯誤碼是 497 錯誤

然後用nginx 的導向將 497 錯誤導向 https 去運作

