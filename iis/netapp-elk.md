## netapp log file

## [netapp log 描述]()

The following list outlines the various log files and their locations used by Data ONTAP. In addition, ONTAP also uses syslogd daemon to log system messages (and uses the config file /etc/syslog.conf)

```
Messages: /etc/log/messages (symbolic link to /etc/messages)  
SnapMirror: /etc/log/snapmirror  
FlexClone: /etc/log/clone  
Auditlog: /etc/log/auditlog  
Deduplication: /etc/log/sis  
LACP: /etc/log/lacp_log  
Backup: /etc/log/backup and /etc/log/ndmpdlog  
FTP: /etc/log/ftp.cmd and /etc/log/ftp.xfer  
Shelf Messages: /etc/log/shelflog/shelflog_ata and /etc/log/shelflog_esh  
Volume Operations: /etc/log/vol_history  
Crash Files: /etc/log/crash/aggregates/<aggregatename>/  
Performance Archives: /etc/log/stats/archives  
ACP: /etc/log/acp/acplog_master and /etc/log/acplog  
```

## [How to manually collect all the logs from a clustered Data ONTAP cluster](https://kb.netapp.com/support/index?page=content&id=1012523)

## [Access Your NetApp Clustered Data ONTAP Logs From Your Browser](https://erailine.com/2014/11/07/access-your-netapp-clustered-data-ontap-logs-from-your-browser/)

![net1](https://erailine.files.wordpress.com/2014/11/screen-shot-2014-11-07-at-10-55-14-pm.png?w=1008)
