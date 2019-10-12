---
layout: post
title: zabbix 監控 MSSQL 數據庫(檔案大小) 與效能
date: 2019-09-23
tags: zabbix msssql
---
Microsoft SQL Server 大多數請求是通過Windows性能計數器 perf_counter 完成的, 一些是通過PowerShell完成的，另一些是通過ODBC完成的。

powershell 可參考(這是由 https://github.com/sfuerte/zbx-mssql 複製來的)
```
https://github.com/echochio-tw/zbx-mssql
```
zabbix_agentd.conf 配置文件中的 PerfCounter 高级参数。

```
PerfCounter = db_pages,"\SQLServer:Buffer Manager()\Database pages",60
PerfCounter = db_free_pages,"\SQLServer:Buffer Manager()\Free pages",60
PerfCounter = db_data_file_size,"\SQLServer:Databases(DATABASE_NAME)\Data File(s) Size (KB)",60
PerfCounter = db_log_file_size,"\SQLServer:Databases(DATABASE_NAME)\Log File(s) Size (KB)",60
PerfCounter = db_userconns,"\SQLServer:General Statistics()\User Connections",60
PerfCounter = db_server_mem,"\SQLServer:Memory Manager()\Total Server Memory (KB)",60
PerfCounter = db_cpu_load,"\Process(sqlservr)\% Processor Time",60
```

範例:
```
PerfCounter = db_pages,"\SQLServer:Buffer Manager()\Database pages",60
PerfCounter = db_free_pages,"\SQLServer:Buffer Manager()\Free pages",60
PerfCounter = db_XXUsernameDB_file_size,"\SQLServer:Databases(XXUsernameDB)\Data File(s) Size (KB)",60
PerfCounter = db_XXUsernameDB_log_file_size,"\SQLServer:Databases(XXUsernameDB)\Log File(s) Size (KB)",60
PerfCounter = db_XXAddressDB_file_size,"\SQLServer:Databases(XXAddressDB)\Data File(s) Size (KB)",60
PerfCounter = db_XXAddressDB_log_file_size,"\SQLServer:Databases(XXAddressDB)\Log File(s) Size (KB)",60
PerfCounter = db_userconns,"\SQLServer:General Statistics()\User Connections",60
PerfCounter = db_server_mem,"\SQLServer:Memory Manager()\Total Server Memory (KB)",60
PerfCounter = db_cpu_load,"\Process(sqlservr)\% Processor Time",60
```

這邊要抓 db_size 大小 設定 Application
```
MSSQL_DB_data_and_log_file_size
```

抓 db_size 大小 設定 Items (這邊是範例看需求設定我的範例設定四個)
```
Name : db_XXUsernameDB_file_size
Type : Zabbix agent
Key  : db_XXUsernameDB_file_size
Tpye of information : Numeric
Units : (B)
Update interval : 30
History : 90d 
Trend store priod : 356d
Applications : MSSQL_DB_data_and_log_file_size
```

Triggers  看需求設定 ...
```
Name : Accounts server:db_XXUsernameDB_file_size > 1G
Severity : High
Expression : {Accounts server:db_XXUsernameDB_file_size.last(#1)}>1G
```

Graphs 
```
Name : DB_data_and_log_file_size
Width : 900
Height : 200
Items : Accounts server db_XXUsernameDB_file_size 
        Accounts server db_XXUsernameDB_log_file_size 
        Accounts server db_XXAddressDB_file_size 
        Accounts server db_XXAddressDB_log_file_size
```

powershell 的 :

zabbix_agentd.win.conf 加入
```
UserParameter = mssql.db.discovery，powershell -NoProfile -ExecutionPolicy Bypass -File“C:\zabbix\scripts\mssql_basename.ps1”
UserParameter = mssql.version，powershell -NoProfile -ExecutionPolicy Bypass -File“C:\zabbix\scripts\ mssql_version.ps1” 
```

將PowerShell腳本（`mssql _ * .ps1`）複製到腳本文件夾C:\zabbix\scripts\

mssql_basename.ps1
```
function convertto-encoding ([string]$from, [string]$to){
    begin{
        $encfrom = [system.text.encoding]::getencoding($from)
        $encto = [system.text.encoding]::getencoding($to)
    }
    process{
        $bytes = $encto.getbytes($_)
        $bytes = [system.text.encoding]::convert($encfrom, $encto, $bytes)
        $encto.getstring($bytes)
    }
}

$SQLServer = $(hostname.exe)

#$uid = "Login" 
#$pwd = "Password"

#$connectionString = "Server = $SQLServer; User ID = $uid; Password = $pwd;"

$connectionString = "Server = $SQLServer; Integrated Security = True;"

$connection = New-Object System.Data.SqlClient.SqlConnection
$connection.ConnectionString = $connectionString
$connection.Open()

#Создаем запрос непосредственно к MSSQL / Create a request directly to MSSQL
$SqlCmd = New-Object System.Data.SqlClient.SqlCommand  
$SqlCmd.CommandText = "SELECT name FROM  sysdatabases"
$SqlCmd.Connection = $Connection
$SqlAdapter = New-Object System.Data.SqlClient.SqlDataAdapter
$SqlAdapter.SelectCommand = $SqlCmd
$DataSet = New-Object System.Data.DataSet
$SqlAdapter.Fill($DataSet) > $null
$Connection.Close()

$basename = $DataSet.Tables[0]

$idx = 1
write-host "{"
write-host " `"data`":[`n"
foreach ($name in $basename)
{
    if ($idx -lt $basename.Rows.Count)
        {
            $line= "{ `"{#DBNAME}`" : `"" + $name.name + "`" }," | convertto-encoding "cp866" "utf-8"
            write-host $line
        }
    elseif ($idx -ge $basename.Rows.Count)
        {
            $line= "{ `"{#DBNAME}`" : `"" + $name.name + "`" }" | convertto-encoding "cp866" "utf-8"
            write-host $line
        }
    $idx++;
}

write-host
write-host " ]"
write-host "}"
```

mssql_version.ps1
```
$ver = Invoke-Sqlcmd -Query "SELECT @@VERSION;" -QueryTimeout 3

write-host $ver.Column1
```

zbx_template_mssql.xml
```
https://raw.githubusercontent.com/sfuerte/zbx-mssql/master/zbx_template_mssql.xml
```

