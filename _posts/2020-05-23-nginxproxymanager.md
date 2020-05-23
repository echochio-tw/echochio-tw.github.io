---
layout: post
title: nginx proxy manager
date: 2020-05-23
tags: nginx proxy
---

https://nginxproxymanager.com/guide/#project-goal

<img src="/images/posts/nginxproxymanager/a.png">


配合資料庫 與docker 

```
yum install epel-release
yum install docker-ce docker-ce-cli
systemctl disable firewalld
systemctl stop firewalld
systemctl enable --now docker
systemctl start docker.service
yum install epel-release
yum install python3 git
pip install docker-compose
mkdir data
mkdir letsencrypt
```
建立 config.json
```
{
  "database": {
    "engine": "mysql",
    "host": "db",
    "name": "npm",
    "user": "npm",
    "password": "npm",
    "port": 3306
  }
}

```
建立 docker-compose.yml
```
version: "3.8"
services:
  app:
    image: jc21/nginx-proxy-manager:2
    restart: always
    ports:
      # Public HTTP Port:
      - '80:80'
      # Public HTTPS Port:
      - '443:443'
      # Admin Web Port:
      - '81:81'
    environment:
      # Uncomment this if IPv6 is not enabled on your host
      # DISABLE_IPV6: 'true'
    volumes:
      # Make sure this config.json file exists as per instructions above:
      - ./config.json:/app/config/production.json
      - ./data:/data
      - ./letsencrypt:/etc/letsencrypt
    depends_on:
      - db
  db:
    image: jc21/mariadb-aria:10.4
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: 'npm'
      MYSQL_DATABASE: 'npm'
      MYSQL_USER: 'npm'
      MYSQL_PASSWORD: 'npm'
    volumes:
      - ./data/mysql:/var/lib/mysql
```

啟動
```
 docker-compose up -d
```

可能用到的指令
```
docker ps -a
docker logs d0c6e590acdc
docker exec -it d0c6e590acdc bash
mysql
show databases;
use npm;
show tables;
```

登入
```
http://your-ip:81/

admin@example.com / changeme
```
