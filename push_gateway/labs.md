# Labs - Prometheus Push Gateway 
## Task-1 
Install Pushgateway. 

- download package

```shell 
wget https://github.com/prometheus/pushgateway/releases/download/v1.5.0/pushgateway-1.5.0.linux-amd64.tar.gz
tar xvfz pushgateway-1.5.0.linux-amd64.tar.gz
```

- add user and allocate authority 
```shell 
cp pushgateway-1.5.0.linux-amd64/pushgateway /usr/local/bin/
useradd -M -r -s /bin/false pushgateway
chown pushgateway:pushgateway /usr/local/bin/pushgateway
```

- add service 
```shell 
vi /etc/systemd/system/pushgateway.service
```

```service 
[Unit]
Description=Prometheus Pushgateway
Wants=network-online.target
After=network-online.target

[Service]
User=pushgateway
Group=pushgateway
Type=simple
ExecStart=/usr/local/bin/pushgateway

[Install]
WantedBy=multi-user.target
```

- finally start & enable pushgateway service 
```shell 

root@prometheus-server ~ ➜  systemctl enable pushgateway 
Created symlink /etc/systemd/system/multi-user.target.wants/pushgateway.service → /etc/systemd/system/pushgateway.service.

systemctl start pushgateway
```


## Task-2
Verify that the `Pushgateway` server is running by sending a request to the `/metrics` endpoint. 

```shell 
root@prometheus-server ~ ➜  curl localhost:9091/metrics
# HELP go_gc_duration_seconds A summary of the pause duration of garbage collection cycles.
# TYPE go_gc_duration_seconds summary
go_gc_duration_seconds{quantile="0"} 0.00012625
go_gc_duration_seconds{quantile="0.25"} 0.00012625
go_gc_duration_seconds{quantile="0.5"} 0.00012625
go_gc_duration_seconds{quantile="0.75"} 0.00012625
go_gc_duration_seconds{quantile="1"} 0.00012625
go_gc_duration_seconds_sum 0.00012625
```


## Task-3 
Configure Prometheus to scrape the `Pushgateway`. The job label should be `pushgateway`, and make sure that `hornor_labels` is set to `true` so that the clients can set the `job/instance` labels. 

```shell 
vim /etc/prometheus/prometheus.yml
```


```yml 
scrape_configs:
  # The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.
  - job_name: "prometheus"

    # metrics_path defaults to '/metrics'
    # scheme defaults to 'http'.

    static_configs:
      - targets: ["localhost:9090"]

  - job_name: "nodes"
    scheme: https
    tls_config:
      ca_file: /etc/prometheus/node_exporter.crt
      insecure_skip_verify: true
    basic_auth:
      username: prometheus
      password: secret-password
    static_configs:
      - targets: ["node01:9100", "node02:9100"]
        
  - job_name: "pushgateway"
    honor_labels: true
    static_configs:
      - targets: ["localhost:9091"]
```

## Task-4 
Using the command below, push the `processing_time_seconds 120` metic into a job labeled as `{job="video_processing"}`.

```
echo "processing_time_seconds 120" | curl --data-binary @- http://localhost:9091/metrics/job/video_processing
```