# Prometheus Metrics 

## Prometheus Metrics 
- metrics pattern 
```
<metric_name>[{<label_1="value1">, <label_2="value2">, <label_3="value3">}] <metric_name>
```

- metrics numeric example 
```shell 
node_cpu_seconds_total{cpu="0",mode="idle"} 2342454.32
node_cpu_seconds_total{cpu="1",mode="iowait"} 934274.32
node_cpu_seconds_total{cpu="2",mode="irq"} 42342454.32
node_cpu_seconds_total{cpu="3",mode="nice"} 56323.32

# here the labels show us the information on which cpu this metric is for 
# and what's the state of the cpu (idle/busy)
```


## Prometheus Metric Types 

### Counter 
- Tells you how many times did 'X' happen  
- Counter's number can only increase 

We can use Counter this metric type to trace total endpoint requests. 

### Gauge 

- Tells you what is the current value of X 
- Can go up or down 

We can use this kind of metric type to trace **current CPU utilization**, **available system memory**, and the **number of concurrent requests**. 

### Histogram 
- How long and how big something is. 
- Groups observations into configurable bucket sizes.  

For example, we can use **histogram** to configure buckets like create a histogram metric type with 3 buckets `< 1s` , `< 0.5s` and `< 0.2s` to trace the system response time. Then this metrics can show us how many requests in total finish their invocation in `less than 1 seconds`, how many finish in `less than 0.5 seconds`, and how many requests finish in `less than 0.2 seconds`. 

It can also be adopted to describe **request size**, by configuring the buckets like `< 1500 MB`, `< 1000 MB` and `< 8000 MB`.


### Summary 
- Summary metric is very similar to histogram metirc, they all designed to track how long or how big.  
- How many observations fell below X. 
- Don't have to define quantiles ahead of time. 

Different from histogram, when we define a Summary to trace the **response time**, summary metric will show us the trace results like: 
```
20% = 0.3 s --> 20% of the total request, took 0.3 seconds to finish 
50% = 0.8 s
80% = 1s 
```


## Prometheus Lables 

- Lables are Key-Value pairs associated with a metric 
- Lables allow you to split up a metric by a specified criteria
- Metric can have more than one lable
- ASCII letters, numbers, unerscores can be included in lables 
- Labels must match regex [a-zA-Z0-9_]*

**Every metric is assigned 2 lables by default(one for instance and the other for job)**

```yaml 
job_name: "node" # --> this is for the job = "node" this lable-1
scheme: https
basic_auth:
  username: admin 
  password: admin 
static_configs:
  - targets:
    ["127.0.0.1:9100"]  # --> this is for the instance="127.0.0.1:9100" this label-2 
```