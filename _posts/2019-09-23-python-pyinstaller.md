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


使用nuitka打包python程序 ...
```
這個一要配合 mingw-w64
然後在 先執行 mingw-w64.bat 下面

C:\>MinGW64\mingw-w64.bat

C:\>echo off
Microsoft Windows [版本 10.0.18363.752]
(c) 2019 Microsoft Corporation. 著作權所有，並保留一切權利。

C:\>cd Python

C:\test> nuitka --windows-disable-console --recurse-all UBgram-OSS.py
Nuitka:WARNING:Use '--plugin-enable=tk-inter' for: Tkinter needs TCL included.
Nuitka:WARNING:Unresolved '__import__' call at 'C:\Python\lib\site-packages\cffi\verifier.py:151' may require use of '--include-plugin-directory' or '--include-plugin-files'.
Nuitka:WARNING:Unresolved '__import__' call at 'C:\Python\lib\site-packages\requests\packages.py:7' may require use of '--include-plugin-directory' or '--include-plugin-files'.
Nuitka will make use of Dependency Walker (http://dependencywalker.com) tool
to analyze the dependencies of Python extension modules. Is it OK to download
and put it in "C:\Users\chio\AppData\Local\Nuitka\Nuitka".
No installer needed, cached, one time question.

Proceed and download? [Yes]/No
Yes
Nuitka:INFO:Downloading 'http://dependencywalker.com/depends22_x86.zip'
Nuitka:INFO:Extracting to 'C:\Users\echochio\AppData\Local\Nuitka\Nuitka\x86\depends.exe'

```

Shed Skin - A (restricted) Python-to-C++ Compiler 這個很久沒更新了支援 2.X 的 python
```
http://shed-skin.blogspot.com/
```


