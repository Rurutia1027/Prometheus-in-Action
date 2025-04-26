# Push Gateway 

**Batch Jobs** only run for a short period of time and then exit which makes it difficult for Prometheus to scrape. 
That's the reason or the problem that Promethueus team design **push gateway**. Push gateway acts as the middleman between the batch job and the Prometheus server. It just like a cache that store the short term batch jobs' running metrics, and once prometheus wanna fetch the metrics it will scrape metrics regularly from the **push gateway** -- even though at that time the batch jobs are already deprecated and their resources are released. 

## Push Gateway Installation 


