---
layout: post
title: telegram bot qrcode
date: 2019-05-23
tags: telegram qrcode
---

telegram 機器人 掃QR code 換網址

python 程式如下
```
#!/usr/bin/python
# Telegram Remote-Shell
from datetime import datetime
now = datetime.now()
import telepot,time,os
import qrtools,zbar
inqr = qrtools.QR()
try:
    def handle(msg):
        content_type, chat_type, chat_id = telepot.glance(msg)
        sender = msg['from']['id']
        f = open('trsh.log', 'a')
        date_time = now.strftime("%m/%d/%Y, %H:%M:%S")
        f.write("Chat-id - "+str(chat_id)+", content_type - "+str(content_type)+", Sender - "+str(sender)+", "+date_time+"\n")
        f.close()
        if (content_type == 'photo') and (os.path.isfile("/tmp/chioqr")):
            bot.download_file(msg['photo'][-1]['file_id'], '/tmp/file.png')
            output = "Scanning ..."
            inqr.decode("/tmp/file.png")
            bot.sendMessage(chat_id, output)
            bot.sendMessage(chat_id, inqr.data)
            stn = inqr.data.find("/",7)
            chioinfo = os.popen("head -1 /tmp/chioqr.info").read()
            os.popen("sed -i '1d' /tmp/chioqr.info").read()
            chioinfo = chioinfo.strip('\n')
            newchio = "http://"+chioinfo+inqr.data[stn:]
            bot.sendMessage(chat_id, "New : "+newchio)
            os.popen("/root/qr.py "+newchio).read()
            photo = open('/tmp/photo.png', 'rb')
            bot.sendPhoto(chat_id, photo)
            output = os.popen("cat /tmp/chioqr.info|wc -l").read()
            output = output.strip('\n')
            bot.sendMessage(chat_id, newchio)
            bot.sendMessage(chat_id,"The domain also has : "+output)
            os.popen("rm -rf /tmp/chioqr").read()
        elif content_type == 'text':
            text = msg['text']
            args=text.split()
            command = args[0]
            if command == '/chio':
                os.popen("rm -rf /tmp/chioqr").read()
                os.popen("touch /tmp/chioqr").read()
                bot.sendMessage(chat_id, "Please sent image")
            else:
                bot.sendMessage(chat_id, "Error input wait for 10 Seconds")
                time.sleep(10)
                os.popen("cd /root;kill `ps -ef |grep chioqr.py|grep -v grep|awk '{print $2}'`;python chioqr.py &").read()
        else:
            bot.sendMessage(chat_id, "Error input wait for 10 Seconds")
            time.sleep(10)
            os.popen("cd /root;kill `ps -ef |grep chioqr.py|grep -v grep|awk '{print $2}'`;python chioqr.py &").read()
    bot = telepot.Bot('643132357:XXXXXXXXXXXXX')
    bot.message_loop(handle)
    while 1:
        time.sleep(10)
except:
    time.sleep(10)
    os.popen("cd /root;kill `ps -ef |grep chioqr.py|grep -v grep|awk '{print $2}'`;python chioqr.py &").read()
```
