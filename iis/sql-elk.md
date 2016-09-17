## SQL server logging file with ELK

### SQL server log file description


- [log file sample](https://sqlserver-help.com/2011/06/26/help-where-is-sql-server-errorlog/)
使用 SQL Server Management Studio 或任何文字編輯器來檢視 SQL Server 錯誤記錄檔。根據預設，錯誤記錄檔是位於 Program Files\Microsoft SQL Server\MSSQL.n\MSSQL\LOG\ERRORLOG 和 ERRORLOG.n 檔案中。

 - location : D:\Program Files\Microsoft SQL Server\MSSQL10_50.SQL2K8R2\MSSQL\Log\
 ![sp2](https://sqlserverhelpdotcom.files.wordpress.com/2011/06/log-folder.png)

- sp_readerrorlog sample
![sp1](https://sqlserverhelpdotcom.files.wordpress.com/2011/06/errorlog.png)

- [Identify location of the SQL Server Error Log file](https://www.mssqltips.com/sqlservertip/2506/identify-location-of-the-sql-server-error-log-file/)

 ![erro1](https://www.mssqltips.com/tipimages2/2506_image004.png)
 ![erro2](https://www.mssqltips.com/tipimages2/2506_image005.png)

- [Parse sql log from log file using logstash](http://stackoverflow.com/questions/27543568/parse-sql-log-from-log-file-using-logstash)
 - path : C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\Log\ERRORLOG*'
