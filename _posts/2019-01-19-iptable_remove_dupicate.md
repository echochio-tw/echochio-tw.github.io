---
layout: post
title: iptable_remove_dupicate
date: 2019-01-19
tags: iptables
---

```
iptables -vnL|grep DROP  | awk '{ print $8}' | grep -v "0.0.0.0" |awk '/[0-9]/' | sort | uniq -c|sort -nr |awk '{if($1 >1) print $1" "$2}' |awk '{for(i=1;i<=$1-1;i++) print "iptables -D INPUT -s  " $2 " -j DROP" | "bash" } '
```

```
#!/usr/bin/perl
$sendto='-xxxxx';
while (1) {
$do = "netstat -nat |grep SYN_RECV |grep 10X00| awk '{print ";
$do = $do."\$"."5}'"."|awk -F: '{print "."\$"."1}'|sort|uniq -c|sort -rn";
$do = $do."|awk '{print "."\$"."1\":\"\$2}'";
@data= `$do`;
@v = split(':', $data[0]);
if ( $v[0] > 100) {
        $now=`date +%s`;
        chomp($now);
        $last=`cat /tmp/sync`;
        chomp($last);
        if ($now > ($last+1200)) {
                $d = 'SYN_RECV数量大于150..有攻击';
                $do = 'curl -G  "http://xx.xx.xx.xx:16888/z.php" --data-urlencode "sendto="'.$sendto.' --data-urlencode "subject="'.$d.' --silent';
                `$do`;
        }
        chomp($v[1]);
        $do = "tcpkill host ".$v[1]." -i eth0 \&";
        system($do);
        $do = "/sbin/iptables -A INPUT -s ".$v[1]." -j DROP \&";
        system($do);
        $do = "echo '".$now."' > /tmp/sync";
        `$do`;
        $do = "sh /root/iptable_remove_dupicate.sh";
        system($do);
}

sleep(10);
}

```
