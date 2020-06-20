---
layout: post
title: Python Regular	Expression	Quick	Guide
date: 2020-06-20
tags: Python Regular
---

紀錄一下 ....
```
Python	Regular	Expression	Quick	Guide
^ Matches the beginning of a line  匹配行首
$ Matches the end of the line 匹配行尾
. Matches any character 匹配任何字符
\s Matches whitespace 匹配空白
\S Matches any non-whitespace character 匹配任何非空白字符
* Repeats a character zero or more times 重複一個字符零次或更多次
*? Repeats a character zero or more times (non-greedy) 重複一個字符零次或更多次(non-greedy)
+ Repeats a character one or more times 重複一個或多個字符
+? Repeats a character one or more times (non-greedy) 重複一個或多個字符(non-greedy)
[aeiou] Matches a single character in the listed set 匹配列表集中的單個字符
[^XYZ] Matches a single character not in the listed set 匹配不在列表集中的單個字符
[a-z0-9] The set of characters can include a range 字符集可以包含一個範圍
( Indicates where string extraction is to start 指示從何處開始提取字符串
) Indicates where string extraction is to end 指示字符串提取將在何處結束
```

https://docs.python.org/3/howto/regex.html
