---
layout: post
title: python 變執行檔 exe ..
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

有 python 在客戶端可用 pyc 就好(例如 linux) 這樣很難反解譯
```
python -m compileall .

python helloword.pyc
```
發現有個好用的工具 auto-py-to-exe
```
pip install auto-py-to-exe
```
然後在桌面設定捷徑 auto-py-to-exe 
(我的在 C:\Python\Scripts\auto-py-to-exe.exe)
點選就可執行
選一選就可生成檔案
