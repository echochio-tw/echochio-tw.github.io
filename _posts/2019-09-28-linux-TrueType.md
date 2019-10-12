---
layout: post
title: linux TrueType font
date: 2019-09-28
tags: linux font 
---
最近常用到紀錄一下

下載 源樣黑體字体

```
wget https://github.com/ButTaiwan/genyog-font/raw/master/TW/GenYoGothicTW-Light.ttf
```

將檔案放到 字集區
```
mv GenYoGothicTW-Light.ttf /usr/share/fonts/.
```

掃描字體加入
```
fc-cache -f -v
```


