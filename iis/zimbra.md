## zimbra (mail server) log


- [zimbra1](https://wiki.zimbra.com/wiki/Centralized_Logs_-_Elasticsearch,_Logstash_and_Kibana)

The Logstash, Elasticsearch and Kibana will be installed on this dedicated VM, in the Zimbra Server, or servers, will be installed the Agent.

![zm2](https://wiki.zimbra.com/images/8/89/Zimbra-kibana-logstash-Diagram.png)

- multitype log format

```
{
"network": {
"servers": [ "IPDEVUESTROLOGSTASH:5000" ],
"timeout": 15,
"ssl ca": "/etc/pki/tls/certs/logstash-forwarder.crt"
},
"files": [
{
"paths": [
"/var/log/syslog",
"/var/log/auth.log",
"/opt/zimbra/log/mailbox.log",
"/opt/zimbra/log/nginx.access.log",
"/opt/zimbra/log/nginx.log",
"/var/log/zimbra.log",
"/var/log/mail.log"
],
"fields": { "type": "syslog" }
}
]
}
```

- dashboard sample:

![zm2](https://wiki.zimbra.com/images/thumb/8/8d/Zimbra-logstashkibana-002.png/1600px-Zimbra-logstashkibana-002.png)
