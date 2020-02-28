---
layout: post
title: nginx proxy client IP 
date: 2020-02-28
tags: nginx rewrite proxy_pass client IP 
---

就是想用 nginx 做 proxy_pass 的動作 傳送 client IP 到後面

```
server {
    listen       443 ssl http2;
    server_name  ios.port.net;
    ssl_certificate    /etc/nginx/fullchain.pem;
    ssl_certificate_key /etc/nginx/privkey.pem;
    ssl_session_cache shared:SSL:50m;
    ssl_session_timeout 5m;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-DSS-AES128-GCM-SHA256:kEDH+AESGCM:ECDHE-RSA-AES128-SHA256:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA:ECDHE-ECDSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES128-SHA:DHE-DSS-AES128-SHA256:DHE-RSA-AES256-SHA256:DHE-DSS-AES256-SHA:DHE-RSA-AES256-SHA:ECDHE-RSA-DES-CBC3-SHA:ECDHE-ECDSA-DES-CBC3-SHA:AES128-GCM-SHA256:AES256-GCM-SHA384:AES128-SHA256:AES256-SHA256:AES128-SHA:AES256-SHA:AES:CAMELLIA:DES-CBC3-SHA:!aNULL:!eNULL:!EXPORT:!DES:!RC4:!MD5:!PSK:!aECDH:!EDH-DSS-DES-CBC3-SHA:!EDH-RSA-DES-CBC3-SHA:!KRB5-DES-CBC3-SHA;
    ssl_prefer_server_ciphers on;
    ssl_dhparam /etc/nginx/ssl/dhparam.pem;
    resolver 1.1.1.1;
    access_log  /var/log/nginx/logs/ios.access.log;
    error_log   /var/log/nginx/logs/ios.error.log;
    client_max_body_size 1000M;
    proxy_connect_timeout       600;
    proxy_send_timeout          600;
    proxy_read_timeout          600;
    send_timeout                600;
    real_ip_header X-Forwarded-For;

    location / {
    real_ip_header    X-Forwarded-For;
    real_ip_recursive on;
    proxy_pass http://backend;
    client_max_body_size 1000M;
    proxy_connect_timeout       600;
    proxy_send_timeout          600;
    proxy_read_timeout          600;
    send_timeout                600;
    proxy_max_temp_file_size 0;
    proxy_next_upstream error timeout invalid_header http_500 http_502 http_503 http_504;
    proxy_redirect off;
    proxy_buffering off;
    proxy_set_header Host      $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header        X-Forwarded-For $proxy_add_x_forwarded_for;
}
}

```

backend 這台
```
server {
    listen       80;
    access_log  /var/log/nginx/logs/ios.access.log;
    error_log   /var/log/nginx/logs/ios.error.log;
    client_max_body_size 1000M;
    proxy_connect_timeout       600;
    proxy_send_timeout          600;
    proxy_read_timeout          600;
    send_timeout                600;
    root  /www/wwwroot/ios.zxreport.net/public;
    index  index.php index.html;
    set_real_ip_from 183.60.110.100/32;
    real_ip_header X-Forwarded-For;
    location / {
            client_max_body_size 1000M;
            proxy_connect_timeout       600;
            proxy_send_timeout          600;
            proxy_read_timeout          600;
            send_timeout                600;
            if (!-e $request_filename){
            rewrite "^/([a-zA-Z0-9]{6})$" /user/install/index/$1/ last;
            rewrite  ^(.*)$  /index.php?s=$1  last;   break;
        }

    location ~ \.php$ {
        try_files $uri =404;
        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        fastcgi_pass   127.0.0.1:9000;
        fastcgi_index  index.php;
        fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
        fastcgi_param  SCRIPT_NAME   $fastcgi_script_name;
        fastcgi_connect_timeout 600;
        fastcgi_read_timeout 600;
        fastcgi_send_timeout 600;
        include        fastcgi_params;
     }
}
}

```
