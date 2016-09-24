## Linux log
 參考內容： http://blog.chinaunix.net/uid-26569496-id-3199434.html


- **/var/log/secure**：記錄登入系統存取資料的檔案，例如 pop3, ssh, telnet, ftp 等都會記錄在此檔案中；
- **/var/log/messages**：這個檔案相當的重要，幾乎系統發生的錯誤訊息（或者是重要的資訊）都會記錄在這個檔案中；
- **/var/log/boot.log**：記錄開機或者是一些服務啟動的時候，所顯示的啟動或關閉訊息；
- **/var/log/maillog** ：紀錄郵件存取或往來( sendmail 與 pop3 )的使用者記錄；

- **/var/log/cron** ：這個是用來記錄 crontab 這個例行性服務的內容的！
- **/var/log/lastlog** : 日誌文件記錄最近成功登錄的事件和最後一次不成功的登錄事件，由login生成。
- **/var/run/utmp** : 該日誌文件記錄有關當前登錄的每個用戶的信息。
- **/var/log/wtmp**：記錄登入者的訊息資料，由於本檔案已經被編碼過，所以必須使用 last 這個指令來取出檔案的內容；
- **/var/log/btmp** – 記錄所有失敗登錄信息。使用last命令可以查看btmp文件。
- **/var/log/xferlog** : 該日誌文件記錄FTP會話，可以顯示出用戶向FTP服務器或從服務器拷貝了什麼文件。
- **/var/log/dmesg**  — 包含內核緩衝信息（kernel ring buffer）。在系統啓動時，會在屏幕上顯示許多與硬件有關的信息。可以用dmesg查看它們。
- **/var/log/auth.log** — 包含系統授權信息，包括用戶登錄和使用的權限機制等。
- **/var/log/daemon.log** — 包含各種系統後台守護進程日誌信息。
- **/var/log/dpkg.log** – 包括安裝或dpkg命令清除軟件包的日誌。
- **/var/log/user.log** — 記錄所有等級用戶信息的日誌。
- **/var/log/anaconda.log**  — 在安裝Linux時，所有安裝信息都儲存在這個文件中。
- **/var/log/faillog** – 包含用戶登錄失敗信息。此外，錯誤登錄命令也會記錄在本文件中。

除了上述Log文件以外， /var/log還基於系統的具體應用包含以下一些子目錄：
- **/var/log/httpd/或/var/log/apache2** — 包含服務器access_log和error_log信息。
- **/var/log/lighttpd/** — 包含light HTTPD的access_log和error_log。
- **/var/log/mail/** – 這個子目錄包含郵件服務器的額外日誌。
- **/var/log/prelink/** — 包含.so文件被prelink修改的信息。
- **/var/log/audit/** — 包含被 Linux audit daemon儲存的信息。
- **/var/log/samba/** – 包含由samba存儲的信息。
- **/var/log/sa/* — 包含每日由sysstat軟件包收集的sar文件。
- **/var/log/sssd/** – 用於守護進程安全服務。


kk
- **/var/log/secure**：記錄登入系統存取資料的檔案，例如 pop3, ssh, telnet, ftp 等都會記錄在此檔案中；
- **/var/log/messages**：這個檔案相當的重要，幾乎系統發生的錯誤訊息（或者是重要的資訊）都會記錄在這個檔案中；
- **/var/log/boot.log**：記錄開機或者是一些服務啟動的時候，所顯示的啟動或關閉訊息；
- **/var/log/maillog** ：紀錄郵件存取或往來( sendmail 與 pop3 )的使用者記錄；

- **/var/log/cron** ：這個是用來記錄 crontab 這個例行性服務的內容的！
- **/var/log/lastlog** : 日志文件记录最近成功登录的事件和最后一次不成功的登录事件，由login生成。 
- **/var/run/utmp** : 该日志文件记录有关当前登录的每个用户的信息。
- **/var/log/wtmp**：記錄登入者的訊息資料，由於本檔案已經被編碼過，所以必須使用 last 這個指令來取出檔案的內容；
- **/var/log/btmp** – 记录所有失败登录信息。使用last命令可以查看btmp文件。
- **/var/log/xferlog** : 该日志文件记录FTP会话，可以显示出用户向FTP服务器或从服务器拷贝了什么文件。
- **/var/log/dmesg**  — 包含内核缓冲信息（kernel ring buffer）。在系统启动时，会在屏幕上显示许多与硬件有关的信息。可以用dmesg查看它们。
- **/var/log/auth.log** — 包含系统授权信息，包括用户登录和使用的权限机制等。
- **/var/log/daemon.log** — 包含各种系统后台守护进程日志信息。
- **/var/log/dpkg.log** – 包括安装或dpkg命令清除软件包的日志。
- **/var/log/user.log** — 记录所有等级用户信息的日志。
- **/var/log/anaconda.log**  — 在安装Linux时，所有安装信息都储存在这个文件中。
- **/var/log/faillog** – 包含用户登录失败信息。此外，错误登录命令也会记录在本文件中。

除了上述Log文件以外， /var/log还基于系统的具体应用包含以下一些子目录：
- **/var/log/httpd/或/var/log/apache2** — 包含服务器access_log和error_log信息。
- **/var/log/lighttpd/** — 包含light HTTPD的access_log和error_log。
- **/var/log/mail/** – 这个子目录包含邮件服务器的额外日志。
- **/var/log/prelink/** — 包含.so文件被prelink修改的信息。
- **/var/log/audit/** — 包含被 Linux audit daemon储存的信息。
- **/var/log/samba/** – 包含由samba存储的信息。
- **/var/log/sa/* — 包含每日由sysstat软件包收集的sar文件。
- /var/log/sssd/ – 用于守护进程安全服务。




- /var/log/httpd
- /var/log/news
- /var/log/mysqld.log
- /var/log/samba
- /var/log/procmail.log
- /var/log/auth.log

