# SLO/SLA/SLI

When designing a system or applications, its imporatnt for teams to set specific measurable targets/goals to help organizations strike the right balance between product development and oepraiton work.

These targets help customers & end users quantify the level of reliability they should come to expect from a service.

## SLI - Service Level Indicator

It's quantitative measure of some aspect of the level of service that is provided.

### Common SLIs

- Request Latency
- Error Rate
- Saturation
- Throughput
- Availability

Not all metrics make for good SLIs. You want to find metrics that accrately measure a **user's** experience. Things like high-cpu/high-memory make for a pool SLI as a user might not see any impact on their end during these events.

## SLO - Service Level Object

It's target value or range of an SLI

- Example of Setting SLI & SLO about Latency
  SLI - Latency
  SLO - Latency < 100ms

- Example of SLI & SLI about Availability
  SLI - availability
  SLI - 99.9% uptime

## Tips of SLOs & SLIs Setting

- SLOs should be directly related to the **customer experience**. The main purppose of the SLO is to quantify reliability of a product to a customer.
- For SLOs it maybe tempting to set them to aggressive values like 100% uptime, however this will come at a higher cost.  
- The goal is not to achieve perfection but instead to make customers happy with the right level of reliability. 
- If a customer is happy with 99% reliability, then increasing it any further doesn't add any other value. 


## SLA - Service Level Aggrement
- Service Level Aggrement (SLA) - contract between a vendor and a user that guarantees a certain SLO.
- The consequences for not meeting any SLO cna be financial based but can also be variety of other things as well.