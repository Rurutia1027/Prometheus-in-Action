# Promtools 

## What's Promtools ? 

Promtools is a utility tool shipped with Prometheus that can be used to: 
- Check and validate configuration 
  - Validate Prometheus.yml 
  - Validate rules files 
- Validate metrics passed to it are correctly formatted 
- Can perform queries on a Prometheus server 
- Debugging & Profiling a Prometheus server  
- Perform unit tests against Recording/Alerting rules  


## How to use Promtools to Validate config files ? 

```shell 
promtool check config /etc/prometheus/prometheus.yml 
```


Prometheus configs can now be validated before applying them to a production server

This will prevent any unnecessary downtime while config issues are being identified 

As we progress through this course, we will cover all of the other features Promtools has to offer 