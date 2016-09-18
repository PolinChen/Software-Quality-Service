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

