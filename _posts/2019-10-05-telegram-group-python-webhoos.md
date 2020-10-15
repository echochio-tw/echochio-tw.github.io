---
layout: post
title: telegram group python webhoos 
date: 2019-10-05
tags: telegram group bot webhoos
---

首先要將 bot 的 Privacy mode 改為 disable 這樣 chat group 的 bot 就會收到所有訊息 ...

所以這個 Privacy mode 改為 disable 的 bot 不能用在多群組內, 只能用於單一 chat group

不然會收到一大堆訊息把 webhoos 搞掛了

進去 BotFather 設定 Privacy mode 
```
/mybots -> select bot -> Bot Settings -> Privacy mode -> disabled (defult Enable)
```

這邊用 ubuntu +  ngrok 

事實我用 win10的 WSL 請參考 https://echochio-tw.github.io/2019/05/win10-WSL-ubuntu/

ngrok 安裝 清參考官網
```
https://dashboard.ngrok.com/get-started
```

我寫的 python 執行 ....
```
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
import requests
import telegram
from flask import Flask, request
from telegram.ext import Dispatcher, MessageHandler, Filters

bot = telegram.Bot(token="917681111:AAFpNGEoiebe1yXbkHDVbaauKAjzDSJLjs")
chat_id = -341335110

def telegram_send(bot_message):
    bot_token = "917681111:AAFpNGEoiebe1yXbkHDVbaauKAjzDSJLjs"
    send_text = 'https://api.telegram.org/bot' + bot_token + '/sendMessage?chat_id=' + str(chat_id) + '&parse_mode=Markdown&text=' + bot_message
    print (send_text)
    response = requests.get(send_text)
    return response.json()

app = Flask(__name__)

@app.route('/', methods = ["POST"])
def webhook_handler():
    if request.method == "POST":
        update = telegram.Update.de_json(request.get_json(force=True), bot)
        #print(update)
        #print(update.message.text)
        #print(update.message.chat.id)
        if (update.message.chat.id == chat_id):
            ret = telegram_send('==>'+str(update.message.text)+'<==')
        #print(ret)
    return('ok')

if __name__ == '__main__':
        app.run(port = 5000)
```

執行 ngrok port 5000

```
./ngrok http 5000
```

看到 分抓到的 https
```
Forwarding                    https://9dbd9deb.ngrok.io -> http://localhost:5000
```

配合 bot_token 產生網址 curl 一下
```
curl "https://api.telegram.org/bot917681111:AAFpNGEoiebe1yXbkHDVbaauKAjzDSJLjs/setWebhook?url=https://9dbd9deb.ngrok.io/"
```

丟一下telegarm 畫面可看到回應及 python 有輸出

```
telegram 輸入 : 處理一下

telegram 輸入 : ==>處理一下<==

```


python 有輸出
```
https://api.telegram.org/bot917681111:AAFpNGEoiebe1yXbkHDVbaauKAjzDSJLjs/sendMessage?chat_id=-341335110&parse_mode=Markdown&text===========>處理一下
```

接下來是 用 google app script 來試試 .....

