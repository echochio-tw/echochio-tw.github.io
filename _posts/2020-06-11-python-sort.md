---
layout: post
title: python 的 sort
date: 2020-06-11
tags: python dict sort
---

我在密大的作業 ... 有點深度留起來(都要用一行解決)

編寫代碼以切換獲winners 的順序，以first name現在從Z變為A。將此列表分配給變量z_winners。

```
winners = ['Alice Munro', 'Alvin E. Roth', 'Kazuo Ishiguro', 'Malala Yousafzai', 'Rainer Weiss', 'Youyou Tu']
z_winners = sorted(winners, reverse=True)
```

編寫代碼以切換獲獎者名單的順序，以last name從A到Z。 將此列表分配給變量z_winners。
```
winners = ['Alice Munro', 'Alvin E. Roth', 'Kazuo Ishiguro', 'Malala Yousafzai', 'Rainer Weiss', 'Youyou Tu']
z_winners = sorted(winners, key = lambda x : x.split()[-1])
```

給定相同的dictionary，獎牌現在按獎 sort by the medal count。 Save the three countries with the highest medal count to the list, top_three.
```
medals = {'Japan':41, 'Russia':56, 'South Korea':21, 'United States':121, 'Germany':42, 'China':70}
top_three = [i for i in sorted(medals, key=medals.get, reverse=True)][:3]
```
按每個ID的後四位數對列表ID進行排序。 使用lambda而不是使用已定義的函數來執行此操作。 將此排序列表保存在變量sorted_id中。
```
ids = [17573005, 17572342, 17579000, 17570002, 17572345, 17579329]
sorted_id = [i for i in sorted(ids, key =lambda x :str(x)[-4:] ) ]
```

將以下列表按每個元素的第二個字母a到z排序。 通過使用lambda。 將結果值分配給變量lambda_sort。
```
ex_lst = ['hi', 'how are you', 'bye', 'apple', 'zebra', 'dance']
lambda_sort=[i for i in sorted(ex_lst , key = lambda x: x[1])]
```
