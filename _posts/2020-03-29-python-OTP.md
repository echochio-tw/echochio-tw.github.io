---
layout: post
title: python 實現 OTP 認證執行外部 exe
date: 2020-03-29
tags: python opt google authenticator
---

所有程式碼在這邊

https://github.com/echochio-tw/python-opt

主程式

https://github.com/echochio-tw/python-opt/blob/master/otp.exe

看 認證碼 ..

https://github.com/echochio-tw/python-opt/blob/master/otp_out.exe

程式就是

otp_out.py --> otp_out.exe

otp.py --> otp.exe

hello.py(這個太簡單了沒放)

執行 otp.exe 要求輸入認證碼

執行 otp_out.exe 取得認證碼  去 otp.exe 那邊輸入

認證碼 正確執行 hello.exe (這個包在 otp.exe 內的...python 呼叫外部程式 .....)

 otp.exe (是有包含 hello.exe 的 , 就是包個 輸入認證碼與檢查認證的外殼)

打包otp.py 的方式(把 hello.exe 放到 C:\Python\hello.exe)

```
pyinstaller -y -F -w --add-data "C:/Python/hello.exe";"."  "C:/Python/otp.py"
```

pyinstaller 指令比較麻煩 auto-py-to-exe 選一選比較簡單 .....
