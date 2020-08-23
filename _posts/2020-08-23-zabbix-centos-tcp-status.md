---
layout: post
title: zabbix 監控 CentOS TCP 連線
date: 2020-08-23
tags: zabbix centos
---

Nginx 被打了監控一下 ...  還好打到 5000 多 沒死 廠商說 256G 

設定要開 linux max 65535 及 nginx 開最大效能及 一些 DDOS 防護

要去做 linux init
```
cp /etc/sysctl.conf /etc/sysctl.conf.bak
if cat /etc/sysctl.conf | grep "anten" > /dev/null ;then
echo ""
else
cat >> /etc/sysctl.conf <<EOF
#system  add
fs.file-max=65535
net.ipv4.tcp_timestamps = 0
net.ipv4.tcp_synack_retries = 5
net.ipv4.tcp_syn_retries = 5
net.ipv4.tcp_tw_recycle = 1
net.ipv4.tcp_tw_reuse = 1
net.ipv4.tcp_fin_timeout = 30
net.ipv4.tcp_keepalive_time = 120
net.ipv4.ip_local_port_range = 1024  65535
kernel.shmall = 2097152
kernel.shmmax = 2147483648
kernel.shmmni = 4096
kernel.sem = 5010 641280 5010 128
net.core.wmem_default=262144
net.core.wmem_max=262144
net.core.rmem_default=4194304
net.core.rmem_max=4194304
net.ipv4.tcp_fin_timeout = 10
net.ipv4.tcp_keepalive_time = 30
net.ipv4.tcp_window_scaling = 0
net.ipv4.tcp_sack = 0
kernel.hung_task_timeout_secs = 0
EOF
fi

sysctl -p

if cat /etc/security/limits.conf | grep "* soft nofile 65535" > /dev/null;then
    echo ""
else
    echo "* soft nofile 65535" >> /etc/security/limits.conf
fi
if cat /etc/security/limits.conf | grep "* hard nofile 65535" > /dev/null ;then
    echo ""
else
    echo "* hard nofile 65535" >> /etc/security/limits.conf
fi


#修改預設128  定義了系统中每一個端口最大的Listen lenth,這是全域變數
echo 1000 >/proc/sys/net/core/somaxconn

systemctl stop firewalld
systemctl disable firewalld
setenforce 0

yum -y update

# selinux
sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/sysconfig/selinux
sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/selinux/config
```

nginx 設定
```
worker_processes 24; #24 核就開 24 

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;

worker_rlimit_nofile 65535; # 開最大
events {
    worker_connections  65535; # 開最大
}

http {
    limit_conn_zone $binary_remote_addr zone=addr:10m;#记录160000个请求 超过将返回失败
    limit_req_zone $binary_remote_addr zone=one:10m rate=30r/s;#单个请求小于30r/s
}
  limit_conn addr 20; #单个IP地址的连接数量
  limit_req zone=one burst=150; #单一的IP地址突发不超过150个请求。
  
```

/etc/zabbix/zabbix_agentd.d/tcp_status.conf
```
UserParameter=tcp.status[*], /usr/sbin/ss -ant|grep -c $1
```
/etc/zabbix/zabbix_agentd.conf 加入
```
UnsafeUserParameters=1
```

zabbix Server 端 zbx_tcp_Status.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<zabbix_export>
    <version>3.4</version>
    <date>2020-08-23T13:16:22Z</date>
    <groups>
        <group>
            <name>Template</name>
        </group>
    </groups>
    <templates>
        <template>
            <template>Template tcp_status</template>
            <name>Template tcp_status</name>
            <description/>
            <groups>
                <group>
                    <name>Template</name>
                </group>
            </groups>
            <applications>
                <application>
                    <name>TCP</name>
                </application>
            </applications>
            <items>
                <item>
                    <name>TCP.Status: CLOSE-WAIT</name>
                    <type>0</type>
                    <snmp_community/>
                    <snmp_oid/>
                    <key>tcp.status[CLOSE-WAIT]</key>
                    <delay>10s</delay>
                    <history>90d</history>
                    <trends>365d</trends>
                    <status>0</status>
                    <value_type>3</value_type>
                    <allowed_hosts/>
                    <units/>
                    <snmpv3_contextname/>
                    <snmpv3_securityname/>
                    <snmpv3_securitylevel>0</snmpv3_securitylevel>
                    <snmpv3_authprotocol>0</snmpv3_authprotocol>
                    <snmpv3_authpassphrase/>
                    <snmpv3_privprotocol>0</snmpv3_privprotocol>
                    <snmpv3_privpassphrase/>
                    <params/>
                    <ipmi_sensor/>
                    <authtype>0</authtype>
                    <username/>
                    <password/>
                    <publickey/>
                    <privatekey/>
                    <port/>
                    <description/>
                    <inventory_link>0</inventory_link>
                    <applications>
                        <application>
                            <name>TCP</name>
                        </application>
                    </applications>
                    <valuemap/>
                    <logtimefmt/>
                    <preprocessing/>
                    <jmx_endpoint/>
                    <master_item/>
                </item>
                <item>
                    <name>TCP.Status: CLOSED</name>
                    <type>0</type>
                    <snmp_community/>
                    <snmp_oid/>
                    <key>tcp.status[CLOSING]</key>
                    <delay>10s</delay>
                    <history>90d</history>
                    <trends>365d</trends>
                    <status>0</status>
                    <value_type>3</value_type>
                    <allowed_hosts/>
                    <units/>
                    <snmpv3_contextname/>
                    <snmpv3_securityname/>
                    <snmpv3_securitylevel>0</snmpv3_securitylevel>
                    <snmpv3_authprotocol>0</snmpv3_authprotocol>
                    <snmpv3_authpassphrase/>
                    <snmpv3_privprotocol>0</snmpv3_privprotocol>
                    <snmpv3_privpassphrase/>
                    <params/>
                    <ipmi_sensor/>
                    <authtype>0</authtype>
                    <username/>
                    <password/>
                    <publickey/>
                    <privatekey/>
                    <port/>
                    <description/>
                    <inventory_link>0</inventory_link>
                    <applications>
                        <application>
                            <name>TCP</name>
                        </application>
                    </applications>
                    <valuemap/>
                    <logtimefmt/>
                    <preprocessing/>
                    <jmx_endpoint/>
                    <master_item/>
                </item>
                <item>
                    <name>TCP.Status: ESTABLISHED</name>
                    <type>0</type>
                    <snmp_community/>
                    <snmp_oid/>
                    <key>tcp.status[ESTAB]</key>
                    <delay>10s</delay>
                    <history>90d</history>
                    <trends>365d</trends>
                    <status>0</status>
                    <value_type>3</value_type>
                    <allowed_hosts/>
                    <units/>
                    <snmpv3_contextname/>
                    <snmpv3_securityname/>
                    <snmpv3_securitylevel>0</snmpv3_securitylevel>
                    <snmpv3_authprotocol>0</snmpv3_authprotocol>
                    <snmpv3_authpassphrase/>
                    <snmpv3_privprotocol>0</snmpv3_privprotocol>
                    <snmpv3_privpassphrase/>
                    <params/>
                    <ipmi_sensor/>
                    <authtype>0</authtype>
                    <username/>
                    <password/>
                    <publickey/>
                    <privatekey/>
                    <port/>
                    <description/>
                    <inventory_link>0</inventory_link>
                    <applications>
                        <application>
                            <name>TCP</name>
                        </application>
                    </applications>
                    <valuemap/>
                    <logtimefmt/>
                    <preprocessing/>
                    <jmx_endpoint/>
                    <master_item/>
                </item>
                <item>
                    <name>TCP.Status: FIN-WAIT-1</name>
                    <type>0</type>
                    <snmp_community/>
                    <snmp_oid/>
                    <key>tcp.status[FIN-WAIT-1]</key>
                    <delay>10s</delay>
                    <history>90d</history>
                    <trends>365d</trends>
                    <status>0</status>
                    <value_type>3</value_type>
                    <allowed_hosts/>
                    <units/>
                    <snmpv3_contextname/>
                    <snmpv3_securityname/>
                    <snmpv3_securitylevel>0</snmpv3_securitylevel>
                    <snmpv3_authprotocol>0</snmpv3_authprotocol>
                    <snmpv3_authpassphrase/>
                    <snmpv3_privprotocol>0</snmpv3_privprotocol>
                    <snmpv3_privpassphrase/>
                    <params/>
                    <ipmi_sensor/>
                    <authtype>0</authtype>
                    <username/>
                    <password/>
                    <publickey/>
                    <privatekey/>
                    <port/>
                    <description/>
                    <inventory_link>0</inventory_link>
                    <applications>
                        <application>
                            <name>TCP</name>
                        </application>
                    </applications>
                    <valuemap/>
                    <logtimefmt/>
                    <preprocessing/>
                    <jmx_endpoint/>
                    <master_item/>
                </item>
                <item>
                    <name>TCP.Status: FIN-WAIT-2</name>
                    <type>0</type>
                    <snmp_community/>
                    <snmp_oid/>
                    <key>tcp.status[FIN-WAIT-2]</key>
                    <delay>10s</delay>
                    <history>90d</history>
                    <trends>365d</trends>
                    <status>0</status>
                    <value_type>3</value_type>
                    <allowed_hosts/>
                    <units/>
                    <snmpv3_contextname/>
                    <snmpv3_securityname/>
                    <snmpv3_securitylevel>0</snmpv3_securitylevel>
                    <snmpv3_authprotocol>0</snmpv3_authprotocol>
                    <snmpv3_authpassphrase/>
                    <snmpv3_privprotocol>0</snmpv3_privprotocol>
                    <snmpv3_privpassphrase/>
                    <params/>
                    <ipmi_sensor/>
                    <authtype>0</authtype>
                    <username/>
                    <password/>
                    <publickey/>
                    <privatekey/>
                    <port/>
                    <description/>
                    <inventory_link>0</inventory_link>
                    <applications>
                        <application>
                            <name>TCP</name>
                        </application>
                    </applications>
                    <valuemap/>
                    <logtimefmt/>
                    <preprocessing/>
                    <jmx_endpoint/>
                    <master_item/>
                </item>
                <item>
                    <name>TCP.Status: LAST-ACK</name>
                    <type>0</type>
                    <snmp_community/>
                    <snmp_oid/>
                    <key>tcp.status[LAST-ACK]</key>
                    <delay>10s</delay>
                    <history>90d</history>
                    <trends>365d</trends>
                    <status>0</status>
                    <value_type>3</value_type>
                    <allowed_hosts/>
                    <units/>
                    <snmpv3_contextname/>
                    <snmpv3_securityname/>
                    <snmpv3_securitylevel>0</snmpv3_securitylevel>
                    <snmpv3_authprotocol>0</snmpv3_authprotocol>
                    <snmpv3_authpassphrase/>
                    <snmpv3_privprotocol>0</snmpv3_privprotocol>
                    <snmpv3_privpassphrase/>
                    <params/>
                    <ipmi_sensor/>
                    <authtype>0</authtype>
                    <username/>
                    <password/>
                    <publickey/>
                    <privatekey/>
                    <port/>
                    <description/>
                    <inventory_link>0</inventory_link>
                    <applications>
                        <application>
                            <name>TCP</name>
                        </application>
                    </applications>
                    <valuemap/>
                    <logtimefmt/>
                    <preprocessing/>
                    <jmx_endpoint/>
                    <master_item/>
                </item>
                <item>
                    <name>TCP.Status: LISTEN</name>
                    <type>0</type>
                    <snmp_community/>
                    <snmp_oid/>
                    <key>tcp.status[LISTEN]</key>
                    <delay>10s</delay>
                    <history>90d</history>
                    <trends>365d</trends>
                    <status>0</status>
                    <value_type>3</value_type>
                    <allowed_hosts/>
                    <units/>
                    <snmpv3_contextname/>
                    <snmpv3_securityname/>
                    <snmpv3_securitylevel>0</snmpv3_securitylevel>
                    <snmpv3_authprotocol>0</snmpv3_authprotocol>
                    <snmpv3_authpassphrase/>
                    <snmpv3_privprotocol>0</snmpv3_privprotocol>
                    <snmpv3_privpassphrase/>
                    <params/>
                    <ipmi_sensor/>
                    <authtype>0</authtype>
                    <username/>
                    <password/>
                    <publickey/>
                    <privatekey/>
                    <port/>
                    <description/>
                    <inventory_link>0</inventory_link>
                    <applications>
                        <application>
                            <name>TCP</name>
                        </application>
                    </applications>
                    <valuemap/>
                    <logtimefmt/>
                    <preprocessing/>
                    <jmx_endpoint/>
                    <master_item/>
                </item>
                <item>
                    <name>TCP.Status: SYN-RECV</name>
                    <type>0</type>
                    <snmp_community/>
                    <snmp_oid/>
                    <key>tcp.status[SYN-RECV]</key>
                    <delay>10s</delay>
                    <history>90d</history>
                    <trends>365d</trends>
                    <status>0</status>
                    <value_type>3</value_type>
                    <allowed_hosts/>
                    <units/>
                    <snmpv3_contextname/>
                    <snmpv3_securityname/>
                    <snmpv3_securitylevel>0</snmpv3_securitylevel>
                    <snmpv3_authprotocol>0</snmpv3_authprotocol>
                    <snmpv3_authpassphrase/>
                    <snmpv3_privprotocol>0</snmpv3_privprotocol>
                    <snmpv3_privpassphrase/>
                    <params/>
                    <ipmi_sensor/>
                    <authtype>0</authtype>
                    <username/>
                    <password/>
                    <publickey/>
                    <privatekey/>
                    <port/>
                    <description/>
                    <inventory_link>0</inventory_link>
                    <applications>
                        <application>
                            <name>TCP</name>
                        </application>
                    </applications>
                    <valuemap/>
                    <logtimefmt/>
                    <preprocessing/>
                    <jmx_endpoint/>
                    <master_item/>
                </item>
                <item>
                    <name>TCP.Status: SYN-SENT</name>
                    <type>0</type>
                    <snmp_community/>
                    <snmp_oid/>
                    <key>tcp.status[SYN-SENT]</key>
                    <delay>10s</delay>
                    <history>90d</history>
                    <trends>365d</trends>
                    <status>0</status>
                    <value_type>3</value_type>
                    <allowed_hosts/>
                    <units/>
                    <snmpv3_contextname/>
                    <snmpv3_securityname/>
                    <snmpv3_securitylevel>0</snmpv3_securitylevel>
                    <snmpv3_authprotocol>0</snmpv3_authprotocol>
                    <snmpv3_authpassphrase/>
                    <snmpv3_privprotocol>0</snmpv3_privprotocol>
                    <snmpv3_privpassphrase/>
                    <params/>
                    <ipmi_sensor/>
                    <authtype>0</authtype>
                    <username/>
                    <password/>
                    <publickey/>
                    <privatekey/>
                    <port/>
                    <description/>
                    <inventory_link>0</inventory_link>
                    <applications>
                        <application>
                            <name>TCP</name>
                        </application>
                    </applications>
                    <valuemap/>
                    <logtimefmt/>
                    <preprocessing/>
                    <jmx_endpoint/>
                    <master_item/>
                </item>
                <item>
                    <name>TCP.Status: TIME-WAIT</name>
                    <type>0</type>
                    <snmp_community/>
                    <snmp_oid/>
                    <key>tcp.status[TIME-WAIT]</key>
                    <delay>10s</delay>
                    <history>90d</history>
                    <trends>365d</trends>
                    <status>0</status>
                    <value_type>3</value_type>
                    <allowed_hosts/>
                    <units/>
                    <snmpv3_contextname/>
                    <snmpv3_securityname/>
                    <snmpv3_securitylevel>0</snmpv3_securitylevel>
                    <snmpv3_authprotocol>0</snmpv3_authprotocol>
                    <snmpv3_authpassphrase/>
                    <snmpv3_privprotocol>0</snmpv3_privprotocol>
                    <snmpv3_privpassphrase/>
                    <params/>
                    <ipmi_sensor/>
                    <authtype>0</authtype>
                    <username/>
                    <password/>
                    <publickey/>
                    <privatekey/>
                    <port/>
                    <description/>
                    <inventory_link>0</inventory_link>
                    <applications>
                        <application>
                            <name>TCP</name>
                        </application>
                    </applications>
                    <valuemap/>
                    <logtimefmt/>
                    <preprocessing/>
                    <jmx_endpoint/>
                    <master_item/>
                </item>
                <item>
                    <name>TCP.Status: CLOSE</name>
                    <type>0</type>
                    <snmp_community/>
                    <snmp_oid/>
                    <key>tcp.status[UNCONN]</key>
                    <delay>10s</delay>
                    <history>90d</history>
                    <trends>365d</trends>
                    <status>0</status>
                    <value_type>3</value_type>
                    <allowed_hosts/>
                    <units/>
                    <snmpv3_contextname/>
                    <snmpv3_securityname/>
                    <snmpv3_securitylevel>0</snmpv3_securitylevel>
                    <snmpv3_authprotocol>0</snmpv3_authprotocol>
                    <snmpv3_authpassphrase/>
                    <snmpv3_privprotocol>0</snmpv3_privprotocol>
                    <snmpv3_privpassphrase/>
                    <params/>
                    <ipmi_sensor/>
                    <authtype>0</authtype>
                    <username/>
                    <password/>
                    <publickey/>
                    <privatekey/>
                    <port/>
                    <description/>
                    <inventory_link>0</inventory_link>
                    <applications>
                        <application>
                            <name>TCP</name>
                        </application>
                    </applications>
                    <valuemap/>
                    <logtimefmt/>
                    <preprocessing/>
                    <jmx_endpoint/>
                    <master_item/>
                </item>
            </items>
            <discovery_rules/>
            <httptests/>
            <macros/>
            <templates/>
            <screens/>
        </template>
    </templates>
    <triggers>
        <trigger>
            <expression>{Template tcp_status:tcp.status[ESTAB].last(#1)}&gt;500</expression>
            <recovery_mode>0</recovery_mode>
            <recovery_expression/>
            <name>gateway tcp ESTAB over 500</name>
            <correlation_mode>0</correlation_mode>
            <correlation_tag/>
            <url/>
            <status>0</status>
            <priority>4</priority>
            <description/>
            <type>0</type>
            <manual_close>0</manual_close>
            <dependencies/>
            <tags/>
        </trigger>
        <trigger>
            <expression>{Template tcp_status:tcp.status[SYN-RECV].last()}&gt;500</expression>
            <recovery_mode>0</recovery_mode>
            <recovery_expression/>
            <name>gateway tcp syn_recv over 500</name>
            <correlation_mode>0</correlation_mode>
            <correlation_tag/>
            <url/>
            <status>0</status>
            <priority>4</priority>
            <description/>
            <type>0</type>
            <manual_close>0</manual_close>
            <dependencies/>
            <tags/>
        </trigger>
    </triggers>
    <graphs>
        <graph>
            <name>TCP_status</name>
            <width>900</width>
            <height>200</height>
            <yaxismin>0.0000</yaxismin>
            <yaxismax>100.0000</yaxismax>
            <show_work_period>1</show_work_period>
            <show_triggers>1</show_triggers>
            <type>0</type>
            <show_legend>1</show_legend>
            <show_3d>0</show_3d>
            <percent_left>0.0000</percent_left>
            <percent_right>0.0000</percent_right>
            <ymin_type_1>0</ymin_type_1>
            <ymax_type_1>0</ymax_type_1>
            <ymin_item_1>0</ymin_item_1>
            <ymax_item_1>0</ymax_item_1>
            <graph_items>
                <graph_item>
                    <sortorder>0</sortorder>
                    <drawtype>0</drawtype>
                    <color>EE0000</color>
                    <yaxisside>0</yaxisside>
                    <calc_fnc>2</calc_fnc>
                    <type>0</type>
                    <item>
                        <host>Template tcp_status</host>
                        <key>tcp.status[ESTAB]</key>
                    </item>
                </graph_item>
                <graph_item>
                    <sortorder>1</sortorder>
                    <drawtype>0</drawtype>
                    <color>666600</color>
                    <yaxisside>0</yaxisside>
                    <calc_fnc>2</calc_fnc>
                    <type>0</type>
                    <item>
                        <host>Template tcp_status</host>
                        <key>tcp.status[CLOSE-WAIT]</key>
                    </item>
                </graph_item>
                <graph_item>
                    <sortorder>2</sortorder>
                    <drawtype>0</drawtype>
                    <color>BB2A02</color>
                    <yaxisside>0</yaxisside>
                    <calc_fnc>2</calc_fnc>
                    <type>0</type>
                    <item>
                        <host>Template tcp_status</host>
                        <key>tcp.status[UNCONN]</key>
                    </item>
                </graph_item>
                <graph_item>
                    <sortorder>3</sortorder>
                    <drawtype>0</drawtype>
                    <color>660066</color>
                    <yaxisside>0</yaxisside>
                    <calc_fnc>2</calc_fnc>
                    <type>0</type>
                    <item>
                        <host>Template tcp_status</host>
                        <key>tcp.status[CLOSING]</key>
                    </item>
                </graph_item>
                <graph_item>
                    <sortorder>4</sortorder>
                    <drawtype>0</drawtype>
                    <color>AC8C14</color>
                    <yaxisside>0</yaxisside>
                    <calc_fnc>2</calc_fnc>
                    <type>0</type>
                    <item>
                        <host>Template tcp_status</host>
                        <key>tcp.status[LAST-ACK]</key>
                    </item>
                </graph_item>
                <graph_item>
                    <sortorder>5</sortorder>
                    <drawtype>0</drawtype>
                    <color>999999</color>
                    <yaxisside>0</yaxisside>
                    <calc_fnc>2</calc_fnc>
                    <type>0</type>
                    <item>
                        <host>Template tcp_status</host>
                        <key>tcp.status[SYN-RECV]</key>
                    </item>
                </graph_item>
                <graph_item>
                    <sortorder>6</sortorder>
                    <drawtype>0</drawtype>
                    <color>5CCD18</color>
                    <yaxisside>0</yaxisside>
                    <calc_fnc>2</calc_fnc>
                    <type>0</type>
                    <item>
                        <host>Template tcp_status</host>
                        <key>tcp.status[SYN-SENT]</key>
                    </item>
                </graph_item>
                <graph_item>
                    <sortorder>7</sortorder>
                    <drawtype>0</drawtype>
                    <color>5A2B57</color>
                    <yaxisside>0</yaxisside>
                    <calc_fnc>2</calc_fnc>
                    <type>0</type>
                    <item>
                        <host>Template tcp_status</host>
                        <key>tcp.status[FIN-WAIT-1]</key>
                    </item>
                </graph_item>
                <graph_item>
                    <sortorder>8</sortorder>
                    <drawtype>0</drawtype>
                    <color>89ABF8</color>
                    <yaxisside>0</yaxisside>
                    <calc_fnc>2</calc_fnc>
                    <type>0</type>
                    <item>
                        <host>Template tcp_status</host>
                        <key>tcp.status[FIN-WAIT-2]</key>
                    </item>
                </graph_item>
                <graph_item>
                    <sortorder>9</sortorder>
                    <drawtype>0</drawtype>
                    <color>3333FF</color>
                    <yaxisside>0</yaxisside>
                    <calc_fnc>2</calc_fnc>
                    <type>0</type>
                    <item>
                        <host>Template tcp_status</host>
                        <key>tcp.status[TIME-WAIT]</key>
                    </item>
                </graph_item>
                <graph_item>
                    <sortorder>10</sortorder>
                    <drawtype>0</drawtype>
                    <color>FF33FF</color>
                    <yaxisside>0</yaxisside>
                    <calc_fnc>2</calc_fnc>
                    <type>0</type>
                    <item>
                        <host>Template tcp_status</host>
                        <key>tcp.status[LISTEN]</key>
                    </item>
                </graph_item>
            </graph_items>
        </graph>
    </graphs>
</zabbix_export>
```

