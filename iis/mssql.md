## SQL server logging file with ELK

### SQL server error log 

檢視 SQL Server 錯誤記錄檔，以確定處理序順利完成 (例如，備份與還原作業、批次命令或其他指令碼和處理序)。這有助於偵測任何目前的或潛在的問題區域，包括自動復原訊息 (尤其是 SQL Server 的執行個體已停止又重新啟動時)、核心訊息或其他伺服器層級的錯誤訊息。

- error log location : D:\Program Files\Microsoft SQL Server\MSSQL10_50.SQL2K8R2\MSSQL\Log\ERRORLOG 和 ERRORLOG.n 檔案中。
 ![sp2](https://sqlserverhelpdotcom.files.wordpress.com/2011/06/log-folder.png)

- sp_readerrorlog sample
![sp1](https://az787680.vo.msecnd.net/user/jamesfu/1407/809ff1cf837b_A8E9/image_thumb.png)


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
