---
layout: post
title: centos7 zabbix Agent Permission denied
date: 2018-10-01
tags: zabbix
---

```
# cat /var/log/zabbix/zabbix_agentd.log

 29740:20181001:095714.412 Starting Zabbix Agent [Zabbix server]. Zabbix 3.4.14 (revision 84877).
 29740:20181001:095714.413 **** Enabled features ****
 29740:20181001:095714.413 IPv6 support:          YES
 29740:20181001:095714.413 TLS support:           YES
 29740:20181001:095714.413 **************************
 29740:20181001:095714.413 using configuration file: /etc/zabbix/zabbix_agentd.conf
 29740:20181001:095714.413 cannot set resource limit: [13] Permission denied
 29740:20181001:095714.413 cannot disable core dump, exiting...
```

selinux 擋了  ...

```
# yum install setroubleshoot

# sealert -a /var/log/audit/audit.log > /opt/audit_report
```

cat /opt/audit_report

```
*****  Plugin catchall (100. confidence) suggests   **************************

If you believe that zabbix_agentd should be allowed setrlimit access on processes labeled zabbix_agent_t by default.
Then you should report this as a bug.
You can generate a local policy module to allow this access.
Do
allow this access for now by executing:
# ausearch -c 'zabbix_agentd' --raw | audit2allow -M my-zabbixagentd
# semodule -i my-zabbixagentd.pp


Additional Information:
Source Context                system_u:system_r:zabbix_agent_t:s0
Target Context                system_u:system_r:zabbix_agent_t:s0
Target Objects                Unknown [ process ]
Source                        zabbix_agentd
Source Path                   /usr/sbin/zabbix_agentd
Port                          <Unknown>
Host                          <Unknown>
Source RPM Packages           zabbix-agent-3.4.14-1.el7.x86_64
Target RPM Packages
Policy RPM                    selinux-policy-3.13.1-102.el7.noarch
Selinux Enabled               True
Policy Type                   targeted
Enforcing Mode                Enforcing
Host Name                     localhost.localdomain
Platform                      Linux localhost.localdomain 3.10.0-514.el7.x86_64
                              #1 SMP Tue Nov 22 16:42:41 UTC 2016 x86_64 x86_64
Alert Count                   50
First Seen                    2018-10-01 09:43:55 CST
Last Seen                     2018-10-01 09:52:37 CST
Local ID                      75a1ebc2-f9c9-47f8-b767-a904610ec20d

Raw Audit Messages
type=AVC msg=audit(1538358757.664:204530): avc:  denied  { setrlimit } for  pid=29341 comm="zabbix_agentd" scontext=system_u:system_r:zabbix_agent_t:s0 tcontext=system_u:system_r:zabbix_agent_t:s0 tclass=process


type=SYSCALL msg=audit(1538358757.664:204530): arch=x86_64 syscall=setrlimit success=no exit=EACCES a0=4 a1=7fffca121a40 a2=0 a3=8 items=0 ppid=29340 pid=29341 auid=4294967295 uid=997 gid=995 euid=997 suid=997 fsuid=997 egid=995 sgid=995 fsgid=995 tty=(none) ses=4294967295 comm=zabbix_agentd exe=/usr/sbin/zabbix_agentd subj=system_u:system_r:zabbix_agent_t:s0 key=(null)

Hash: zabbix_agentd,zabbix_agent_t,zabbix_agent_t,process,setrlimit

```
照上面指示打 ....

再看 zabbix_agentd.log
```
# ausearch -c 'zabbix_agentd' --raw | audit2allow -M my-zabbixagentd
# semodule -i my-zabbixagentd.pp
# tail -f /var/log/zabbix/zabbix_agentd.log
 29775:20181001:095745.170 Starting Zabbix Agent [Zabbix server]. Zabbix 3.4.14 (revision 84877).
 29775:20181001:095745.170 **** Enabled features ****
 29775:20181001:095745.170 IPv6 support:          YES
 29775:20181001:095745.170 TLS support:           YES
 29775:20181001:095745.170 **************************
 29775:20181001:095745.170 using configuration file: /etc/zabbix/zabbix_agentd.conf
 29775:20181001:095745.176 agent #0 started [main process]
 29776:20181001:095745.177 agent #1 started [collector]
 29777:20181001:095745.177 agent #2 started [listener #1]
 29778:20181001:095745.177 agent #3 started [listener #2]
 29779:20181001:095745.177 agent #4 started [listener #3]
 29780:20181001:095745.177 agent #5 started [active checks #1]
```


