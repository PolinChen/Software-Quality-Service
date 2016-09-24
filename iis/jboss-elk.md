## Jboss log file with ELK

### Jboss log description

- [detail information](https://docs.jboss.org/jbossas/guides/installguide/r1/en/html/dirs.html)
 - default path: /jboss-4.0.4/server/some_server_conf/log/
![jboss-path](https://docs.jboss.org/jbossas/guides/installguide/r1/en/html/images/jboss_directory_structure.jpg)

- sample of jboss server log
 - [Logging Configuration](https://docs.jboss.org/author/display/AS71/Logging+Configuration#LoggingConfiguration-DefaultLogFileLocations)
 - [Example That Sets Up Different Logging Levels](http://developers.redhat.com/quickstarts/eap/logging/)
```
    FATAL [org.jboss.as.quickstarts.logging.LoggingExample] (default task-1) THIS IS A FATAL MESSAGE
    ERROR [org.jboss.as.quickstarts.logging.LoggingExample] (default task-1) THIS IS AN ERROR MESSAGE
    WARN  [org.jboss.as.quickstarts.logging.LoggingExample] (default task-1) THIS IS A WARNING MESSAGE
    INFO  [org.jboss.as.quickstarts.logging.LoggingExample] (default task-1) THIS IS AN INFO MESSAGE
    ERROR [org.jboss.as.quickstarts.logging.LoggingExample] (default task-1) THIS IS AN ERROR MESSAGE
    FATAL [org.jboss.as.quickstarts.logging.LoggingExample] (default task-1) THIS IS A FATAL MESSAGE
    INFO  [org.jboss.as.quickstarts.logging.LoggingExample] (default task-1) THIS IS AN INFO MESSAGE
    WARN  [org.jboss.as.quickstarts.logging.LoggingExample] (default task-1) THIS IS A WARNING MESSAGE
```
## jboss for ELK

- [GETTING STARTED WITH ELK AND JBOSS EAP6](https://blog.akquinet.de/2015/08/24/logstash-jboss-eap/)

```
input {
  log4j {
    mode => "server"
    host => "0.0.0.0"
    port => 4712
    type => "log4j"
  }
}
 
output {
  elasticsearch {
    host => "127.0.0.1"
    cluster => "elasticsearch-logstash"
    index => "logstash-%{+YYYY.MM.dd}"
  }
  #stdout { codec => rubydebug }
}
```

 - dashboard sample

![jboss1](https://akquinetblog.files.wordpress.com/2015/08/screen-shot-2015-07-16-at-15-23-17.png?w=300&h=208)

## sample 2 for ELK

- [Getting Started with ELK Stack on WildFly](http://planet.jboss.org/post/getting_started_with_elk_stack_on_wildfly)

 - ![jboss2](http://blog.arungupta.me/wp-content/uploads/2015/07/logstash-processing-pipeline.png)
 - ![jboss3](http://blog.arungupta.me/wp-content/uploads/2015/07/elk-stack.png)
 - ![jboss4](http://blog.arungupta.me/wp-content/uploads/2015/07/elk-stack-wildfly-output-1024x562.png)
 - ![jboss5](http://wildfly.org/images/2015-07-25-kibana.png)

## jboss for IBM LA

- [Web Access Log Insight 套件](http://www.ibm.com/support/knowledgecenter/zh-tw/SSPFMY_1.3.3/com.ibm.scala.doc/extend/iwa_extend_weblogs_ovw.html)


## [JEAP 6 - Domain Mode 啟動](http://wei-meilin.blogspot.tw/2012_07_01_archive.html)

- ![log sample](http://3.bp.blogspot.com/-GfYJJOppUEc/UA38QMAeUHI/AAAAAAAAAOY/4R1egmKxrwE/s640/螢幕快照+2012-07-24+上午9.36.29.png)
