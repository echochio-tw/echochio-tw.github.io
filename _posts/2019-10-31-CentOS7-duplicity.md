---
layout: post
title: CentOS7 duplicity backup
date: 2019-10-31
tags: linux backup
---

duplicati 在 linux 備份到 FTP 有問題這 ... 

只好找其他軟體 ... duplicity

```
yum install duplicity lftp
```

這樣就裝起來了 ...
寫個 shell (下面這個不是上線的 shell 下面那個才是)

```
#!/bin/bash
FTP_PASSWORD=*JWK9\$aasdff
export FTP_PASSWORD
duplicity full --progress --no-encryption /backup/zabbix_cfg_db-mysql.sql.gz ftp://admin@192.168.0.111/zabbix-db/
duplicity full --progress --no-encryption /backup/domain_list.sql ftp://admin@192.168.0.111/list/
```
發現備份目錄比較好 ...還原時不必記檔名,檔名不正確無法還原 ... 最後是這方式上線
```
#!/bin/bash
FTP_PASSWORD=*JWK9\$aasdff
export FTP_PASSWORD
duplicity full --progress --no-encryption /backup/data ftp://admin@192.168.0.111/zabbix-db/
```
備份目錄後還原整個目錄 ... 這比較方便 ... 不必記檔案名
```
duplicity -v9 --no-encryption restore --no-encryption ftp://admin@210.209.15.114/FTP/zabbix-db/ /tmp/backup/
```
還原整個目錄內單一檔案... 當然要知道還原的檔名才能還原
```
duplicity -v9 --no-encryption --file-to-restore zabbix_cfg_db-mysql.sql.gz ftp://admin@210.209.15.114/FTP/zabbix-db/ /tmp/zabbix_cfg_db-mysql.sql.gz
```
刪除三天前資料
```
duplicity remove-older-than 3D ftp://admin@192.168.0.111/zabbix-db/
duplicity remove-older-than 3D ftp://admin@192.168.0.111/list/
```
列出備份的狀態,有多少個完整備份及增量備份,備份的時間等等.
```
 duplicity collection-status ftp://admin@192.168.0.111/zabbix-db/
```
恢復文件(備份檔名還原的方法.. 不是備份目錄的)
```
duplicity -v9 restore --no-encryption ftp://admin@192.168.0.111/zabbix-db/ /tmp/zabbix_cfg_db-mysql.sql.gz
```

其他還可 ssh 的 sftp ... 下面是網路找的
```
#!/bin/bash
# set up the GPG private key password
#這裏設置的PASSPHRASE就是你的GPG密鑰的密碼
#PASSPHRASE=xxxxxx
#export PASSPHRASE
#set the ssh key password
#FTP_PASSWORD是你的ssh密鑰的密碼,導入到環境中就不用手動輸入了
FTP_PASSWORD=mypassword
export FTP_PASSWORD
#upload server root ,exclude /proc /sys /tmp to sftp server,and no encryption
#下面是備份根目錄,--exclude表示將 /proc /sys /tmp 除外 ,這裏需要註意的是,duplicity文檔中特別註明了如果你要備份根,那麽必須要把 /proc除外
#下面的 --dry-run表示並沒有實際運行,只是測試,而 --ssh-options後面的是ssh的一些設置,端口號為8888,ssh密鑰的位置.
duplicity -v5 --dry-run --progress   --no-encryption --ssh-options="-oPort=8888 -oIdentityFile=/home/xx/private/id_rsa" --exclude /proc --exclude /sys --exclude /tmp / sftp://mysftp@youserver.locastion/upload/#unset varunset FTP_PASSWORD
```
