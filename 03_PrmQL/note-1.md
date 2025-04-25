# PromQL Introduction 

## What is PromQL 
- Short for Prometheus Query Language 
- Main way to query metrics within Prometheus  
- Data returned cna be visualized in dashboards 
- Used to build **alerting** rules to notify administrators 

## PromQL Data Types 
- A PromQL expression can evaluate to one of four types: 

#### String
a simple string value (currently used)

#### Scalar
A simple numeric **floating point** value

#### Instant Vector 
Set of time series containing a single sample of each time series, all sharing the same timestamp

#### Range Vector 
Set of time series containing a range of data points over time for each time series. 