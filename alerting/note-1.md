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


## Define Alerting Rules via prometheus.yml 

### Case-1 

- Alerting rules are similar to recording rules and can be placed right along side recording rules within a rule group.

```yml 
groups:
  - name: node 
    interval: 5s
    rules:
      - record: node_memory_memFree_percent
        expr: 100 - (100 * node_memory_MemFree_bytes{job="node"} / node_memory_MemTotal_bytes{job="node"})
      - alert: LowMemory # here we define the alert name 
        expr: node_memory_memFree_percent < 20  # here we declare the alert rule 
```

### Case-2 Define Alert via `For`
- Define an alert that triggerred by k8s nodes down 

```yml 
groups:
  - name: node
    rules:
      - alert: node down
        expr: up{job="node"} == 0 
        for: 5m # the node is down for 5 total minutes -> then the alert will be triggered 
```


The **for** clause's syntax is telling Prometheus that an expression must evaluate to true for a specified period of time before firing alert. 

The example above expects the node to be down for **5 minutes** before firing an alert. 

Helps prevent against race conditions and scrape timeouts. Network issues could cause an individual scrape to fail, its best to specify a duration to avoid false alarms. 



## Alert States 
There are three alert states defined in alert syntax in Prometheus. 
- **Inactive**: Expression has not returned any results 
- **Pending**: expression returned results but it hasn't been long enough to be considered firing (5m in this case)
- **Firing**: active for more than the defined for clause (5m in this case).

```yml 
groups:
  - name: node 
    rules:
      - record: node down 
        expr: up{job="node"} == 0 
        for: 5m
```

