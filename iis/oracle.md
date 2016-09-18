## Oracle 常用的log文件內容

- Alter log, Error log, tracing file

 Alert log警告日誌是一個特殊的trace文件，它是按照時間排序的一些log或error信息，具體內容包括：
 - 所有internal error (ora-600), block corruption errors (ora-1578) , 以及死鎖錯誤(ora-60)
 - 管理性操作，比如create , alert , drop 以及startup , shutdown 和archivelog 語句等。
 - 相關shared server 及 dispatcher 進程等功能相關的信息和錯誤。

 ALERT.LOG 保存位置範例

```
SQL> show parameter BACKGROUND_DUMP_DEST

NAME                                 TYPE        VALUE
------------------------------------ ----------- ------------------------------------------
background_dump_dest                 string      /app/oracle/diag/rdbms/o11gr1/o11gr1/trace
```

- Active Session History (ASH) report

 ASH側重於當前數據中活動會話的信息分析，被長期保存在數據字典中，可以通過查詢視圖V$ACTIVE_SESSION_HISTROY來得到。
 - 運行腳本為$ORACLE_HOME/RDBMS/ADMIN/ashrpt.sql

- Automatic Workload Repository (AWR) report
  AWR由運行在oracle的後台進程自動、定期收集資料庫的性能數據並保存，每一個小時，awr都生成一次性能數據快照，為DBA提供某個時刻資料庫性能分析的數據信息。
 - awr默認收集最近7天的採集信息
 - 執行$ORACLE_HOME/RDBMS/ADMIN/awrrpt.sql生成awr報告。



## 利用ELK 來分析alert log

> 對於 Oracle 資料庫來說，alert log 紀錄著 Oracle 資料庫系統所產生的訊息，像是開啟與關閉資料庫、交易日誌切換以及系統錯誤...等訊息都會紀錄在 alert log 文件中，經由查看 alert log 可以發現資料庫有無錯誤，尤其是 ORA-00600、ORA-07445 這類型的 internal error 有可能會造成資料庫服務中止，當資料庫發生問題時，首要工作就是檢查 alert log 有無相關錯誤訊息，因此定時的檢查 alert log 可以及早發現問題並處理。

將Oracle 的alert.log 和 listener.log 集中到ELK , 利用 ELK 來統一收集，搜索，分析和可視化實時信息, 來分析Oracle 產生的問題, 通常在alert.log 和listen.log 包含的內容如下：

- When did the Instance start?
- When has the Instance been shutdown?
- When did ORA- occur? With which code?
- Which IP client did connect to the Instance? With which user?
- How did it connect? Through a service? Through the SID?
- Which program has been used to connect?
- A connection storm occurred, what is the source of it?

### 確認alert.log 和 listener.log 的位置和內容

```
/u01/app/oracle/diag/rdbms/pbdt/PBDT/trace/alert_PBDT.log
/u01/app/oracle/diag/tnslsnr/Dprima/listener/trace/listener.log
```

### 設定 alert.log 到ELK 相關內容

 - The @timestamp field is reflecting the timestamp at which the log entry was created (rather than when logstash read the log entry).
 - It traps ORA- entries and creates a field ORA- when it occurs.
 - It traps the start of the Instance (and fill a field oradb_status accordingly).
 - It traps the shutdown of the Instance (and fill a field oradb_status accordingly).
 - It traps the fact that the Instance is running (and fill a field oradb_status accordingly).

### 設定 listener.log 到ELK 相關內容

 - The @timestamp field is reflecting the timestamp at which the log entry was created (rather than when logstash read the log entry).
 - It traps the connections and records the program into a dedicated field program.
 - It traps the connections and records the user into a dedicated field user.
 - It traps the connections and records the ip of the client into a dedicated field ip_client.
 - It traps the connections and records the destination into a dedicated field dest.
 - It traps the connections and records the destination type (SID or service_name) into a dedicated field dest_type.
 - It traps the command (stop, status, reload) and records it into a dedicated field command.


### realtime dashboard 相關案例

- 範例1: connection repartition to our databases by program and by dest_type (listener.log):

![oracle1](https://bdrouvot.files.wordpress.com/2016/03/kibana_example.png?w=1280&h=676)

- 範例2:  connection “storm” occurred and where it came from 

![oracleA](https://bdrouvot.files.wordpress.com/2016/03/storm1.png?w=640&h=339)


## 利用ELK 來分析ASH report

- [ASH report to ELK 案例說明](https://www.elastic.co/blog/visualising-oracle-performance-data-with-the-elastic-stack) 


###  Logstash with ASH with JDBC Input


me simple structure : input, filter, output. And of those, filter is optional. Here’s our starter for ten, that’ll act as a smoke-test for the Logstash-JDBC-Oracle connectivity:
依據Logstash-JDBC-Oracle 的連接方式，的jdbc 設定檔案如下

```
input {
    jdbc {
        jdbc_validate_connection => true
        jdbc_connection_string => "jdbc:oracle:thin:@oradb:1521/orcl"
        jdbc_user => "system"
        jdbc_password => "Admin123"
        jdbc_driver_library => "/opt/ojdbc7.jar"
        jdbc_driver_class => "Java::oracle.jdbc.driver.OracleDriver"
        statement => "SELECT sysdate from dual"
       }
}
output   {
    stdout { codec => rubydebug }
}
```

### Dashboard sample chart

- sample1: 
 ![ash1](https://www.elastic.co/assets/blt4fe75af2c5869737/kibana-visualization-cutomization-2.png)

- sample2: 
![sample1](https://www.elastic.co/assets/blt4e86578e7326d4c6/kibana-dashboard-hovering.png)

- sample3: 
![sample2](https://www.elastic.co/assets/blt9f68cf2410d7e40d/kibana-dashboard-change-view.png)

- sample4: 
![sample3](https://www.elastic.co/assets/blt9d351eced68fb2a6/kibana-split-chart.png)


### Alter log

The **alert log** file (also referred to as the ALERT.LOG) is a chronological log of messages and errors written out by an Oracle Database. Typical messages found in this file is: database startup, shutdown, log switches, space errors, etc. This file should constantly be monitored to detect unexpected messages and corruptions.

### Location of the ALERT.LOG file[edit]

Oracle will write the alert.log file to the directory as specified by the BACKGROUND_DUMP_DEST parameter. If this parameter is not set, the alert.log will be created in a directory below the value of the DIAGNOSTIC_DEST parameter: DIAGNOSTIC_DEST/diag/rdbms/DB_NAME/ORACLE_SID/trace. If this later parameter is not set, the alert.log file is created in the ORACLE_HOME/rdbms/trace directory.
```
SQL> show parameter BACKGROUND_DUMP_DEST

NAME                                 TYPE        VALUE
------------------------------------ ----------- ------------------------------------------
background_dump_dest                 string      /app/oracle/diag/rdbms/o11gr1/o11gr1/trace
```

### sample of alert log

```
Fri Jul 19 13:21:21 2013
CREATE DATABASE "dev12c"
MAXINSTANCES 8
MAXLOGHISTORY 1
MAXLOGFILES 16
MAXLOGMEMBERS 3
MAXDATAFILES 100
DATAFILE '/u01/app/oracle/oradata/dev12c/system01.dbf' SIZE 700M REUSE AUTOEXTEND ON NEXT 10240K MAXSIZE UNLIMITED
EXTENT MANAGEMENT LOCAL
SYSAUX DATAFILE '/u01/app/oracle/oradata/dev12c/sysaux01.dbf' SIZE 550M REUSE AUTOEXTEND ON NEXT 10240K MAXSIZE UNLIMITED
SMALLFILE DEFAULT TEMPORARY TABLESPACE TEMP TEMPFILE '/u01/app/oracle/oradata/dev12c/temp01.dbf' SIZE 20M REUSE AUTOEXTEND ON NEXT 640K MAXSIZE UNLIMITED
SMALLFILE UNDO TABLESPACE "UNDOTBS1" DATAFILE '/u01/app/oracle/oradata/dev12c/undotbs01.dbf' SIZE 200M REUSE AUTOEXTEND ON NEXT 5120K MAXSIZE UNLIMITED
CHARACTER SET WE8MSWIN1252
NATIONAL CHARACTER SET AL16UTF16
LOGFILE GROUP 1 ('/u01/app/oracle/oradata/dev12c/redo01.log') SIZE 50M,
GROUP 2 ('/u01/app/oracle/oradata/dev12c/redo02.log') SIZE 50M,
GROUP 3 ('/u01/app/oracle/oradata/dev12c/redo03.log') SIZE 50M
USER SYS IDENTIFIED BY USER SYSTEM IDENTIFIED BY
Database mounted in Exclusive Mode
Lost write protection disabled
Ping without log force is disabled.
Using default pga_aggregate_limit of 2560 MB
Fri Jul 19 13:21:28 2013
db_recovery_file_dest_size of 4815 MB is 0.00% used. This is a
user-specified limit on the amount of space that will be used by this
database for recovery-related files, and does not reflect the amount of
space available in the underlying filesystem or ASM diskgroup.
Successful mount of redo thread 1, with mount id 3622234653
Using SCN growth rate of 16384 per second
Assigning activation ID 3622234653 (0xd7e6ea1d)
Starting background process TMON
Fri Jul 19 13:21:28 2013
TMON started with pid=24, OS id=24298
Thread 1 opened at log sequence 1
 Current log# 1 seq# 1 mem# 0: /u01/app/oracle/oradata/dev12c/redo01.log
Successful open of redo thread 1
```


### error from a trace file:

```
*** KEWROCISTMTEXEC - encountered error: (ORA-06525: Length Mismatch for CHAR or RAW data
ORA-06512: at "SYS.DBMS_STATS", line 40111
```


### [Oracle event list 诊断事件作用说明](http://www.dataunload.net/?/article/13)

```
ORA-10000: control file debug event, name 'control_file'                       
ORA-10001: control file crash event1                                           
ORA-10002: control file crash event2                                           
ORA-10003: control file crash event3                                           
ORA-10004: block recovery testing - internal error                             
ORA-10005: trace latch operations for debugging                                
ORA-10006: block recovery testing - external error                             
ORA-10007: log switch debug crash after new log select, thread                 
ORA-10008: log switch debug crash after new log header write, thread           
ORA-10009: log switch debug crash after old log header write, thread           
ORA-10010: Begin Transaction                                                   
```






### oracle database log file
- [oracle all log](https://docs.oracle.com/cd/E18440_01/doc.111/e21484/logs_and_directories.htm#OPCRG169)


### 常用的log file
- alert.log
 - select value from v$parameter where name = 'background_dump_dest'
 - file path: /u01/app/oracle/diag/dbms/orcl/orcl/trace
- Active Session History (ASH) file 
- AWR report file:
 - 


## sample log file with ELK

### alert.log with ELK
- [oracle-elk](https://bdrouvot.wordpress.com/2016/03/26/push-the-oracle-alert-log-and-listener-log-into-elasticsearch-and-analyzevisualize-their-content-with-kibana/)
- The oracle alert.log and listener.log contain useful information to provide answer to questions like:
 - When did the Instance start?
 - When has the Instance been shutdown?
 - When did ORA- occur? With which code?
 - Which IP client did connect to the Instance? With which user?
 - How did it connect? Through a service? Through the SID?
 - Which program has been used to connect?
 - A connection storm occurred, what is the source of it?

```
/u01/app/oracle/diag/rdbms/pbdt/PBDT/trace/alert_PBDT.log
/u01/app/oracle/diag/tnslsnr/Dprima/listener/trace/listener.log
```

![oracle1](https://bdrouvot.files.wordpress.com/2016/03/kibana_example.png?w=1280&h=676)

### ASH with ELK
- [ASH](https://www.elastic.co/blog/visualising-oracle-performance-data-with-the-elastic-stack) - with JDBC input

 ![ash1](https://www.elastic.co/assets/blt4fe75af2c5869737/kibana-visualization-cutomization-2.png)
 ![ash2](https://www.elastic.co/assets/blt9d351eced68fb2a6/kibana-split-chart.png)
 ![ash3](https://www.elastic.co/assets/blt4e86578e7326d4c6/kibana-dashboard-hovering.png)


### goldengate with ELK
- file path : /u01/ogg-bd/ggserr.log
- [sample-gg](http://rmoff.net/2016/04/14/streaming-data-through-oracle-goldengate-to-elasticsearch/)







### oracle product log 統攬
- [oracle-log](https://docs.oracle.com/cd/B14099_19/core.1012/b13995/log.htm)

 -  Oracle Application Development Framework ODL Yes ORACLE_HOME/bc4j/logs/OC4J_Name
 - BPEL Process Manager Text No ORACLE_HOME/integration/orabpel/system/logs
 - CM SDK Text No ORACLE_HOME/ifs/cmsdk/log/cmsdk_domain_name/DomainController.log ORACLE_HOME/ifs/cmsdk/log/cmsdk_domain_name/node_name.log 
 - DCM ODL Yes ORACLE_HOME/dcm/logs
 - Discoverer Text No ORACLE_HOME/discoverer/logs
 - Enterprise Manager Text No ORACLE_HOME/sysman/log
 - Forms Text No ORACLE_HOME/j2ee/OC4J_BI_FORMS/application -deployments/formsapp/island/application.log
 - HTTP Server Text Yes ORACLE_HOME/Apache/Apache/logs/error_log.time
 - InterConnect Text No ORACLE_HOME/oai/10.1.2/adapters/adapter_name/logs
 - Integration B2B ODL Yes ORACLE_HOME/ip/log
 - BPEL Process Analytics Text No ORACLE_HOME/integration/bam/log
 - Log Loader ODL Yes ORACLE_HOME/diagnostics/logs
 - OC4J instance_name Text Yes ORACLE_HOME/j2ee/instance_name/log
ORACLE_HOME/j2ee/instance_name/application-deployments/application_name/application.log 
 - OCA Text No From the command line, for administrator use only, messages are stored at: ORACLE_HOME/oca/logs/admin.log
 - Oracle Internet Directory Text No ORACLE_HOME/ldap/log
 - OPMN Text No ORACLE_HOME/opmn/logs ORACLE_HOME/opmn/logs/component_type~...
 - Port Tunneling Text No ORACLE_HOME/iaspt/logs
 - Reports Server Text No ORACLE_HOME/reports/logs
 - Single Sign-On Text No ORACLE_HOME/sso/log

