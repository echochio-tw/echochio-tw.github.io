---
layout: post
title: python perl file to list 
date: 2020-04-05
tags: python perl file list 
---

python list to file
```
dataout=['hi','hello','welcome'] 
with open('test.txt') as f: f.write('\n'.join(dataout))
```

python file to list
```
datainput = [line.strip() for line in open("test.txt", 'r')]
```

perl file to list
```
@datainput = `cat test.txt`;
```

python list len
```
len(datainput)
```

perl list len
```
$datainputSize= @datainput;
```
