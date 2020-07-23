---
layout: post
title: Python urllib
date: 2020-06-20
tags: Python urllib
---

密大作業
```
Actual problem: Start at: http://py4e-data.dr-chuck.net/known_by_Romanie.html
Find the link at position 18 (the first name is 1). Follow that link. Repeat this process 7 times. The answer is the last name that you retrieve.
Hint: The first character of the name of the last page that you will load is: A
```

亂寫一通 ... 老師規定不能用 request 給他 XXX
```
import urllib.request, urllib.parse, urllib.error
from bs4 import BeautifulSoup
import ssl
def f(url,i):
    ctx = ssl.create_default_context()
    ctx.check_hostname = False
    ctx.verify_mode = ssl.CERT_NONE
    html = urllib.request.urlopen(url, context=ctx).read()
    soup = BeautifulSoup(html,'html.parser')
    return [a['href'] for a in soup.find_all('a')][i-1]

url = input('Enter URL : ')
n = int(input('Enter count : '))
i = int(input('Enter position : '))
print ('Retrieving: {}'.format(url))
for x in range(0,n):
    url = f(url,i)
    print ('Retrieving: {}'.format(url))
```

亂寫一通 ...說不給過沒 urllib 挖哩

```
import requests
from bs4 import BeautifulSoup
def f(url,i):
    r = requests.get(url)
    html = BeautifulSoup(r.text,'html.parser')
    return [a['href'] for a in html.find_all('a')][i-1]

url = input('Enter URL : ')
n = int(input('Enter count : '))
i = int(input('Enter position : '))
print ('Retrieving: {}'.format(url))
for x in range(0,n):
    url = f(url,i)
    print ('Retrieving: {}'.format(url))
```
