# Service Discovery 

## What is Service Discovery 
Service Discovery allows Prometheus to populate a list of endpoints to scrape that can get dynamically updated as new endpoints get created & destroyed. 


## Service Discovery in Prometheus 
Prometheus has **built-in** support for several service discovery mechanisms, like EC2, Azure, GCE, Kubernetes, and so on. 


## Static 
- We say a service configured on Prometheus is 'static' it means define it as 'static_configs' under job label in prometheus.yml file, like:
```yml 
scrape_configs:
  - job_name: "web"
    static_configs:
      - targets: ["localhost:9090"]
  - job_name: "node"
    static_configs: 
      - targets: ["localhost:9100"]
```


## File Service Discovery 

File service discovery allows you to import a list of jobs/targets from a file, and this allows you to integrate with service discovery system Prometheus doesn't support out of box. 


### A Demo for File Service Discovery 
- file-sd.json 

```json 
[
    {
        "targets": ["node1:9100", "node2:9100"], 
        "labels": {
            "team": "dev",
            "job": "node"
        }
    }, 
    {
        "targets": ["localohst:9090"],
        "labels": {
            "team": "monitoring",
            "job": "prometheus"
        }
    }
]
```

- prometheus.yml 

```yml 
scrape_configs:
  - job_name: file-example
    file_sd_configs:
      - files: 
          - 'file-sd.json' # '*.json' <- this also supported =
```


