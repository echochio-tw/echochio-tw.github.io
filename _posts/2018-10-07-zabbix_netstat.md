---
layout: post
title: linux zabbix netstat session
date: 2018-10-07
tags: zabbix
---

client site
```
echo "UserParameter=connect.session,netstat -natp | wc -l" >> /etc/zabbix/zabbix_agentd.d/userparameter_mysql.conf
systemctl restart zabbix-agent
chmod u+s /usr/bin/netstat
```

Server  import xml
```
<?xml version="1.0" encoding="UTF-8"?>
<zabbix_export>
    <version>3.4</version>
    <date>2018-10-07T02:40:45Z</date>
    <groups>
        <group>
            <name>Linux servers</name>
        </group>
    </groups>
    <templates>
        <template>
            <template>Template OS Linux session</template>
            <name>Template OS Linux session</name>
            <description/>
            <groups>
                <group>
                    <name>Linux servers</name>
                </group>
            </groups>
            <applications>
                <application>
                    <name>Session</name>
                </application>
            </applications>
            <items>
                <item>
                    <name>session</name>
                    <type>0</type>
                    <snmp_community/>
                    <snmp_oid/>
                    <key>connect.session</key>
                    <delay>30s</delay>
                    <history>1w</history>
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
                            <name>Session</name>
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
    <graphs>
        <graph>
            <name>netstat session</name>
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
                    <color>1A7C11</color>
                    <yaxisside>0</yaxisside>
                    <calc_fnc>2</calc_fnc>
                    <type>0</type>
                    <item>
                        <host>Template OS Linux session</host>
                        <key>connect.session</key>
                    </item>
                </graph_item>
            </graph_items>
        </graph>
    </graphs>
</zabbix_export>
```
