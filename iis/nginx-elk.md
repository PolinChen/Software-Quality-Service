## NGINX log

## log location
- /var/log/nginx/error.log
- /var/log/nginx/access.log

## [NGINX Log Analysis Use Cases](http://logz.io/blog/nginx-log-analysis/)

- dashboard for SEO
![ng2](http://logz.io/wp-content/uploads/2015/11/nginx-technical-seo-analysis.jpg)

- dashboard for access log
![ng1](http://logz.io/wp-content/uploads/2015/11/nginx_dashboard.jpg)

## [How to Monitor NGINX Logs on ELK Stack](https://miteshshah.github.io/linux/elk/how-to-monitor-nginx-logs-on-elk-stack/)

```
$ vim /etc/logstash/conf.d/nginx.conf
input {
  file {
    type => "nginx"
    start_position => "beginning"
    path => [ "/var/log/nginx/*.log" ]
  }
}
filter {
  if [type] == "nginx" {
    grok {
	patterns_dir => "/etc/logstash/patterns"
	match => { "message" => "%{NGINX_ACCESS}" }
	remove_tag => ["_grokparsefailure"]
	add_tag => ["nginx_access"]
    }
    grok {
	patterns_dir => "/etc/logstash/patterns"
	match => { "message" => "%{NGINX_ERROR}" }
	remove_tag => ["_grokparsefailure"]
	add_tag => ["nginx_error"]
    }
    geoip {
      source => "visitor_ip"
    }
  }
}
```

![ng3](https://cloud.githubusercontent.com/assets/1223371/9406579/5707fa08-481f-11e5-848a-d2ac7a184626.png)

## [Monitoring NGINX Plus Statistics with ELK](https://www.nginx.com/blog/monitoring-nginx-plus-statistics-elk/)

> Monitoring NGINX Plus performance and the health of your load‑balanced applications is critical, and acting on these metrics enables you to provide a reliable and satisfying user experience. Knowing how much traffic your virtual servers are receiving and keeping an eye on error rates allows you to triage your applications effectively.

![ng4](https://assets.wp.nginx.com/wp-content/uploads/2015/12/kibana-json-dashboard-1024x642.png)


## [Shipping your Nginx logs to Elasticsearch using Logstash](https://www.devops.zone/solr-elasticsearch/shipping-your-nginx-logs-to-elasticsearch-using-logstash/)

your /etc/logstash/logstash.conf like so:
```
input {
  file {
    path => "/var/log/nginx/*access*"
  }
}
filter {
  mutate { replace => { "type" => "nginx_access" } }
  grok {
    match => { "message" => "%{NGINXACCESS}" }
  }
  date {
    match => [ "timestamp" , "dd/MMM/YYYY:HH:mm:ss Z" ]
  }
  geoip {
    source => "clientip"
  }
}
output {
  elasticsearch {
    host => "localhost"
    protocol => "http"
  }
  stdout { codec => rubydebug }
}
```
