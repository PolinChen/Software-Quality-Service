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
