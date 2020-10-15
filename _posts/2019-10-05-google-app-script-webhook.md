---
layout: post
title: telegram group google app script webhoos 
date: 2019-10-05
tags: telegram group bot webhoos
---

用 google app script (配合 telegram group)

```
https://echochio-tw.github.io/2019/10/telegram-group-python-webhoos/
```

發布 -> 部署為網路應用程式 
```
Deploy as web app 
Execute the app as :  Me
Who has access tothe app: Anyone,even anonymous
```
其中 
```
chat_id = -341335111
bot = 917681111:AAFpNGEoiebe1yXbkHDVbS38aaAjzDSJLjs
google 的 script = https://script.google.com/macros/s/AKfycbweh3kkFqNlOkMYpBS_Ltmsj8oKiLNTXQ8fVcBgmLAa0w_ED15V/exec
```
自行修改

google app script 如下
```
function doPost(e) {
  // POST來的資料轉json
  var chat = JSON.parse(e.postData.contents);  
  // 是文字格式時
  if(chat.message) {
    // 房間號的正確時處理
    if (chat.message.chat.id == "-341335111"){
      // 切割字串
      var message = chat.message.text;
　    var messageArray = new Array();
      var messageArray = message.split(" ");
      // 前個為 /refresh 時處理
      if (messageArray[0] == "/refresh") {
        sendTelegram(messageArray[1])
      }
    }
  }
}

function sendTelegram(body) {
    var botSecret = "917681111:AAFpNGEoiebe1yXbkHDVbS38aaAjzDSJLjs";
    var chatId ="-341335111";
    var response = UrlFetchApp.fetch("https://api.telegram.org/bot" + botSecret + "/sendMessage?text=" + encodeURIComponent(body) + "&chat_id=" + chatId + "&parse_mode=HTML");
}
```

telegram webhook
```
curl "https://api.telegram.org/bot917681111:AAFpNGEoiebe1yXbkHDVbS38aaAjzDSJLjs/setWebhook?url=https://script.google.com/macros/s/AKfycbweh3kkFqNlOkMYpBS_Ltmsj8oKiLNTXQ8fVcBgmLAa0w_ED15V/exec"

```

接下來試試 google app script 呼叫這個 ....串起來
```
https://echochio-tw.github.io/2019/09/flask+uwsgi/
```

要讓 bot 可以在 group 裡面可以回應
```
私訊 @BotFather
輸入 /setprivacy 指令
選擇 Bot
點擊 Disable
```
