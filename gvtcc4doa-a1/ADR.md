# Architecture Decision Record

## Requirements

> ... health product manufacturing company ... on-premises infrastructure ... eager to explore the benefits of migrating to a cloud native infrastructure

If we want the solution to be accepted, it's important that we minimize friction in the migration proposal, that means keeping in mind the effort required for the IT ops team in reskilling etc.

We want to provide the bare minimum of granularity and control with the lowest migration effort

> ... a private network

Immediately, we know we need a VPC, but that's generally a given

> ... being a B2C company

The user experience is key, more than that of B2B businesses

> ... 3-tier architecture

With a presentation, application and data tiers

> ... needs their applications to be accessible over the internet

Self-explanatory

> ... aims to reduce the administrative burden of managing their database ... prefer a managed and highly scalable database

So a managed RDBS

> ... encounters medium to high traffic levels daily during business hours ... need of a cost-effective solution that can dynamically scale to meet varying workload demands automatically

So some form of autoscaling based on traffic levels

> ... is interested in recommendations for various metrics that can be monitored to enhance customer satisfaction

Increased latency impacts customer satisfaction

We will definitely want to implement monitoring, especially synthetics

## Second-order questions
- what are our RTO and RPOs?
- what is our max tolerable latency?
- how are the presentation and application tier organized? will they be served separately (i.e SPA) or tightly coupled/monolithic (templates within an application)
- what is expected traffic like? do we expect reads > writes? are we expecting a high volume of quick requests, or a lower volume of compute-intensive tasks?
- what is the variance in workload expected to be?

## Architecture

### Network

- VPC with 2 AZs
- private + public subnet in each AZ
- single load balancer

### Application Tier

- ECS with Fargate

### Data Tier

- Amazon RDS General Purpose 3 (SSD) Storage
- Multi-AZ Instance
- db.m5.large
- 200 GB allocated storage with storage autoscaling

### Miscellaneous
- ECR for image storage

### Considerations & Assumptions
- no mention that this is a high-performance workload
    - exclude IOPS (SSD)
    - we also don't need to use a multi-az cluster
- we assume request/second has low variance
    - avoid burstable instance types
- assumes a somewhat monolithic application, that can be containerized
    - excludes Lambda, this would require significant retooling of the application (extracting presentation and rewriting the controllers etc.)
    - excludes EKS, there is added operational overheads and reskilling, but can be easily adopted later
    - excludes App Runner, this is a relatively new service but can be easier adopted later
    - excludes Elastic Beanstalk, this service in a LTS-esque state
    - excludes EC2, while this is probably the most familiar service, you lose on all the potential administration savings by going cloud