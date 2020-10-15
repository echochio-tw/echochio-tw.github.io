---
layout: post
title: mssql truncate log
date: 2019-09-25
tags: mssql 
---

<img src="/images/posts/mssqtruncate/1.png">

<img src="/images/posts/mssqtruncate/2.png">

<img src="/images/posts/mssqtruncate/3.png">

<img src="/images/posts/mssqtruncate/4.png">

<img src="/images/posts/mssqtruncate/5.png">

<img src="/images/posts/mssqtruncate/6.png">

附上 T-SQL script 请将 <DATABASE-NAME> 换成您的 DB 名称
```
USE [master]  
GO  
ALTER DATABASE <DATABASE-NAME> SET RECOVERY SIMPLE WITH NO_WAIT  
GO  
ALTER DATABASE <DATABASE-NAME> SET RECOVERY SIMPLE 
GO  
USE <DATABASE-NAME>  
GO  
DBCC SHRINKFILE (N'<DATABASE-NAME>_log' , 2, TRUNCATEONLY)  
GO  
USE [master]  
GO  
ALTER DATABASE <DATABASE-NAME> SET RECOVERY FULL WITH NO_WAIT  
GO  
ALTER DATABASE <DATABASE-NAME> SET RECOVERY FULL
GO
```
  
在做之前可發telegram 通知 ,每做完 TRUNCATE DB log 就該 DB log 做完的通知

先設定 SQL script 可執行外部 指令
```
-- To allow advanced options to be changed.  
EXEC sp_configure 'show advanced options', 1;  
GO  
-- To update the currently configured value for advanced options.  
RECONFIGURE;  
GO  
-- To enable the feature.  
EXEC sp_configure 'xp_cmdshell', 1;  
GO  
-- To update the currently configured value for this feature.  
RECONFIGURE;  
GO  
```

在穿插於完成前後執行 (其中 CMDSQL 自行更名)
```
EXEC master..xp_CMDShell 'c:\script\telegram-bot-db.exe'  
```

那個 telegram-bot-db.exe 是 python 用 ... pyinstall 編譯過的 參考
```
https://echochio-tw.github.io/2019/09/python-pyinstaller/
```

附上 telegram-bot-db python
```
def telegram_send(bot_message,bot_chatID = '-3XXXXXXXX'):
    import requests
    bot_token = '866411090:AAEv-xxxxxxxxxxxxxxxxxxxx'
    send_text = 'https://api.telegram.org/bot' + bot_token + '/sendMessage?chat_id=' + bot_chatID + '&parse_mode=Markdown&text=' + bot_message
    response = requests.get(send_text)
    return response.json()
telegram_send("Truncate DBXXXXX 結束")
```

當然如果在大陸地區發 telegram 要過跳板 
在外地可通的地方開 nginx 反向代理 參考
```
https://echochio-tw.github.io/2019/02/phpservermonotor_telegram/
```
