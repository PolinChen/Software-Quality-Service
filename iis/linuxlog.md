## Linux log
 參考內容： http://blog.chinaunix.net/uid-26569496-id-3199434.html
- /var/log/secure：記錄登入系統存取資料的檔案，例如 pop3, ssh, telnet, ftp 等都會記錄在此檔案中；- /var/log/wtmp：記錄登入者的訊息資料，由於本檔案已經被編碼過，所以必須使用 last 這個指令來取出檔案的內容；
- /var/log/messages：這個檔案相當的重要，幾乎系統發生的錯誤訊息（或者是重要的資訊）都會記錄在這個檔案中；
- /var/log/boot.log：記錄開機或者是一些服務啟動的時候，所顯示的啟動或關閉訊息；
- /var/log/maillog 或 /var/log/mail/*：紀錄郵件存取或往來( sendmail 與 pop3 )的使用者記錄；
- /var/log/mail/info
- /var/log/mail/warnings
- /var/log/mail/errors
- /var/log/cron：這個是用來記錄 crontab 這個例行性服務的內容的！
- /var/log/httpd
- /var/log/news
- /var/log/mysqld.log
- /var/log/samba
- /var/log/procmail.log
- /var/log/auth.log
- /var/log/messages

