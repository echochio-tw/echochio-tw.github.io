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
d = lambda klist : ['x' for i in klist if i == 1]
print (d([1,2,1,4]))
--> ['x', 'x']
```

那 else 要如何處理 ? ... 用 map
```
d = lambda klist : list(map(lambda i : 'x' if i == 1 else 'X', klist))
print (d([1,2,1,4]))
--> ['x', 'X', 'x', 'X']
```
看前面 lambda function 用 klist 當 lambda 輸入 

return 是 list -->  list(map(function ,data))

map 的 function 是 lambda i : 'x' if i == 1 else 'X' --> 輸出 'x' 或 'X'

map 的 data 是 klist , 這樣會將 list 內的一個一個丟到前面 function 處理

處理後 map 變 list 就會當成前面 lambda 的 return 

data 這是輸入 -> klist

