---
layout: post
title: 灰域 CDN 刷新
date: 2018-10-07
tags: 灰域 CDN 
---

**get_sid.pl**

其中 username=test%40gmail.com&password=echochio 

... 請自行更換

```
#!/usr/bin/perl
$do="curl -X POST https://api.greypanel.com/api/login -H 'Content-Type: application/x-www-form-urlencoded' -d 'username=test%40gmail.com&password=echochio'"."  2> /dev/null";
$ret=`$do`;
$f_ret='{"result":"';
$l_ret='","response":"success","message":""}';
$o_ret='';
if($ret =~  "success") {
        $ret =~ s/$f_ret/$o_ret/;
        $ret =~ s/$l_ret/$o_ret/;
        print $ret;
}
```

**get_site.pl**

```
#!/usr/bin/perl
use JSON;
$ret = `./get_sid.pl`;
$do="curl -X GET 'https://api.greypanel.com/api/site/find-sites?keyWord=&page=1&pageSize=200' -H 'Content-Type:application/x-www-form-urlencoded' -H 'Cookie:sid=".$ret."'"."  2> /dev/null";
$ret_data=`$do`;
$ret_data = substr($ret_data, index($ret_data,"["));
$ret_data = substr($ret_data, 0, index($ret_data,"]")+1);
$obj = from_json($ret_data);
print $ret;
foreach $item( @$obj ) {
	print " ".$item->{name};
	print " ".$item->{uid};
}
```

**do_refash.pl**

```
#!/usr/bin/perl
$inp=$ARGV[0];
chomp($inp);
@sites = split / /, `./get_site.pl`;
$a=0;
foreach $site (@sites) {
        $a=$a+1;
        if($site =~  $inp){
               last;
       }
}
$do = "curl -X GET 'https://api.greypanel.com/api/cache/clear-site-uid?uid=".$sites[$a]."' -H 'Content-Type:application/x-www-form-urlencoded' -H 'Cookie:sid=".$sites[0]."' 2> /dev/null";
$ret = `$do`;
print $ret;
```
**執行 刷新 hello.com 這網域**

```
# perl do_refash.pl hello.com
{"result":"0edfd67a-5c7b-XXXX-XXXX-XXXXXXXXX","response":"success","message":""}

```
