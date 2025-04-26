# Re-labling 

Re-labeling -- allows you to classify/filter Prometheus targets and metrics by rewriting their label set. 


## Re-labling Template in Prometheus.yml 

```yml 
scrape_configs:
  - job_name: EC2

    # here we need to know: relabeling occurs before scrape occurs, and only has access to labels that added by Service Discovery 
    relabel_configs: 

    # metric_relabel_configs relabeling after the scrape 
    metric_relabel_configs:
    ec2_sd_configs:
      - region: <region>
        access_key: <access key>
        secret_key: <secret key>
```

## Configure Re-labeling in Prometheus.yml 
- case-1
```yml 
scrape_configs:
  - job_name: example 
    relabel_configs:
      - source_labels: [__meta_ec2_tag_env] # source_labels - is an array of labels to match on 
        regex: prod   # regex is used to set regular expressions to match on a specific value 
        action: keep | drop | replace  # actions here, keep- mean we will scrape target, if it has the source_label, drop means we will not scrape target 
```

- case-2
```yml 
scrape_configs:
  - job_name: example 
    relabel_configs:
      - source_labels: [__meta_ec2_tag_env]
        regex: dev 
        action: drop 
```

This configuration means we gonna drop any targets wehre statisfy `__meta_ec2_tag_env=dev`


- case-3: config two labels 
```yml 
scrape_configs:
  - job_name: example 
    relabel_configs:
      - source_labels: [env, team]
        regex: dev; marketing
        action: keep 
```

> When there are more than 1 source labels selected, they will be joined by a ; 