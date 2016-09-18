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

## Forwarding NetApp Event Log Entries via Syslog(http://www.monitorware.com/common/en/articles/netapp-eventlog-syslog.php)

## [Monitoring NetApp Ontap Clusters](https://outsideit.net/check-netapp-ontap/)

![net2](https://outsideit.net/wp-content/uploads/2016/05/check_netapp_ontap_logical_view_01.png)

## [Performance Log](https://kb.netapp.com/support/index?page=content&id=3014366)

- How do I upload a Performance Archive?
Run the 'system node autosupport invoke-performance-archive' sub-command with the following options:
 - -start-date <"MM/DD/YYYY HH:MM:SS">
 - -duration <[<integer>h][<integer>m][<integer>s]> (or -end-date)
 - -node *
 - -case-number <text>
It is important to note that the maximum duration for any single collection is 6 hours and the recommended sample period is 4 hours. 

**For Example:**
Using an external monitoring tool (for example: OnCommand Performance Manager), a change in the latency trend for a cluster was identified between 6AM and noon on on November 1st 2014. According to the tool, an increase in latency occurred at 8AM and lasted approximately 4 hours. As the best data set covers the period before the issue is seen as well as the transition to when the latency trend changes, data should be collected from 6AM to 10AM.

This would provide a 2 hour sample of data before the issue was identified (6-8AM) and a 2 hour period (8AM-10AM) when the issue was occurring. Upon calling Customer Support, this issue was assigned case number 42:

```
system node autosupport invoke-performance-archive -start-date "11/1/2014 06:00:00" -duration 4h -case-number 42 -node *
```

