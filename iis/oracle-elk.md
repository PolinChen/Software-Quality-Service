
## Oracle log file

- alert.log
 - select value from v$parameter where name = 'background_dump_dest'
 - file path: /usr/lib/oracle/xe/app/oracle/admin/XE/bdump
 - file path: /u01/app/oracle/diag/dbms/orcl/orcl/trace
- Active Session History (ASH) file 
 - file path: 
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

