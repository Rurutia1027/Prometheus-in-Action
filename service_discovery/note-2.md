# Service Discovery in AWS EC2 

We all know that, cloud infrastructure is very dynamic especially when autoscaling is enabled. 

EC2 Service Discovery makes it so Prometheus has a list of all available **EC2** instances to scrape. 

 ## Setup EC2 Service Discovery in Prometheus 
 - `prometheus.yml` 
 ```yml 
 scrape_configs:
   - job_name: EC2
     ec2_sd_configs:
       - region: <region>
         access_key: <access_key>
         secret_key: <secret key>
 ```

 Credentials should be setup with an IAM user with the **AmazonEC2ReadOnlyAccess** policy. 

 ## EC2 Service Discovery 
 THe EC2 service discovery was able to extract metadata for each EC2 instance it found: 
 - `__meta_ec2_tag_Name="web"`: is the Name tag for the instance
 - `__meta_ec2_instance_type="t2.micro"`: Is the instance type 
 - `__meta_ec2_vpc_id="vpc-345fw23"`: is the VPC instance's location 
 - `__meta_ec2_private_ip="23.243.34.33"`: is the private IP address for the instance. 

The instance label defaults to using the private ip. This is done due to the assumption that Prometheus will be running as "close" as possible to what it is monitoring. 

In addition, not all EC2 instances have public IPs. 

