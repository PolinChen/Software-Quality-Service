# MySQL Slow Query log Monitoring using Beats & ELK

## [MySQL Slow Query log Monitoring using Beats & ELK](http://www.slideshare.net/YoungHeonKim1/mysql-slow-query-log-monitoring-using-beats-elk) - slideshare


## [MySQL慢查询日志](http://kibana.logstash.es/content/logstash/examples/mysql-slow.html)

```
input {
  file {
    type => "mysql-slow"
    path => "/var/log/mysql/mysql-slow.log"
    codec => multiline {
      pattern => "^# User@Host:"
      negate => true
      what => "previous"
    }
  }
}

filter {
  # drop sleep events
  grok {
    match => { "message" => "SELECT SLEEP" }
    add_tag => [ "sleep_drop" ]
    tag_on_failure => [] # prevent default _grokparsefailure tag on real records
  }
  if "sleep_drop" in [tags] {
    drop {}
  }
  grok {
    match => [ "message", "(?m)^# User@Host: %{USER:user}\[[^\]]+\] @ (?:(?<clienthost>\S*) )?\[(?:%{IP:clientip})?\]\s*# Query_time: %{NUMBER:query_time:float}\s+Lock_time: %{NUMBER:lock_time:float}\s+Rows_sent: %{NUMBER:rows_sent:int}\s+Rows_examined: %{NUMBER:rows_examined:int}\s*(?:use %{DATA:database};\s*)?SET timestamp=%{NUMBER:timestamp};\s*(?<query>(?<action>\w+)\s+.*)\n# Time:.*$" ]
  }
  date {
    match => [ "timestamp", "UNIX" ]
    remove_field => [ "timestamp" ]
  }
}
```


## [LOGSTASH+ELASTICSEARCH处理MYSQL慢查询日志](http://www.wklken.me/posts/2016/05/24/elk-mysql-slolog.html)


