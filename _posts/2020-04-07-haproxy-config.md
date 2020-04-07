---
layout: post
title: haproxy 設定檔
date: 2020-04-07
tags: haproxy
---

紀錄一下
```
global
    log         127.0.0.1 local2
    chroot      /var/lib/haproxy
    pidfile     /var/run/haproxy.pid
    maxconn     40000
    user        haproxy
    group       haproxy
    daemon
    stats socket /var/lib/haproxy/stats

defaults
        log     global
        log 127.0.0.1 local3
        mode    http
        option  tcplog
        option  dontlognull
        retries 10
        option redispatch
        option forwardfor
        maxconn         20000
        timeout connect         10s
        timeout client          1m
        timeout server          1m
        timeout http-keep-alive 10s
        timeout check           10s

listen  port9700
        bind 0.0.0.0:9700
        mode tcp
        balance roundrobin
        server web1 47.12.90.145:9700
        server web2 47.10.12.219:9700
        server web3 182.252.188.197:9700
        server web4 49.233.98.215:9700
        server web5 39.13.33.136:9700
        
listen  port8900
        bind 0.0.0.0:8900
        mode tcp
        balance roundrobin
        server web1 47.12.90.145:8900
        server web2 47.10.12.219:8900
        server web3 182.252.188.197:8900
        server web4 49.233.98.215:8900
        server web5 39.13.33.136:8900

listen stats
        bind 0.0.0.0:1080
        mode http
        option httplog
        maxconn 10
        stats refresh 30s
        stats uri /stats
        stats realm XingCloud\ Haproxy
        stats auth admin4321:KnXFuZ
        stats hide-version
        stats admin if TRUE
```
