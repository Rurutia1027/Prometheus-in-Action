# Monitoring Containers 

## Container Metrics 

- Metrics can also be scraped from containerized environments 
- Docker Engine Metrics 
- Container metrics using cAdvisor 

## Configure Docker Engine Metrics 

- Step-1: Configure `daemon.json`

```json 
{
    "metrics-addr": "127.0.0.1:9323",
    "experimental": true
}
```

- Step-2: Configure Prometheus Config File 
```yml 
scrape_configs:
  - job_name: "docker"
    static_configs:
      - targets: ['localhost:9323']
```

- Step-3: Restart Docker 
```shell 
sudo systemctl restart docker 
```

- Step-4: Check Metric Endpoint 
```shell 
curl localhost:9323/metrics 
```


## Configure cAdvisor Metrics
- Step-1: Create `docker-compose.yml` file 

```yml 
version: '3.8'
servics:
  cadvisor:
    image: gcr.io/cadvisor/cadvisor 
    container_name: cadvisor 
    privileged: true 
    devices:
      - "/dev/kmsg:/dev/kmsg"
    volumes:
      - /:/rootfs:ro
      - /var/run:/var/run:ro
      - /sys:/sys:ro
      - /var/lib/docker/:/var/lib/docker:ro
      - /dev/disk/:/dev/disk:ro
    ports:
      - 8080:8080
```


- Step-2: Setup Docker Compose 
```shell 
docker-compose up 
```


- Step-3: Send Curl Request 
```shell 
curl localhost:8080/metrics 
```


- Step-4: Update Prometheus Configs 
```yml 
scrape_configs:
  - job_name: "cadvisor"
    static_configs:
      - targets: ['localhost:8080']
```

- Step-5: Restart promtheus services after modify configuration 
```shell 
sudo systemctl restart prometheus 

sudo systemctl status prometheus 
```

## Difference between Docker Metrics & cAdvisor Metrics 
### Docker Engine metrics 
- How many cpu does docker use 
- Total number of failed iamge biulds 
- Time to process container actions 
- No metrics specific to a container 


### cAdvisor metrics 
- How much cpu/mem does each container use 
- Number of processes running inside a container 
- Container uptime 
- Metrics on a per container basis 


