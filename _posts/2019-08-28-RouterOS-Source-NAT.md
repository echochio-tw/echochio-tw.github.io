---
layout: post
title: RouterOS WAN IP 對應特定內網 IP
date: 2019-08-28
tags: RouterOS nat
---

WAN IP 是 10.0.0.216 & 10.0.0.217

特定內網 IP 是 192.168.0.4 走 10.0.0.216 出去

內網 IP 是  192.168.0.254/24 走 10.0.0.217 出去

設定如下 :

先設定 WAN , local IP

```
[admin@MikroTik] ip address> add address=10.0.0.216/24 interface=Public
[admin@MikroTik] ip address> add address=10.0.0.217/24 interface=Public
[admin@MikroTik] ip address> add address=192.168.0.254/24 interface=Local
```

default Routing 

```
[admin@MikroTik] ip route> add gateway=10.0.0.1 prefsrc=10.0.0.217
```

設定 目的 NAT
```
[admin@MikroTik] ip firewall nat> add action=dst-nat chain=dstnat dst-address=10.0.0.216/32 to-addresses=192.168.0.4
```

來源 NAT (走 10.0.0.216 出去)
```
[admin@MikroTik] ip firewall nat> add action=src-nat chain=srcnat src-address=192.168.0.4/32 to-addresses=10.0.0.216
```

內網所有機器 NAT (走 10.0.0.217 出去)
```
[admin@MikroTik] ip firewall nat> add action=src-nat chain=srcnat src-address=192.168.0.0/24 to-addresses=10.0.0.217
```

為何 走 10.0.0.217 出去 不必設定 來源 NAT ? 

因為 default 是走  10.0.0.217 (prefsrc=10.0.0.217) ....

應該是這樣吧 ..... XD
