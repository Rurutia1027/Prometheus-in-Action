# Selectors & Matchers 

### Selector
A query with just the metric name will return all time series with that metric. 


What if we only want to return a subset of times series for a metric?  -- Use Label Metrics. 

### Equality Matcher 

- Return all time series from node1
```text 
node_filesystem_avail_bytes{instance="node1"}

# this queries will match all time series from node01 
```

- Return all time series where device is not equal to "tmpfs" 

```text 
node_filesystem_avail_bytes{device!="tmpfs"}

# here the != matcher will ensure that only devices that are not "tmpfs" will be filtered and returned 
```


### Regex Matcher 
- Return all time series where device starts with "/dev/sda" (sda2 & sda3) 

```text 
node_filesystem_avail_bytes{device=~"/dev/sda.*"}

# To match anything that starts with /dev/sda, a regex "/dev/sda.*" will need to be used. 
```

### Negative Regex Matcher 
- Return all time series where mountpoint does not start with "/boot"

```text
node_filesystem_avail_bytes{mountpoint!~"/boot.*"}
```

### Multiple Selectors
- Return all time series from node1 without a "device=tmpfs"

```text 
node_file_avail_bytes{instance="node1", device!="tmpfs"}

# multiple selectors can be used by separating them by a comma  
```


### Range Vector Selectors 
- Returns all the values for a metric over a period of time 
```text 
node_arp_entries{instance="node01"}[2m]
```





