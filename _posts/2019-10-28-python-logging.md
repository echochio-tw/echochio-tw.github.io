---
layout: post
title: python logging
date: 2019-10-28
tags: python
---

寫了一大堆 python 常用 log + wintail 去看執行的資訊 紀錄一下

```
import logging
logging.basicConfig(filename='D:/run.log',level=logging.INFO,format='%(asctime)s - %(name)s - %(levelname)s - %(message)s')
logging.info('finish multi sim bix1 close')
```
