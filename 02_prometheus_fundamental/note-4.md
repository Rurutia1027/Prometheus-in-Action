# Prometheus Configuration 

## Prometheus Configuration Template 
```yml 
# Default parameters for all other config sections 
global:
  scrape_interval: 1m
  scrape_timeout: 10s 

# Define targets & configs for metrics collection 
scrape_configs:
  # A collection of instances that need to be scraped 
  - job_name: 'node'
    # Configs for scrape job. Takes precedence over global config 
    scrape_interval: 15s
    scrape_timeout: 5s
    sample_limit: 1000 

    # set of targets to scrape 
    static_configs:
      - targets: ['localhost:9090']

# configuration related to AlertManager 
alerting: 


# Rule files specifies a list of files rules are read from 
rule_files: 

# Settings related to the remote read/write feature 
remote_read:
remote_write: 

# Storage related settings 
storage: 
```


## Prometheus Configuration Example 

```yml 
scrape_configs:
  # here we define a job with name of 'nodes'
  - job_name: 'nodes'
    
    # here we define metrics should be scraped every 30 seconds 
    scrape_interval: 30s 
    
    # timeout of scrape is 3s 
    scrape_timeout: 3s 
    
    # here we configure use HTTPs instead of default HTTP 
    scheme: https

    # scrape path should be changed from default /metrics 
    # to /stats/metrics  
    metrics_path: /stats/metrics

    # here we declare Prometheus gonna scrape two targets at the following IP
    static_configs:
      - targets: ['localhost:9090', 'localhost:9091']
```