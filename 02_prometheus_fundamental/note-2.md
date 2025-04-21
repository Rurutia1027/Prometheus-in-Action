# Prometheus Basics 

## What's Prometheus 
- **Prometheus** is an open-source monitoring tools that collects metrics data, and provide tools to visualize the collected data. In addition, Prometheus allows you to generate **alerts** when metrics** reach a user specified threshold. 

- In addition, Prometheus allows you to generate **alerts** when metrics reach a user speicifed thresholdPending . 

- Prometheus collects metrics by scraping targets who expose metrics through an HTTP endpoint. 

- Scraped metrics are then stored in a time series database which can be queried using Prometheus' built-in query language **PromQl**.

## What kind of metrics can Prometheus Monitor? 

- CPU/Memory Utilizaiton 
- Disk space
- Service Uptime 
- Application specific data 
  - Number of exceptions 
  - Latency 
  - Pending Requests 

## What type of data is Prometheus Designed to Track ? 

Prometheus is designed to monitor **time-series** data that is **numeric**.


## What type of data should Prometheus avoid Monotiring ? 
- Events 
- System logs 
- Traces  


## Background of Prometheus 
- It was originally sponsored by SoundCloud but in 2016 it joined the Cloud Native Computing Foundation.  
- It is primarily written GoLang
- Documentation for Prometheus can be found at prometheus.io/docs 