# Prometheus in Action 
**Prometheus in Action** is a hands-on observability lab focused on real-world use of Prometheus within **Cloud Native** environments. This repo aims to bridge core Prometheus with cloud-native monitoring patterns, showcasing how to build production-ready observability stacks step-by-step. 

Whether you're monitoring containers, services, or infrastructure, this project helps you understand and apply the best practices in metrics collection, querying, alerting, and visualization - all while staying aligned with CNCF observability principles. 

---

## Role of Prometheus in Cloud Native's Observability 

![image](https://github.com/user-attachments/assets/42af4a34-8ebd-4698-bcfd-f8c10f5b6a93)


- [Prometheus & Cloud Native Observability 101](https://medium.com/devops-dev/prometheus-cloud-native-observability-101-3b630e34cd86)

---

## What This Repo Covers 
This repo guides you through the essential layers of Prometheus-powered observability: 

- **Observibility Basics**: Understand the 3 pillars - logs, metrics, traces - and Prometheus's role in metrics. 
- **Prometheus Architecture**: TSDB, pull-based collection, scrape targets, and storage. 
- **Prometheus Setup**: Local installation, Docker Compose setup, and config best practices. 
- **Metrics Deep Dive**: Counter, Gauge, Histogram, Summary - when and how to use each.
- **PromQL Mastery**: Selectors, modifiers, operators, functions, and quantiles. 
- **Exporters**: Node Exporter, App-level instrumentation, and custom metric endpoints.
- **Grafana & Dashboards**: Real-time visualization of Prometheus data.
- **Alerting**: Setup alert rules and integrate with Alertmanager.
- **Push Gateway**: Handle short-lived jobs in a push-based pattern.
- **Service Discovery**: Dynamically detect targets in container environments.
- **Monitoring Kubernetes**: Auto-discovery, kube-state-metrics, cSdvisor, kube-prometheus-stack.

--- 

## Quickstart (Local lab)
> Setup Prometheus + Exporters in seconds: 

```bash 
git clone https://github.com/Rurutia1027/prometheus-in-action.git
cd prometheus-in-action
docker compose up -d
```

- Prometheus UI: http://localhost:9090
- Node Exporter: http://localhost:9100/metrics

--- 

## Cloud Native Focus 

This repo is designed with **Cloud Native Observability** in mind: -
- Built around **CNCF best practices**
- Containerized workflows with **Docker Compose**
- Ready for **Kubernetes extensions**
- Aligns with **GitOps-friendly** monitoring setups
- Can evolve into **Prometheus Operator** or **kube-prometheus-stack** setups

--- 

## Learning Outcome 
By working through this repo, you'll be able to -
- Build and configure a Prometheus stack from scratch
- Write production-grade PromQL queries
- Expose and collect application metrics
- Visualize metrics and alert on key conditions
- Understand how Prometheus fits into cloud-native infrastructure 
  
---

## References
- [KodeKloud-Prometheus-Certificate](https://learn.kodekloud.com/user/courses/prometheus-certified-associate-pca)
- [Prometheus Certified Associate (PCA) Exam](https://github.com/cncf/curriculum/blob/master/PCA_Curriculum.pdf)
