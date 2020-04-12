---
layout: post
title: python PySimpleGUI
date: 2020-04-12
tags: python PySimpleGUI
---

簡單python gui

```
import PySimpleGUI as sg
def box(values):
    import requests
    response = requests.get(url=values)  
    sg.popup_scrolled('輸出 : \n'+response.text, title="API JSON", size=(80, None))

layout = [  [sg.Text('API 輸入網址')],
            [sg.Text('輸入 :'), sg.InputText()],
            [sg.OK(), sg.Cancel()]]

window = sg.Window('API 輸入', layout)
while True:             
    event, values = window.read()
    if event in (None, 'Cancel'):
        break
    box(values[0])

window.close()
```
<img src="/images/posts/PySimpleGUI/p1.png">

<img src="/images/posts/PySimpleGUI/p2.png">
