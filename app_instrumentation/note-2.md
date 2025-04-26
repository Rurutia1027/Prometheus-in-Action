# Best Practices 

## Naming Metrics 
- `library_name_unit_suffix`
Metrics should follow **snake_case** -- lowercase with words separated by '_', like `http_requests_total`.

The first word of the metric should be the application/library the metric is used for, like `postgres_`. 

Then the name portion of the metric name should provide a description of what the metric is used for. More can one word can be used, like `postgres_queue_size`.  

The unie should always be included in the metric name, so there's no second guessing which units its using. 

Make sure to use unprefixed base units like seconds, and bytes, and metrics. We don't want to use microsecons or kilobytes, like `postgres_queue_size_bytes`.

`_total`, `_count`, `_sum`, and `_bucket` suffixes are used by various metrics so its best to avoid adding those yourselves.  

One tip is, only exception is to add `_total` for counter metrics. 



## Proper Metric Names vs. Incorrect Metric Names 

### Proper 

- process_cpu_seconds 
- http_requests_total
- redis_connection_errors 
- node_disk_read_bytes_total 


### Incorrect 
- container_docker_restarts -> docker_container_restarts 
- http_request_sum 
- nginx_disk_free_kilobytes 
- dotnet_queue_waiting_time -> unit is missing 


```txt
what does 'instrument' mean in this context ? 

When we talk about "instrumentation" in the context of monitoring or performance tracking, we're referring to the process of adding tracking mechanisms or observability hooks to the sytem. In simpler terms, instrumenting a system means embedding code or configuration into the system to measure certain behaviors (like performance, errors, or resource usage) and send that data to a monitoring tool.
```

## What should be Instrument 

- Online-serving systems 
- Offline processing 
- Batch jobs 


### Online-serving Systems 

Online-serving system is one where an **immediate** response is expected. Databases and web-servers would fall into this category. 
- Number of Queries / Requests 
- Number of Errors 
- Latency  


### Offline Processing Service 
Offline processing service is one where no one is actively waiting for a response. Usually patch up work and have multiple stages in a pipeline. 

For each stage, we wanna to get : 
- Amount of queued work
- Amount of work in processing status 
- Rate of processing 
- errors  

### Batch Jobs 
Batch Jobs are similar to offline-serving systems with the main difference being Batch Jobs are run on a regular schedule whereas offline-serving systems run continuously. 

Will require a pushGateway as it isn't continuously running to be scrapped

- Time processing each stage of job 
- Overall runtime  
- Lasttime job completed. 


