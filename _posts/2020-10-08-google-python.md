---
layout: post
title: google python 網管雜亂記
date: 2020-10-08
tags: python google
---

google python 網管做個紀錄

network.py
```
#!/usr/bin/env python3
import requests
import socket
def check_localhost():
    localhost = socket.gethostbyname('localhost')
    return localhost == "127.0.0.1"
def check_connectivity():
    localhost = socket.gethostbyname('localhost')
    request = requests.get("http://www.google.com")
    return request.status_code == 200
```

helth_check.py
```
#!/usr/bin/env python3
import shutil
import psutil
from network import *
def check_disk_usage(disk):
    """Verifies that there's enough free space on disk"""
    du = shutil.disk_usage(disk)
    free = du.free / du.total * 100
    return free > 20
def check_cpu_usage():
    """Verifies that there's enough unused CPU"""
    usage = psutil.cpu_percent(1)
    return usage < 75
# If there's not enough disk, or not enough CPU, print an error
if not check_disk_usage('/') or not check_cpu_usage():
    print("ERROR!")
elif check_localhost() and check_connectivity():
    print("Everything ok")
else:
    print("Network ERROR!")
```
