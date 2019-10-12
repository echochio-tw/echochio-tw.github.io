---
layout: post
title: centos7 esxi 移 vworkstation 出現 dracut-initqueue timeout
date: 2019-08-23
tags: centos esxi vm
---
centos 7 移机 用 vm vConverter 将 Esxi 移到 vmdk 拿到 vworkstation 開出現
dracut-initqueue timeout

centos 7 移机出現 dracut-initqueue timeout 都可用

```
掛載 centos7-minimal-xx 之類的iso
選 Troubleshooting
選 Rescuse a CentOS Linux system
選 Continue (1)

```
啟動網路及 chroot ....

```
dhclient
chroot /mnt/sysimage
```
更新及判斷硬體 (要看 grub 在哪顆硬碟 ... 正常是 sda
```
yum update -y
grub2-mkconfig -o /boot/grub2/grub.cfg
grub2-install /dev/sda
```
如需要改密碼這時改一下
```
passwd root
```
重開 ... 移除光碟
```
exit
reboot
```
