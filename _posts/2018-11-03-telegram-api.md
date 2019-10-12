---
layout: post
title: telegram api
date: 2018-11-03
tags: telegram api
---

大陸地區無法用 api.telegram ... 轉香港 ...還好可用

用在 PERL .... 
```
$do = 'curl -G  "http://207.148.73.58:16888/z.php" --data-urlencode "sendto="'.$sendto.' --data-urlencode "subject=提醒："'.$inpn.' --data-urlencode "message=XXXXXX" 2> /dev/null';
```

要先申請 bot 取 boot id : xxxxxxxx:xxxxxxxxxxxxxxxx

也要知道 chat_id ....
```
<?php
$sendto=$_GET['sendto'];
$subject=$_GET['subject'];
$message=$_GET['message'];

// chat_id=xxxxxxx123456789 BOT私訊
// chat_id=-123456789 哈拉服務群
$telegram_api="https://api.telegram.org/botxxxxxxxx:xxxxxxxxxxxxxxxx/sendMessage?";

// $send=$telegram_api . "chat_id=" . $sendto. "&text=" . $message;
$send=$telegram_api . "chat_id=" . $sendto. "&text=" . $subject;

file_get_contents($send);
?>

```
