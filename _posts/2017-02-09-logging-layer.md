---
layout: post
title:  "logging layer"
date:   Thu Feb  9 17:03:32 +03 2017
author: Tuncay UYSAL
categories: jekyll update
---





* # Step One

 be ready for pre-installation previously

{% highlight bash %}

nano /etc/syslog.conf
*.*                     @127.0.0.1:5000
sudo killall -HUP syslogd

Apple Script logs (asl)
/etc/asl.conf

Audit logs
/etc/security/audit_control

{% endhighlight %}

**however** i'll make catch system events with `collectd`

```
brew install collectd
nano /usr/local/Cellar/collectd/5.7.1/etc/collectd.conf

```

* # Step Two

 brew install elasticsearch logstash kibana

 list of caveats

{% highlight text linenos %}
Data:    /usr/local/var/elasticsearch/elasticsearch_Tuncay/
Logs:    /usr/local/var/log/elasticsearch/elasticsearch_Tuncay.log
Plugins: /usr/local/opt/elasticsearch/libexec/plugins/
Config:  /usr/local/etc/elasticsearch/
plugin script: /usr/local/opt/elasticsearch/libexec/bin/elasticsearch-plugin

To have launchd start elasticsearch now and restart at login:
  brew services start elasticsearch
Or, if you don't want/need a background service you can just run:
  elasticsearch

Please read the getting started guide located at:
  https://www.elastic.co/guide/en/logstash/current/getting-started-with-logstash.html

Config: /usr/local/etc/kibana/
If you wish to preserve your plugins upon upgrade, make a copy of
/usr/local/opt/kibana/plugins before upgrading, and copy it into the
new keg location after upgrading.

To have launchd start kibana now and restart at login:
  brew services start kibana
Or, if you don't want/need a background service you can just run:
  kibana


you can take info anytime => brew info package

{% endhighlight %}

* # Step Three

start of services

`logstash -f /Users/Tuncay/Amazon Drive/logstash.conf`

{% highlight text linenos %}
input {
    udp {
        port => 5000
        type => syslog
    }
}


filter {
    if [type] == "syslog" {
        grok {
            match => { "message" => "%{SYSLOGTIMESTAMP:syslog_timestamp} %{SYSLOGHOST:syslog_hostname} %{DATA:syslog_program}(?:\[%{POSINT:syslog_pid}\])?: %{GREEDYDATA:syslog_message}" }
            add_field => [ "received_at", "%{@timestamp}" ]
            add_field => [ "received_from", "%{host}" ]
        }
        syslog_pri { }
        date {
            match => [ "syslog_timestamp", "MMM  d HH:mm:ss", "MMM dd HH:mm:ss" ]
        }
    }
}


output {
    elasticsearch { 
      hosts => ["localhost:9200"] 
    }
}
{% endhighlight %}




* # Tricks

#to read a specific file

`sudo /usr/bin/syslog -f /private/var/log/asl/2015.11.20.G80.asl`

#to see all sudo usage

`sudo /usr/bin/syslog -k Sender sudo`

#to see all critical messages

`sudo /usr/bin/syslog -k Level Nle 2`

SEVERITY LOGGING LEVEL
: 0 Emergency
: 1 Alert
: 2 Critical
: 3 Error
: 4 Warning
: 5 Notice
: 6 Informational informational messages
: 7 Debug debug-level messages

<p> </p>

AUDIT REDUCE
: auditreduce -- select records from audit trail files

#show all activity from root

`sudo /usr/sbin/auditreduce -e root /var/audit/current | praudit | tail`

#show user authentication activity

`sudo /usr/sbin/auditreduce -m AUE_auth_user, /var/audit/current | praudit`

#show logins

`sudo /usr/sbin/auditreduce -m AUE_lw_login, /var/audit/current | praudit`

#show logouts

`sudo /usr/sbin/auditreduce -m AUE_logout /var/audit/current | praudit`

<p> </p>  https://github.com/lmenezes/elasticsearch-kopf

https://mtalavera.wordpress.com/2015/02/16/monitoring-with-collectd-and-kibana/  

SYSTEM LOG FILES
: **Main folder:** /var/log/
: **Apple System Log:** /var/log/asl/
: **Audit Log:** /var/audit/
: **User Logs:** ~/Library/Logs/ 
: **Application Logs:** /Library/Logs/

---

for [reference]

[reference]: http://macadmins.psu.edu/wp-content/uploads/sites/24696/2016/06/psumac2016-19-osxlogs_macadmins_2016.pdf