---
layout: post
title: ansible shell and script
date: 2019-05-23
tags: ansible shell script
---

ansible 目錄如下

```
.
├── ansible.cfg
├── date_local_shell.sh
├── date_remote_shell.sh
├── inventory
│   └── host.xml
├── roles
│   └── clean
│       └── task
└── shell
    └── date.sh
```

host.xml
```
dy:
  vars:
    ansible_port: 22
    ansible_user: root
    ansible_password: 'UxxxxxUUUUU'
    gateway_key: 'dy'

  hosts:
    dy01:
      ansible_host: 192.168.0.11

    dy02:
      ansible_host: 192.168.0.12

    dy03:
      ansible_host: 192.168.0.13

    dy04:
      ansible_host: 192.168.0.14
```

shell/date.sh
```
date
```

在遠端 執行ansible server 端 script
date_local_shell.sh
```
ansible $1 -m script -a 'shell/date.sh'
```

在遠端 執行遠端 shell script 
date_remote_shell.sh
```
ansible $1 -m shell -a "date"
```

ansible.cfg
```
[defaults]
inventory      = /home/ansible/inventory
```
