# Introduction in Associate Exam

## Global Infrastructures

### Compute

EC2, Lambda

### Storage

### Databases

Relational Database like RDS is available.

Non Relational Database is also available. Like DynamoDB, RedShift

### Migration & Transfer

Snowball

### Network & Content Delivery

CloudFront (CDN) or Route 53 or VPC

### Security, Identity & Compliance

Identity Access Management

### Others

Robotics, Block-chain, Machine Learning...

## Fundamentals

### Region

It is just a geographical Area. Physical location in the world.

Each region will always consist of multiple Availability Zones.

### Availability Zone

It is just (one or more) discrete data center. It is filled with servers,,, There could be multiple data centers in a AZ. But they would be very close to each other.

Each has redundant power, and redundant connectivity. (Boosts Availability)

### Edge locations ($\neq$ region)

Endpoints for AWS which are used for **caching** content. This consists of CloudFront(Content Delivery Network).

Number of Edge locations > Number of AZs > Number of Regions


