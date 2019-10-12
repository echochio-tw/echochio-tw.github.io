---
layout: post
title: 網頁用 root 執行程式
date: 2019-03-03
tags: linux nginx apache
---

有時會忘記紀錄一下 (裡面跑 python , shell ,perl .... 都可)

作法是寫個 C 然後 suid 到 root , 用這 C 編譯好的去執行 ....

當然要 suid --> 4755

C 的程式如下
```
#include <string.h>
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
int main(int argc, char* argv[])
{
setuid(0);
char str[200];
strcpy (str,"python /var/www/html/58yumi/58yumi_set_new.py ");
strcat (str,argv[1]);
strcat (str," ");
strcat (str,argv[2]);
system(str);
return 0;
}
```

編譯然後 , suid -->  chmod 4755
```
gcc /var/www/html/58yumi/58yumi_set_new.c -o /var/www/html/58yumi/58yumi_set_new
chmod 4755 /var/www/html/58yumi/58yumi_set_new
```

就網頁呼叫 這個 C ... 例如 php :

```
<?php
echo shell_exec("/var/www/html/58yumi/58yumi_set_new xxxx 1234") ;
```
