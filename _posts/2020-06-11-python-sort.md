---
layout: post
title: python 的 sort
date: 2020-06-11
tags: python dict sort
---

在密大的作業 ... 有點深度留起來(都要用一行解決)

Write code to switch the order of the winners list so that it is now Z to A, by first name. Assign this list to the variable z_winners.
```
winners = ['Alice Munro', 'Alvin E. Roth', 'Kazuo Ishiguro', 'Malala Yousafzai', 'Rainer Weiss', 'Youyou Tu']
z_winners = sorted(winners, reverse=True)
```

Write code to switch the order of the winners list so that it is now A to Z by last name. Assign this list to the variable z_winners.
```
winners = ['Alice Munro', 'Alvin E. Roth', 'Kazuo Ishiguro', 'Malala Yousafzai', 'Rainer Weiss', 'Youyou Tu']
z_winners = sorted(winners, key = lambda x : x.split()[-1])
```

Given the same dictionary, medals, now sort by the medal count. Save the three countries with the highest medal count to the list, top_three.
```
medals = {'Japan':41, 'Russia':56, 'South Korea':21, 'United States':121, 'Germany':42, 'China':70}
top_three = [i for i in sorted(medals, key=medals.get, reverse=True)][:3]
```

Sort the list ids by the last four digits of each id. Do this using lambda and not using a defined function. Save this sorted list in the variable sorted_id.
```
ids = [17573005, 17572342, 17579000, 17570002, 17572345, 17579329]
sorted_id = [i for i in sorted(ids, key =lambda x :str(x)[-4:] ) ]
```

Sort the following list by each element’s second letter a to z. Do so by using lambda. Assign the resulting value to the variable lambda_sort.
```
ex_lst = ['hi', 'how are you', 'bye', 'apple', 'zebra', 'dance']
lambda_sort=[i for i in sorted(ex_lst , key = lambda x: x[1])]
```
