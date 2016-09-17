## APACHE Traffic Server log

## apache log location
- /var/log/httpd/access.log
- /var/log/httpd/error.log


## [Apache Log Analyzer: Elasticsearch, Logstash, and Kibana](http://logz.io/blog/apache-log-analyzer/)

```
if [type] in [ "apache" , "apache_access" , "apache-access" ]  {
   grok {
      match =&gt; [
      "message" , "%{COMBINEDAPACHELOG}+%{GREEDYDATA:extra_fields}",
      "message" , "%{COMMONAPACHELOG}+%{GREEDYDATA:extra_fields}"
      ]
      overwrite =&gt; [ "message" ]
   }
 
   mutate {
      convert =&gt; ["response", "integer"]
      convert =&gt; ["bytes", "integer"]
      convert =&gt; ["responsetime", "float"]
   }
 
   geoip {
      source =&gt; "clientip"
      target =&gt; "geoip"
      add_tag =&gt; [ "apache-geoip" ]
   }
 
   date {
      match =&gt; [ "timestamp" , "dd/MMM/YYYY:HH:mm:ss Z" ]
      remove_field =&gt; [ "timestamp" ]
   }
 
   useragent {
      source =&gt; "agent"
   }
 }
```

- Use Case #1: Operational Analysis
![ap1](http://logz.io/wp-content/uploads/2016/03/apache-log-analyzer.png)

- Use Case #2: Technical SEO
i![ap2](http://logz.io/wp-content/uploads/2016/03/apache-server-technical-seo.jpg)

- Visualizing Apache Logs
![ap3](http://logz.io/wp-content/uploads/2016/03/apache-elk-apps.jpg)

-  [Logstash Configuration Examples](https://www.elastic.co/guide/en/logstash/current/config-examples.html#config-examples)




### APACHE Traffic Server log

- [Understanding Traffic Server Log Files](https://docs.trafficserver.apache.org/en/latest/admin-guide/monitoring/logging/understanding.en.html)

>
Traffic Server records information about every transaction (or request) it processes and every error it detects in log files. Traffic Server keeps three types of log files:

>  - Error log files record information about why a particular transaction was in error.
>  - Event log files (also called access log files) record information about the state of each transaction Traffic Server processes.
>  - System log files record system information, including messages about the state of Traffic Server and errors/warnings it produces. This kind of information might include a note that event log files were rolled, a warning that cluster communication timed out, or an error indicating that Traffic Server was restarted.

