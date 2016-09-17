## logging for weblogic logs

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
