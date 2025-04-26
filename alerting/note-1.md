# Alerting 
- Prometheus allows you to define **conditions**, that if met trigger alerts. 
- These conditions are defined as standard **PromQL** expressions.

```shell 
node_filesystem_avail_bytes < 1000 

node_filesystem_avail_bytes{device="tmpfs", instance="node1", mountpoint="/run/lock"} 547 # this value will trigger alert 
```

- What we need to know is Prometheus is only responsible for triggering alerts. 
- PromQL defines the alert trigger rules, sending alert messages, and alert message format is not the duty of Prometheus.  
- Sending alert messages responsibility is offloaded onto a separate process called **AlertManager**. 