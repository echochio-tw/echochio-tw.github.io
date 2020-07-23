---
layout: post
title: Python xml
date: 2020-06-20
tags: Python xml
---

密大作業 ... 計算xml 內 count 總和
```
In this assignment you will write a Python program somewhat similar to http://www.py4e.com/code3/geoxml.py. ......
.....
Sample data: http://py4e-data.dr-chuck.net/comments_42.xml (Sum=2553)
Actual data: http://py4e-data.dr-chuck.net/comments_694850.xml (Sum ends with 3)
```

亂寫一通 ... 老師規定要用 urllib
```
import urllib.request, urllib.parse, urllib.error
import xml.etree.ElementTree as ET
import ssl

ctx = ssl.create_default_context()
ctx.check_hostname = False
ctx.verify_mode = ssl.CERT_NONE

url = 'http://py4e-data.dr-chuck.net/comments_694850.xml'
html = urllib.request.urlopen(url, context=ctx).read()
tree = ET.fromstring(html)
a = sum([int(i.find('count').text) for i in tree.findall('comments')[0]])
print (a)
```
