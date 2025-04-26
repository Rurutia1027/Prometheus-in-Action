# Labels && Annotations 

## Labels' Usage in Prometheus Alert Rules 

**Labels** can be added to alerts to provide a mechanism to classify and match specific alerts in Alertmanager. 

- rules.yml 
```yml 
groups:
  - name: node 
    rules:
      - alert: Node down 
        expr: up{job="node"} == 0
        labels: 
          severity: warning 
      - alert: Multiple Nodes down
        expr: avg without(instance)(up{job="node"}) <= 0.5 
        labels: 
          severity: cirtical 
```



## Annotations' Usage in Prometheus Alert Rules 
**Annotations** can be used to provide additional information, however, unlike labels they do not play a part in the alerts identity. 

- rules.yml with alert's annotation 
```yaml 
groups:
  - name: node 
    rules: 
      - alert: node_filesystem_free_percent 
        expr: 100 * node_filesystem_free_bytes{job="node"} / node_filesystem_size_bytes{job="node"} < 70 
        annotations: 
          description: "filesystem {{.labels.device}} on {{.Labels.instance}} is low on space, current available space is {{.Value}}"
```
