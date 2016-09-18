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

- [CIFS Peformance problem](http://community.netapp.com/t5/Data-ONTAP-Discussions/CIFS-performance-problem/td-p/78390)

I normally use “stats show” and “sysstat” command to collect the Netapp storage performance data and do self analysis.
But now I was trying to see which all volumes have more write and which all volumes have more read operations in a period of say 7 days or 15 days.


```
CPU    NFS   CIFS   HTTP      Net kB/s     Disk kB/s      Tape kB/s    Cache
                               in   out     read  write    read write     age
82%   7127   9003      0    8177 39226    31073   2606       0     0       1
91%   6785   9091      0   12189 37056    34302  12137       0     0       1
84%   9599   9360      0    9488 32152    29497   6466       0     0       1
96%   7827   9637      0   15959 38404    33400  16630       0     0       1
91%   9361  10466      0    6490 34312    27940   3994       0     0       1
90%   8826  10128      0    4958 32662    29347   4403       0     0       1
96%   6339   9484      0   10731 34338    32835   8322       0     0       1
94%   5309   9929      0   13300 35917    35079  15151       0     0       1
cifs stat 10
  GetAttr      Read     Write      Lock   Open/Cl    Direct     Other
113626497782  33474482130  1294930546  767394214  60353397035  9863767165  26313299
    46118     13394       475       301     24960      4007        11
    46332     14480       564       346     24191      3978        10
    42864     11059       402       243     20880      3534        26
    43499     12055       430       281     21502      3585        18
    43934     11888       403       297     21675      3573         9
    45586     13033       373       279     24417      3877        24
    45639     12739       432       276     24351      3889        10
    48137     14694       564       359     26145      4356         6
    50359     15435       541       326     29034      4658        25
```

- [How to collect Performance Statistics for intermittent issues](https://kb.netapp.com/support/index?page=content&id=1012616)

**gPerfstat GUI (Windows, Linux, Mac)**

Before using this setup, ensure a sucessful perfstat can be collected using the GUI. If using the gPerfstat GUI an extended perfstat collection can be initiated, this isnt a true rolling perfstat but this can collect multiple perfstat over multiple days and facilitate capturing intermitten issues if the time set covers the issue. The GUI can be setup in multiple ways but the following is an example.

This example sets 18 iterations by 5 minutes, which will collect a minumum of 90 minutes. Setting the "Runs" to 20 will collect 20 perfstats back to back, 90 minutes each covering about 30 hours.

![net4](https://kb.netapp.com/library/CUSTOMER/2016-08-04%2011_10_42-Perfstat%208_3%20GUI.png)


**Linux/Unix/Mac**

**Perfstat7 (Data ONTAP 7-Mode):**

```
#!/bin/sh
SCRIPTDIR="/home/andrew/Downloads"
OUTDIR="/home/andrew/Downloads"
CASENUM="2005559999"
while true
do
DS=`date '+%Y%m%d%H%M%S'`
OUTFILE="$ _perfstat_$ .out"
#echo "output file name: $OUTFILE"
$ /perfstat.sh -S -l root -f 10.216.4.55 -t 5 -i 12 -I > $ /$ 
done
```
