---
layout: post
title: python lambda 妙用
date: 2020-05-27
tags: python lambda
---

python 沒 switch 用法 但是有更強的用法
用 dict get 指到 lambda 印出 英文等級 , 找不到的是印出 'E'

```
score = int(input('請輸入分數：'))
level = score // 10
{
    10 : lambda: print('Perfect'),
    9  : lambda: print('A'),
    8  : lambda: print('B'),
    7  : lambda: print('C'),
    6  : lambda: print('D')
}.get(level, lambda: print('E'))()
```

例如這個 print(add(1)(2))  
```
def add(n1):
    def func(n2):
        return n1 + n2
    return func
```

變 lambda
```
add = lambda n1 : lambda n2: n1 + n2
```

配合 if
```
max = lambda m, n: m if m > n else n
```


配合 for loop 例如 (True, False … 然後 SUM 加起來)
```
count_vowels = lambda s : sum([char in 'aeiouAEIOU' for char in s])
```

配合 for loop with if 例如

```
d = lambda klist : [i for i in klist if i.is_displayed() else 'X']
print (d['111','2222','3333'])
```
lambda 很容易配合 if 或是 for loop 完成工作

