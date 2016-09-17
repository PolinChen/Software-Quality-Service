## logging for weblogic logs

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
