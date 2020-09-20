---
layout: post
title: whatsapp yowsup
date: 2020-09-20
tags: whatsapp
---

要用 whatsapp to telegram 没完成先做 yowsup

```
git clone https://github.com/tgalal/yowsup.git
cd yowsup.git
python setup.py install
```

```
find / -name env_android.py

/usr/local/lib/python3.6/site-packages/yowsup/env/env_android.py
/root/yowsup/build/lib/yowsup/env/env_android.py
/root/yowsup/yowsup/env/env_android.py
```

```
改 /usr/local/lib/python3.6/site-packages/yowsup/env/env_android.py
```
改 MD5_CLASSES , _VERSION
```
#   _MD5_CLASSES = "bdZVQh58MKZaaeiYnh7wkw=="
    _MD5_CLASSES = "aIiTbnlq1G2jxLKYgZI5Xw=="
    _KEY = "eQV5aq/Cg63Gsq1sshN9T3gh+UUp0wIw0xgHYT1bnCjEqOJQKCRrWxdAe2yvsDeCJL+Y4G3PRD2HUF7oUgiGo8vGlNJOaux26k+A2F3hj8A="

#   _VERSION = "2.19.51"
    _VERSION = "2.20.108"
    _OS_NAME = "Android"
    _OS_VERSION = "8.0.0"
    _DEVICE_NAME = "star2lte"
    _MANUFACTURER = "samsung"
    _BUILD_VERSION = "star2ltexx-user 8.0.0 R16NW G965FXXU1ARCC release-keys"
    _AXOLOTL = True
```
送簡訊
```
yowsup-cli registration --requestcode sms --config-phone 886931000000 --config-cc 886 --config-mcc 466 --config-mnc 89
```
確認簡訊(收到 123-459)
```
yowsup-cli registration --config-cc 886 --config-phone 886931000000 --register 123-459
```
顯示
```
yowsup-cli  v3.2.0
yowsup      v3.2.3

Copyright (c) 2012-2019 Tarek Galal
http://www.openwhatsapp.org

This software is provided free of charge. Copying and redistribution is
encouraged.

If you appreciate this software and you would like to support future
development please consider donating:
http://openwhatsapp.org/yowsup/donate


I 2020-09-20 08:28:07,539 yowsup.common.http.warequest - b'{"status":"ok","login":"886931000000","type":"existing","edge_routing_info":"CAIIDA==","security_code_set":false}\n'
{
    "__version__": 1,
    "cc": "886",
    "client_static_keypair": "qJIcKm0FwXaK84/AmPHLmXz0pMpByasdxO6aUfrqS0wQ9+DOWGTcSxcVzRn+v6+NP2chBXBFVYkqTVHvwPDdbA==",
    "edge_routing_info": "CAIIDA==",
    "expid": "WR5aJ9j0QaaaTxGdkasI9Q==",
    "fdid": "6ff70cb3-032c-4ade-8add-553eabd81ed2",
    "id": "3lXN2qija5gfeJdId8Ol0LRtZoc=",
    "mcc": "466",
    "mnc": "89",
    "phone": "886931000000",
    "sim_mcc": "000",
    "sim_mnc": "000"
}
status: b'ok'
login: b'886931000000'
type: b'existing'
edge_routing_info: b'CAIISA=='
```

連上去 ....
```
yowsup-cli demos -y -c ./.config/yowsup/886931000000/config.json
```

```
yowsup-cli  v3.2.0
yowsup      v3.2.3

Copyright (c) 2012-2019 Tarek Galal
http://www.openwhatsapp.org

This software is provided free of charge. Copying and redistribution is
encouraged.

If you appreciate this software and you would like to support future
development please consider donating:
http://openwhatsapp.org/yowsup/donate


Yowsup Cli client
==================
Type /help for available commands

[offline]:/L
I 2020-09-20 08:36:32,210 yowsup.layers.network.layer - Connecting to e7.whatsapp.net:443
I 2020-09-20 08:36:32,418 yowsup.axolotl.manager - Generating 812 prekeys
I 2020-09-20 08:36:32,857 yowsup.axolotl.manager - Storing 812 prekeys
I 2020-09-20 08:36:34,030 yowsup.axolotl.manager - Loaded 812 unsent prekeys
general: Disconnected:
[offline]:
[offline]:

```

再來將 他連到 telegram 請參考
```
https://ibcomputing.com/setting-up-whatsapp-telegram-bridge-using-matterbridge-2020/
```
