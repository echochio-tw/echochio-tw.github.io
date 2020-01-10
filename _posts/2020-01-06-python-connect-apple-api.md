---
layout: post
title: python connect apple api
date: 2020-01-06
tags: 圖
---

php 用這個
```
https://github.com/echochio-tw/appstore-connect-api
```

python 用 appstoreconnect 詳細還在研究

```
from appstoreconnect import Api
# api = Api(key_id, path_to_key_file, issuer_id)
api = Api("X2SWxxxass", "/root/ios/AuthKey_X2SWCDZH8T.p8", "c307d2f0-dab0-4339-8f27-77ef29cfffff")
apps = api.list_devices()
for app in apps:
    print(app)
```
print udid
```
from appstoreconnect import Api
api = Api("X2SWxxxass", "/root/ios/AuthKey_X2SWCDZH8T.p8", "c307d2f0-dab0-4339-8f27-77ef29cfffff")
apps = api.list_devices()
for app in apps:
    print('{:41s} --> {:>10s} --> {:>10s}'.format(vars(app)['_data']['attributes']['udid'],vars(app)['_data']['attributes']['model'],vars(app)['_data']['attributes']['name']))

```
