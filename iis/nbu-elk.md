## NBU log file 

## [NetBackup MSDP log files](https://www.veritas.com/support/en_US/article.000052938#v44319473)

- The bpbrm backup and restore manager. The following is the path to the log files:

 - UNIX: /usr/openv/netbackup/logs/bpbrm

 - Windows: install_path\Veritas\NetBackup\logs\bpbrm

- The bpdbm database manager. The following is the path to the log files:

 - UNIX: /usr/openv/netbackup/logs/bpdbm

 - Windows: install_path\Veritas\NetBackup\logs\bpdbm

- The bptm tape manager for I/O operations. The following is the path to the log files:

 - UNIX: /usr/openv/netbackup/logs/bptm

 - Windows: install_path\Veritas\NetBackup\logs\bptm

- [log file path](https://www.veritas.com/support/en_US/article.TECH75805)
 Consideration may be given to creation of separate filesystems for the main log areas, /usr/openv/netbackup/logs and /usr/openv/logs. 


- [detail for NBU log file](http://itdoc.hitachi.co.jp/manuals/oem/veritas/JP1V110VERINBU02000/NetBackup7.7_RefGuide_Logging_e.PDF)

- Application log messages
```
05/02/10 11:02:01.717 [Warning] V-116-18 failed to connect to nbjm, will retry
```

- Diagnostic log messages
```
05/05/09 14:14:30.347 V-116-71 [JobScheduler::doCatIncr] no configured session based incremental catalog schedules
```

- Debug log messages
```
10/29/09 13:11:28.065 [taolog] TAO (12066|1) - Transport_Cache_Manager::bind_i, 0xffbfc194 -> 0x7179d0 Transport[12]
```

- File name format for unified logging
 -   /usr/openv/logs/nbpem/51216-116-2201360136-041029-0000000000.log

- file location
```
Windows install_path\NetBackup\logs
UNIX /usr/openv/logs
```

## [VOX log information](https://vox.veritas.com/t5/NetBackup/configuer-logs-for-backup-failure/td-p/348267)

```
admin – Keeps the logs of adminstrative commands
bpbrm – Keeps the logs of backup and restore manager
bpcd - Keeps the logs of client daemon
bpdbjobs - Keeps the logs of database manager program process
bpdm - Keeps the logs of disk manager process
bpjava-msvc - Keeps the logs of Java application server authentication service
bprd - Keeps the logs of request daemon process
bpsched - Keeps the logs of scheduler process that runs on master servers
bptm - Keeps the logs of media management process
bparchive - Keeps the logs of archive program
bpbackup - Keeps the logs of backup program
bpbkar - Keeps the logs of program that generates golden images
bplist - Keeps the logs of program that lists backed up and archived files
user-ops - directory for use by Java programsbp – client user interface process
```
