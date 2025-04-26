# Lab - Re-Labeling 

## Task-1
For the `demo` job, configure `re-label` configs to scrape only `targets` with `env="prod"` label and `drop` all other targets. 

- modify prometheus.yml relabel content 

```yml 
  - job_name: "demo"
    file_sd_configs:
      - files:
          - /etc/prometheus/file-sd.json
    relabel_configs: # add relabel configs under "demo" job 
      - source_labels: [env]
        regex: prod
        action: keep
```

Execute shell command to restart prometheus service. 

```shell 
systemctl restart prometheus 
```


## Task-2
We have decided to `scrape` the `metrics` from targets that have the following labels only: 
- team=`api`
- env=`prod`

Make the required changes for `demo` job.

- modify prometheus.yml file and configure two labels filter conditions 
```yml 
  - job_name: "demo"
    file_sd_configs:
      - files:
          - /etc/prometheus/file-sd.json
    relabel_configs:
      - source_labels: [team;env]
        regex: api;prod
        action: keep
```

Execute shell command to restart prometheus service.  

```shell 
systemctl restart prometheus 

systemctl status prometheus 
root@prometheus-server ~ ➜  systemctl status prometheus 
● prometheus.service - Prometheus
     Loaded: loaded (/etc/systemd/system/prometheus.service; enabled; vendor preset: enabled)
     Active: active (running) since Sat 2025-04-26 08:01:49 GMT; 3s ago
       Docs: https://prometheus.io/docs/introduction/overview/
   Main PID: 4233 (prometheus)
      Tasks: 20 (limit: 77143)
     Memory: 27.7M
     CGroup: /system.slice/prometheus.service
             └─4233 /usr/local/bin/prometheus --config.file=/etc/prometheus/prometheus.yml --storage.tsdb.path>

Apr 26 08:01:49 prometheus-server prometheus[4233]: ts=2025-04-26T08:01:49.690Z caller=head.go:683 level=info >
Apr 26 08:01:49 prometheus-server prometheus[4233]: ts=2025-04-26T08:01:49.695Z caller=head.go:683 level=info >
Apr 26 08:01:49 prometheus-server prometheus[4233]: ts=2025-04-26T08:01:49.696Z caller=head.go:683 level=info >
Apr 26 08:01:49 prometheus-server prometheus[4233]: ts=2025-04-26T08:01:49.696Z caller=head.go:720 level=info >
Apr 26 08:01:49 prometheus-server prometheus[4233]: ts=2025-04-26T08:01:49.697Z caller=main.go:1014 level=info>
Apr 26 08:01:49 prometheus-server prometheus[4233]: ts=2025-04-26T08:01:49.697Z caller=main.go:1017 level=info>
Apr 26 08:01:49 prometheus-server prometheus[4233]: ts=2025-04-26T08:01:49.697Z caller=main.go:1197 level=info>
Apr 26 08:01:49 prometheus-server prometheus[4233]: ts=2025-04-26T08:01:49.782Z caller=main.go:1234 level=info>
Apr 26 08:01:49 prometheus-server prometheus[4233]: ts=2025-04-26T08:01:49.782Z caller=main.go:978 level=info >
Apr 26 08:01:49 prometheus-server prometheus[4233]: ts=2025-04-26T08:01:49.782Z caller=manager.go:944 level=in>
```


## Task-3 
Currently, there is a label that follows the format `team=<team-name>`

- team=`api`
- team=`database`

Re-label this label so the label name changes to the `organziation` and the value gets prepended with `org-` text. 


- Example: 
organization=org-api 
organization=org-database 

Modify the `prometheus.yml`

```yml 
  - job_name: "demo"
    file_sd_configs:
      - files:
          - /etc/prometheus/file-sd.json
    relabel_configs:
      - source_labels: [team]
        regex: (.*)
        action: replace
        target_label: organization
        replacement: org-$1
```

- Execute command to refresh prometheus service 
  
```shell 
root@prometheus-server ~ ➜  vim /etc/prometheus/prometheus.yml 

root@prometheus-server ~ ➜  systemctl restart prometheus 

root@prometheus-server ~ ➜  systemctl status prometheus 
● prometheus.service - Prometheus
     Loaded: loaded (/etc/systemd/system/prometheus.service; enabled; vendor preset: enabled)
     Active: active (running) since Sat 2025-04-26 08:07:00 GMT; 4s ago
       Docs: https://prometheus.io/docs/introduction/overview/
   Main PID: 4705 (prometheus)
      Tasks: 20 (limit: 77143)
     Memory: 30.6M
     CGroup: /system.slice/prometheus.service
             └─4705 /usr/local/bin/prometheus --config.file=/etc/prometheus/prometheus.yml --storage.tsdb.path>

Apr 26 08:07:00 prometheus-server prometheus[4705]: ts=2025-04-26T08:07:00.783Z caller=head.go:683 level=info >
Apr 26 08:07:00 prometheus-server prometheus[4705]: ts=2025-04-26T08:07:00.786Z caller=head.go:683 level=info >
Apr 26 08:07:00 prometheus-server prometheus[4705]: ts=2025-04-26T08:07:00.786Z caller=head.go:683 level=info >
Apr 26 08:07:00 prometheus-server prometheus[4705]: ts=2025-04-26T08:07:00.786Z caller=head.go:720 level=info >
Apr 26 08:07:00 prometheus-server prometheus[4705]: ts=2025-04-26T08:07:00.788Z caller=main.go:1014 level=info>
Apr 26 08:07:00 prometheus-server prometheus[4705]: ts=2025-04-26T08:07:00.788Z caller=main.go:1017 level=info>
Apr 26 08:07:00 prometheus-server prometheus[4705]: ts=2025-04-26T08:07:00.788Z caller=main.go:1197 level=info>
Apr 26 08:07:00 prometheus-server prometheus[4705]: ts=2025-04-26T08:07:00.789Z caller=main.go:1234 level=info>
Apr 26 08:07:00 prometheus-server prometheus[4705]: ts=2025-04-26T08:07:00.789Z caller=main.go:978 level=info >
Apr 26 08:07:00 prometheus-server prometheus[4705]: ts=2025-04-26T08:07:00.789Z caller=manager.go:944 level=in>
lines 1-20/20 (END)
```

## Task-4
The `type` label is no longer needed, set up a `relabel policy` for the `demo` job to drop this label. 

```yml 
  - job_name: "demo"
    file_sd_configs:
      - files:
          - /etc/prometheus/file-sd.json
    relabel_configs:
      - regex: type
        action: labeldrop
```

## Task-5
By default, the labels that start with `__` will get dropped after the `re-labeling` process. Now, we want to keep all of those `labels` as well and change the name so it removes the `__meta_` text from the name.

For example change: 
`__meta_os__=centos` => `os=centos`
`__meta_mem__=8000mb` => `mem=8000mb`

Use the `labelmap` action to assign these `discovered` labels as `target` labels. 
Make the required changed under `demo` job. 

```yml 
  - job_name: "demo"
    file_sd_configs:
      - files:
          - /etc/prometheus/file-sd.json
    relabel_configs:
      - regex: __meta_(.*)__
        action: labelmap
        replacement: $1
```

## Task-6

It has been determined that the metric `node_network_transmit_drop_total` is no longer needed, configure a `metric_relable` policy to drop this metric. 

```yml 
  - job_name: "demo"
    file_sd_configs:
      - files:
          - /etc/prometheus/file-sd.json
    metric_relable_configs:
      - source_labels: [__name__]
        regex: node_network_transmit_drop_total
        action: drop
```

## Task-7

Update the Prometheus configuration to rename the metric `node_network_receive_bytes_total` to `node_network_rx_bytes_total`

```yml 
    metric_relabel_configs:
      - source_labels: [__name__]
        regex: node_network_receive_bytes_total
        action: replace
        target_label: __name__
        replacement: node_network_rx_bytes_total
```

## Task-8
The `node_filesystem_files` metric has a label called `fstype`. Drop this label.

```yml 
  - job_name: "demo"
    file_sd_configs:
      - files:
          - /etc/prometheus/file-sd.json
    metric_relabel_configs:
      - regex: fstype
        action: labeldrop
```

## Task-9
For network related metrics like `node_network_receive_bytes_total`, the `device` label provides the interface. 
Rename this label to `interface` instead of `device`, and then delete the old `device` label as well so that there isn't an extra unnecessary label. 

```yml 
  - job_name: "demo"
    file_sd_configs:
      - files:
          - /etc/prometheus/file-sd.json
    metric_relabel_configs:
      - source_labels: [device]
        regex: (.*)
        action: replace
        target_label: interface
        replacement: $1
```