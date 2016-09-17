
## Oracle log file

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

