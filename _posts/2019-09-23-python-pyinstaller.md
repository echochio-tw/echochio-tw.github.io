---
layout: post
title: python 變執行檔
date: 2019-09-23
tags: linux windows python
---

這邊簡單紀錄一下 pyinstaller 這工具在 windows 與 linux 都可正常運作

這是把所需的都包在裡面不是真的編譯

```
pip install pyinstaller

pyinstaller -F helloword.py

dist\helloword

輸出 : 
hello word

```

要檔案小一點用 upx 壓縮 (大部分 linux 都會有 upx ... 可不指定也會壓縮)
```
pyinstaller helloword.py --upx-dir=c:\python -y --onefile

pyinstaller helloword.py --upx-dir=/usr/bin -y --onefile
```

Windows 添加  --noconsole 會沒有 dos 窗口輸出

```
pyinstaller -F helloword.py --noconsole
```
