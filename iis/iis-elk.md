## sample 404 for dashboard

### IIS log file detail description

- [Log Files for a Web site](https://www.iis.net/configreference/system.applicationhost/sites/site/logfile)



- [HTTP Error Codes graph](https://discuss.elastic.co/t/how-to-change-the-color-of-visualization-based-on-error/2800) 
 note:
  - error code for query 同時顯示不同的pie，製作方式
  - 定義404 為紅色的定義
	![iis1](https://discourse-cdn.global.ssl.fastly.net/elastic/uploads/default/optimized/1X/8d8251170117b94116608860d8787863a75d2383_1_690x350.png)

- [Analyze web traffic with Squid proxy & ELK](https://medium.com/@thomasdecaux/analyze-web-traffic-with-squid-proxy-elasticsearch-logstash-kibana-stack-e2a471e34bc4#.2gf6qyv2s)

 note:
 - cache_access_log /var/log/squid3/cache-access.log
 - cache_log /var/log/squid3/cache.log
 - cache_store_log /var/log/squid3/store.log
 - 如何整合 proxy log + apache log
![iis2](https://cdn-images-1.medium.com/max/1800/1*2KX0o7aUCDwF8j6d5vmVDw.jpeg)


- [Powerful IIS/Apache Monitoring dashboard using ElasticSearch+Grafana](http://www.codeproject.com/Articles/1094405/Powerful-IIS-Apache-Monitoring-dashboard-using) 
 note:
 - 每個metric 的定義問題
 - Grafana 的dashboard
   ![apache1](http://www.codeproject.com/KB/applications/1094405/More_Web_graph.png)

 - Architecture:

   ![apacheA](http://www.codeproject.com/KB/applications/1094405/Architecture.png)

- [IIS Log Analyzer: logz.io](http://logz.io/blog/iis-log-analyzer/)

 ![logz-iis](http://logz.io/wp-content/uploads/2016/11/nginx-business-intelligence.png)

- [Elastic Stack: Process IIS Logs](http://www.secureict.info/2016/07/elastic-stack-process-iis-logs.html) -- good sample
- :wq!
