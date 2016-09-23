## logging for weblogic logs




### [日誌文件說明](http://wangrusheng5200.iteye.com/blog/474055)
 
WebLogic SERVER運行日誌
  
假如 WebLogic SERVER 在啓動或運行過程中有錯誤發生， 錯誤信息會顯示在屏幕上，並且會記錄在一個LOG文件中，該文件默認名為AdminServer.log。該文件也記錄WebLogic的啓動及關閉等其他運行信息。可在Gernal屬性頁中設置該文件的路徑及名字，錯誤的輸出的等級等。
 
HTTP訪問日誌
在WebLogic中可以對用HTTP，HTTPS協議訪問的服務器上的文件都做記錄，該LOG文件默認的名字為Access.log,內容如下，該文件具體記錄在某個時間，某個IP地址的客戶端訪問了服務器上的那個文件。

DOMAIN運行日誌
記錄一個DOMIAN的運行情況，一個DOMAIN中的各個 WebLogic SERVER可以把它們的一些運行信息（比如：很嚴重的錯誤）發送給一個DOMAIN的ADMINISTRATOR SERVER上，ADMINISTRATOR SERVER把這些信息些到 DOMAIN 日誌中。


## [Overview of WebLogic Server Log Messages and Log Files](https://docs.oracle.com/cd/E13222_01/wls/docs81/ConsoleHelp/logging.html#1048117)
>Each subsystem within WebLogic Server generates server log messages to communicate its status. For example, when you start a WebLogic Server instance, the Security subsystem writes a message to report its initialization status.
>To keep a record of the messages that its subsystems generate, WebLogic Server writes the messages to log files. The server log file is located on the computer that hosts the server instance. By default, the server log file is located below the server instance's root directory: root-directory\server-name\server-name.log. For more information, refer to Changing the Name and Location of the Server Log File.
>To view messages in a server log file, you can log on the WebLogic Server host computer and use a standard text editor, or you can log on to any computer and use the log file viewer in the Administration Console. For more information, refer to Viewing Server Logs.

- [managing Logs in weblogic](http://otechmag.com/magazine/2015/spring/ahmed-aboulnaga.html)

### weblogic log description

- file type : Weblogic server and application log
- log path

```
input {
  # server logs
  file {
    type => "weblogic"
    tags => "weblogic"
    path => [ "/data/logfiles/weblogic/*/*/weblogic.log" ]
    codec => plain { charset => "ISO-8859-1" }
  }
  # application logs
  file {
    type => "application"
    tags => "weblogic"
    path => [ "/data/logfiles/weblogic/*/*/app1.log",
              "/data/logfiles/weblogic/*/*/app2.log",
              "/data/logfiles/weblogic/*/*/app3.log",
              "/data/logfiles/weblogic/*/*/app4.log" ]
    codec => plain { charset => "ISO-8859-1" }
  }                                                                                                                                                           
}

```

- [weblogic1](https://kuther.net/blog/indexing-and-searching-weblogic-logs-using-logstash-elasticsearch-and-kibana)

## sample for ELK 

- [用ELK监控系统请求和错误](http://blog.csdn.net/xeseo/article/details/38817315)

```
现在的系统，动则分布式，分布式情况下，错误的定位比传统的单机要麻烦很多。因为log文件分布在不同的feature的不同节点。一直想找一种方式来简化这个过程。
目前主流的log监控有：
Splunk:商业收费
syslog-ng:漏数据
flume：java写的，有点重
chukwa：专为Hadoop服务
scribe：java写的，略重
zabbix、ganglia：全面的系统监控，大材小用
graylog2：不详，没来得及研究
```

Kibana有一个问题，它列出来的fields只能按照一个field进行排序。事实上Elasticsearch支持组合排序。这个问题已经进入了Kibana的JIRA，有望在未来解决。
贴一个我定义的问题收集界面：
![web1](http://img.blog.csdn.net/20140825104045173?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveGVzZW8=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
![web2](http://img.blog.csdn.net/20140825104048730?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveGVzZW8=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)


- [prasing weblogic](http://beyondoraclesoa.blogspot.tw/2013/12/logstash-for-weblogic-part-iii-using.html)

```
"####<%{DATA:wls_timestamp}> <%{WORD:severity}> <%{DATA:wls_topic}> %{HOST:hostname}> <(%{WORD:server})?> %{GREEDYDATA:logmessage}"
```

![web2](http://1.bp.blogspot.com/-7PfR8jE6zLw/UsFHNje1-CI/AAAAAAAAACU/giCLZQhWnas/s1600/screenshot11.png)
