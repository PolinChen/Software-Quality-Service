## Window Server Log 


### Centralized Monitoring system expected feature list:

 [Real Time Operational Insights using ELK Stack](http://www.hcltech.com/blogs/real-time-operational-insights-using-elk-stack)

- Capable of combining data from multiple sources and different formats
- Capable enough to process GBs of logs daily and support Real-Time or Near Real Time Data analysis and dashboard.
- Support correlation, aggregation, and filtering of data on-the-fly
- Support reliable and secure data transmission and should be Fault Tolerant
- Generate custom Events/Notification.
- Should support modern responsive visualization with support of all latest browsers

#### Architecture

![ms1](http://www.hcltech.com/sites/default/files/images/elk_stack.png)

#### Performance dashboard

![ms2](http://www.hcltech.com/sites/default/files/images/image3.jpg)

#### Windows Event Log  dashboard

![ms3](http://www.hcltech.com/sites/default/files/images/monitoring_dashboard.png)

- [Sending Windows Event Logs to Logstash](https://blog.rootshell.be/2015/08/24/sending-windows-event-logs-to-logstash/)

![ms4](https://blog.rootshell.be/wp-content/uploads/2015/07/eventlog-in-logstash.png)


### [Sending your Windows Event Logs to Logsene using NxLog and Logstash](https://sematext.com/blog/2016/02/01/sending-windows-event-logs-to-logsene-using-nxlog-and-logstash/)

- The Architecture

![The Architecture](https://sematext.com/wp-content/uploads/2016/01/nxlog-logstash-logsene-1024x381.png)

- Setting up Logstash

```
input {
  tcp {
    port => 3515
    codec => "line"
    type => "WindowsEventLog"
  }
}

filter {
  if [type] == "WindowsEventLog" {
    json {
      source => "message"
    }
    if [SourceModuleName] == "eventlog" {
      mutate {
        replace => [ "message", "%{Message}" ]
      }
      mutate {
        remove_field => [ "Message" ]
      }
    }
  }
}

output {
  elasticsearch {
    hosts => "logsene-receiver.sematext.com:443"
    ssl => "true"
    index => "YOUR_TOKEN"
    manage_template => false
  }
}
```

- Visualizing Your Logs

![vis1](https://sematext.com/wp-content/uploads/2016/01/live-tail-1024x405.png)
![vis2](https://sematext.com/wp-content/uploads/2016/01/dashboard-1024x400.png)

