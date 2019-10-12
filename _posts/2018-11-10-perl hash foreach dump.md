---
layout: post
title: perl hash foreach dump
date: 2018-11-10
tags: perl json xml
---

寫perl用到 json xml ... 記錄一下

## perl HASH Dump ..... 

```
print_element("Domain",$xml,"");
sub print_element {
  my $name= shift;
  my $element = shift;
  my $indent = shift;
  print $indent . "Element: " . $name. "\n";
  $indent .= "    ";
  foreach my $key (keys %$element) {
    if (ref $element->{$key} eq "ARRAY") {
      foreach my $subelement (@{$element->{$key}}) {
        if (ref $subelement eq "HASH") {
          print_element($key, $subelement,$indent);
        } else {
          print $indent . "Element: " . $key . ", Value: " . $subelement . "\n";
        }
      }
    } else {
      print $indent . "Attribute: " . $key . ". Value: " . $element->{$key} . "\n";
    }
  }
}
```
