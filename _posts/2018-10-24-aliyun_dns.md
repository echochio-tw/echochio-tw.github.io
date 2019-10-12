---
layout: post
title: 阿里云 DNS 換 IP
date: 2018-10-24
tags: DNS
---

安装阿里云SDK前请先安装以下依赖包：
```
python-pip
gcc
gcc-devel
autoconf
automake
python-devel
python-pip
python-crypto
python-cryptography 
```

#centos可使用以下命令进行安装：
```
yum install python-pip gcc gcc-devel autoconf automake python-devel python-pip python-crypto python-cryptography -y
```
python2 运行此脚本，脚本所需模块如下：
```
json
os
re
sys
datetime
requests
aliyun-python-sdk-core-v3
aliyun-python-sdk-alidns
```

准备的账户信息如下：
```
Access Key ID
Access Key Secret
账号ID
i_dont_know_record_id设为no并运行脚本。`
```

程式如下
```
#!/usr/bin/env python
# encoding: utf-8

import json
import os
import re
import sys
from datetime import datetime

from aliyunsdkalidns.request.v20150109 import UpdateDomainRecordRequest, DescribeDomainRecordsRequest, \
    DescribeDomainRecordInfoRequest
from aliyunsdkcore import client

#请填写你的Access Key ID
access_key_id = "KXXXXXXXXXXXXXXXXK\"

#请填写你的Access Key Secret
access_key_secret = "KXXXXXXXXXXXXXXXX"

#请填写你的账号ID
account_id = "maintain"

#请填写解析记录ID
#rc_record_id = '410XXXXXXXXX'

#请填写你的一级域名
rc_domain = 'presseco.com'

#请填写你的解析记录,对应的主机记录
#rc_rr = 'me'

#请填写你的记录类型，DDNS请填写A，表示A记录
#rc_type = 'A'
#rc_type = ‘CNAME’

#请填写解析有效生存时间TTL，单位：秒
#rc_ttl = '1'
rc_ttl = '600'

#请填写返还内容格式，json，xml
rc_format = 'json'

def my_ip():
    get_ip_value =  '192.168.0.1'
    return get_ip_value

def check_records(dns_domain):
    clt = client.AcsClient(access_key_id, access_key_secret, 'cn-hangzhou')
    request = DescribeDomainRecordsRequest.DescribeDomainRecordsRequest()
    request.set_DomainName(dns_domain)
    request.set_accept_format(rc_format)
    #result = clt.do_action(request)
    result = clt.do_action_with_exception(request)
    return result

def old_ip():
    clt = client.AcsClient(access_key_id, access_key_secret, 'cn-hangzhou')
    request = DescribeDomainRecordInfoRequest.DescribeDomainRecordInfoRequest()
    request.set_RecordId(rc_record_id)
    request.set_accept_format(rc_format)
    result = clt.do_action_with_exception(request).decode()
    result = json.JSONDecoder().decode(result)
    result = result['Value']
    return result

def update_dns(dns_rr, dns_type, dns_value, dns_record_id, dns_ttl, dns_format):
    clt = client.AcsClient(access_key_id, access_key_secret, 'cn-hangzhou')
    request = UpdateDomainRecordRequest.UpdateDomainRecordRequest()
    request.set_RR(dns_rr)
    request.set_Type(dns_type)
    request.set_Value(dns_value)
    request.set_RecordId(dns_record_id)
    request.set_TTL(dns_ttl)
    request.set_accept_format(dns_format)
    #result = clt.do_action(request)
    result = clt.do_action_with_exception(request)
    return result

if len(sys.argv) < 4:
    print "Usage:", sys.argv[0], "<subdomain> <type> <NEW-IP>"
    sys.exit(1)

rc_rr = sys.argv[1]
rc_type =sys.argv[2]
rc_value = sys.argv[3]
data = json.loads(check_records(rc_domain))
lst = data['DomainRecords']['Record']
for i in lst:
    print i['RR']," ==> ",i['RecordId']
    if rc_rr == i['RR']:
        rc_record_id = i['RecordId']

print "get ",rc_rr," ==> ",rc_record_id
rc_value_old = old_ip()
print "OLD IP : ", rc_value_old
print "NEW IP : ", rc_value
if rc_value_old == rc_value:
    print 'The specified value of parameter Value is the same as old'
else:
    print update_dns(rc_rr, rc_type, rc_value, rc_record_id, rc_ttl, rc_format)


```


执行结果 :
```
[root@S70 ~]# python aliyun_ddns.py
Usage: aliyun_ddns.py <subdomain> <type> <NEW-IP>

[root@S70 ~]# python aliyun_ddns.py sh A 106.106.106.106
sh  ==>  KXXXXXXXXXXXXXXXX
sms  ==>  KXXXXXXXXXXXXXXXX
pay  ==>  KXXXXXXXXXXXXXXXX
get  sh  ==>  KXXXXXXXXXXXXXXXX
OLD IP :  106.106.106.106
NEW IP :  106.106.106.106
The specified value of parameter Value is the same as old

[root@S70 ~]# python aliyun_ddns.py sh A 192.168.111.1
sh  ==>  KXXXXXXXXXXXXXXXX
sms  ==>  KXXXXXXXXXXXXXXXX
pay  ==>  KXXXXXXXXXXXXXXXX
get  sh  ==>  KXXXXXXXXXXXXXXXX
OLD IP :  106.106.106.106
NEW IP :  192.168.111.1
{"RecordId":"KXXXXXXXXXXXXXXXX","RequestId":"6F521E1E-FD6E-413B-XXXX-XXXXXXXX"}

[root@S70 ~]# python aliyun_ddns.py sh A 106.106.106.106
sh  ==>  4107268733097984
sms  ==>  4095910775101440
pay  ==>  4095910449387520
get  sh  ==>  4107268733097984
OLD IP :  192.168.111.1
NEW IP :  106.106.106.106
{"RecordId":"4107268733097984","RequestId":"A05D2BB5-9629-4F15-XXXX-XXXXXXXX"}
```
