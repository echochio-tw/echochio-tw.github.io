---
layout: post
title: linux 本機錄封包
date: 2020-04-28
tags: linux network ngrep
---

抓封包 .... 

抓一般封包沒問題  ... 抓 https ....

yum 裝 epel 後可裝 
```
yum install ngrep 
```

裝 nginx 與設定反向代理
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
        listen 5000;
        location ~* ^/bot {
        proxy_buffering off;
            proxy_pass https://api.telegram.org$request_uri;
        }
    }
}

```

這邊用 go 寫一段程式

```
package main
import (
    "net/http"
    "encoding/json"
    "fmt"
    "bytes"
)

func main() {
    request_url := "https://api.telegram.org/bot610418721:AAGBzrJ5dYipW0DB9799999YFB6RDwM8x7s/sendMessage"
    client := &http.Client{}

    values := make(map[string]interface{})
    values["chat_id"] = "-390234700"
    values["text"] = "This is a test"
    values["disable_notification"] =  true

    jsonStr, _ := json.Marshal(values)
    req, _ := http.NewRequest("POST", request_url, bytes.NewBuffer(jsonStr))
    req.Header.Set("content-type", "application/json")

    res, err := client.Do(req)
    if(err != nil){
        fmt.Println(err)
    } else {
        fmt.Println(res.Status)
    }
}

```

 ngrep -d any port 5000
 ```
 ####
T 127.0.0.1:57592 -> 127.0.0.1:5000 [AP] #4
  POST /bot610418722:ABGBdr9sdYipS0DB97mgXkdYFB6RDwM8x7s/sendMessage HTTP/1.1..Host: 127.0.0.1:5000..User-Agent: Go-http-client/1.1..Content-Length: 76..Content-Type: application/json..Accept-Encoding: gzip....{"chat_id":"-390234000","disable_notification":true,"text":"This is a test"}
##
T 127.0.0.1:5000 -> 127.0.0.1:57592 [AP] #6
  HTTP/1.1 200 OK..Server: nginx/1.14.1..Date: Tue, 28 Apr 2020 13:21:36 GMT..Content-Type: application/json..Content-Length: 295..Connection: keep-alive..Strict-Transport-Security: max-age=31536000; includeSubDomains; preload..Access-Control-Allow-Origin: *..Access-Control-Allow-Methods: GET, POST, OPTIONS..Access-Control-Expose-Headers: Content-Length,Content-Type,Date,Server,Connection....{"ok":true,"result":{"message_id":50597,"from":{"id":610418722,"is_bot":true,"first_name":"BOT","username":"Massagebbbot"},"chat":{"id":-390234712,"title":"test_chart","type":"group","all_members_are_administrators":true},"date":1588080096,"text":"This is a test"}}
####
```
