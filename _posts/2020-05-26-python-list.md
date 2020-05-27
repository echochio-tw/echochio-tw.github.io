---
layout: post
title: python list 妙用
date: 2020-05-26
tags: python list
---

python list 妙用記錄一下

原本將 k 這個 list 當其中元素 符合的放入 d 這個 list 
```
d=[]
for i in k:
    if i.is_displayed():
        d.append(i)
```

改成這樣 ,夠簡單吧 ....
```
d = [i for i in k if i.is_displayed()]
```

還有這樣的 字串處理
```
def count_vowels(s):
    num_vowels = 0
    for char in s:
        if char in 'aeiouAEIOU':
             num_vowels = num_vowels + 1
    return num_vowels
```    

我把它改成這樣一行會不會太誇張了 ....
```
def count_vowels(s): return len([char for char in s if char in 'aeiouAEIOU'])
```

可試試 放入字串呼叫 count_vowels 計算出母音 兩個是相同的結果 ...
```
print (count_vowels('test'))
1
```


這個寫法直接看 True, False ... 然後 SUM 加起來 ... 這也行簡單 ....
```
def count_vowels(s): return sum([char in 'aeiouAEIOU' for char in s])
```

老外老師 教我用  lambda function 這樣更簡單
```
count_vowels = lambda s : len([char for char in s if char in 'aeiouAEIOU'])
```

```
count_vowels = lambda s : sum([char in 'aeiouAEIOU' for char in s])
```
