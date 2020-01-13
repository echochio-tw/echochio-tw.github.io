---
layout: post
title: python include php config file
date: 2020-01-13
tags: python php
---

紀錄一下下次應該還會用到 

python 呼叫 php 的 database.php

/var/www/html/conf/database.php  內容是 :
```
<?php
/**
 * 配置文件
 */

return [
    // 數據庫類型
    'type'     => 'mysql',
    // 服務器地址
    'hostname' => '127.0.0.1',
    // 數據庫名
    'database' => 'cj',
    // 用户名
    'username' => 'root',
    // 密碼
    'password' => '@WSX!QAZ',
    // 端口
    'hostport' => '3306',
    // 數據庫編碼默認採用utf8
    'charset'  => 'utf8mb4',
    // 數據庫表預前缀
    'prefix'   => 'cmf_',
    "authcode" => 'JL5rPyAVz3wy8rKq3F',
    //#COOKIE_PREFIX#
];

```
python 
```
import subprocess
import json
import pymysql
import time
do = "php -r '$array = include(" + '"/var/www/html/conf/database.php"' + ");echo(json_encode($array));'"
proc = subprocess.Popen(do, shell=True, stdout=subprocess.PIPE)
script_response = proc.stdout.read()
my_json = script_response.decode('utf8').replace("'", '"')
d = json.loads(my_json)
print (d)
```
輸出
```
{'type': 'mysql', 'hostname': '127.0.0.1', 'database': 'cj', 'username': 'root', 'password': '@WSX!QAZ', 'hostport': '3306', 'charset': 'utf8mb4', 'prefix': 'cmf_', 'authcode': 'JL5rPyAVz3wy8rKq3F'}
```
套到 sql
```
db = pymysql.connect(host=d['hostname'],
                     port=int(d['hostport']),
                     user=d['username'],
                     password=d['password'],
                     charset=d['charset'],
                     db=d['database'],
                     cursorclass=pymysql.cursors.DictCursor)
cursor = db.cursor()
sql = "SELECT * FROM `cmf_super_signature_ipa`"
cursor.execute(sql)
records = cursor.fetchall()
print (records)
db.close()
```


