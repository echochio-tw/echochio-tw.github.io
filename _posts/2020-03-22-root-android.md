---
layout: post
title: root 的安卓 adb
date: 2020-03-22
tags: android root
---

修改有 root 的安卓 ... 紀錄一下

```
C:\Users\user>adb devices
List of devices attached
emulator-5554   device
127.0.0.1:5555  offline
```

remout 
```
C:\Users\user>adb -s emulator-5554 shell
root@aosp:/ # su
su
root@aosp:/ # mount -o rw,remount /system
mount -o rw,remount /system
```
