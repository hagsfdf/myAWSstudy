# 101 Database

## Relational Database

The most traditional method for scheme.

### Relational Database on AWS

- SQL server
- Oracle
- MySQL server
- PostgreSQL
- Aurora
- MariaDB

## RDS two key feature

- Multi-AZ : Disaster Recovery
- Read Replicas : For boosting performance

### Scenario

#### Multi AZ

We have our EC2 instance at the top, and it connects in to our databases using a **connection string** (it is just a DNS address)

By this DNS record, it points at a primary database. If it is lost in some reason, Amazon automatically modifies the DNS to point to the secondary database. 

With multi-AZ, failover is automatic.

#### Read Replicas

If you write to the primary database, that write will be done on the secondary database automatically(perfect copy of database).

There is no automatic failover here. If the primary DB fails, you have to make EC2 instance point to the replica DB and make a corresponding URL.

Suppose a case that you get a million requests per hour. You can scale your database by routing half of the requests to the read replica version of it.

## Non relational Database

Consist of :

- Collection (= Table)
- Document (= Row) (inside the collection)
- Key Value Pairs (= Fields) (a.k.a. column)

Adding the column in RDS sucks (it is part of the Data Definition Language)

It effects all around the table, but for in Non-RDS, there could be different column for each row.

## Data Warehousing

Tools like Cognos, SAP NetWeaver, Oracle Hyperion

Used to pull in very large and complex data sets. Usually used by management to do queries on data.

## OLTP vs OLAP

Online Transaction Processing differs from Online Analytics Processing in terms of **types of queries you will run**

**Bear in mind, it has a prefix "usually",**

- OLTP : queries for primary keys, have a single(not-many) result. Think of the scenario querying "Show me the date of record with record number 23032"
- OLAP : queries that require aggregation, grouping, etc. Pulls in large numbers of records.

Data Warehousing database use different type of architecture both from a database perspective and infrastructure layer. In order to deal with complicated queries in OLAP, Amazon's solution is **Redshift**.

## ElasticCache

Web service that makes it easy to deploy, operate, and scale an in-memory cache in the cloud. Allowing you to retrieve information from fast, managed, in-memory caches. Disk-base databases are much more slower.

 Say you own amazon.com, and the millions of customers would query "what's the top 10 purchased sales product?" Top 10 list wouldn't be constantly changing from time to time. You may just cache it in your memory! Caching your most common web queries to help overloaded DB.

 It supports two open-source in-memory caching engines.

 1. Memcached
 2. Redis

## Summary

### RDS (OLTP)

Six different type that amazon provides

### DynamoDB (No SQL)

### RedShift (used for OLAP warehousing solution, or business intelligence)

### ElasticCache (used for speeding up performance of existing database)

two different caching engines

Redis, Memcached

- **And also, RDS runs on virtual machines, and you cannot log in to these operating systems. (for security issues)**
- Patching of the RDS OS and DB is Amazon's responsibility, not ours.
- RDS is NOT server-less (exception of Aurora Serverless)

## More on Back Ups, Multi-AZs, Read Replicas

### Back ups on RDS

Two different type of back ups : 

1. Automated Backups : allow you to recover your database to any point in time within a "retention period" (between one and 35 days). It will take a full daily snapshot and will also store transaction logs throughout the day. Allows point-in-time recovery period down to second! Enabled by default. Backup data is stored in S3, and you get free storage space equal to the size of your database

2. DB Snapshots : they are done manually! Unlike automated backups, they are stored even after you delete the RDS instance. 

Whenever you restore either an Automatic Back or a manual snapshot, the restored version will be a brand new RDS instance with a new DNS endpoint.

### Encryption at rest

Encryption is done using the AWS Key Management Service. Only once your RDS instance is encrypted, the data stored at rest in the underlying storage, automated backups, read replicas, snapshots are all encrypted as well.

### Multi-AZ

- Synchronously update all the changes or updates to the primary database.
- Having an exact copy of your production database in another AZ.
- Automatic Synchronization/failover (AWS does it for you)

If the primary database is lost, Amazon will automatically update the DNS settings and the RDS endpoint to point to secondary DB in different AZ.

Use CASE:

- planned database maintenance
- AZ failure
- DB instance failure

BEAR IN MIND, it is for Disaster recovery only. Not improves performance.

Available for the RDS databases **except Aurora**

### Read Replica

Base case is that multiple EC2 instances are reading from the one an only primary database. But if you have read replica, then you can balance the load. 

READ-ONLY copy of your production database. **Asynchronous replication** from the primary RDS to the read replica. For read-heavy database workload.

Q.*How can you improve the performance of RDS (in general terms)?*
A. Read Replica, or using ElasticCache

Available for the RDS databases **except for SQL server**

BEAR IN MIND :

- used for scaling not for DR(disaster recovery)
- Must have automatic backups to deploy a read replica
- 5 read replica copies of any database
- chain of read-replica is OK (but watch out for replica latency)
- Each replica has its own DNS end point
- multi AZ configuration of read replicas are available, also the primary DB could be in multi AZ too.
- Replicas can be promoted to be their own databases, and breaking the replication
- replica can have different AZ from the primary one


## Final Summary

### two ways to back up RDS

2. Automated backup
1. DB snapshot

### Read replicas

- Can be Multi-AZ
- Used to improve performance.
- Should turn on backup
- Can be in different regions
- can be promoted to master, which in turn break the read replica

### Multi-AZ

- Used for DR.
- And also, you can force a failover from one AZ to another by rebooting the RDS instance!

### Encryption at rest

- By AWS Key Management Service
- Supported for all 6 RDS services
- When RDS is encrypted, backups, read replica, snapshots are all then encrypted. 


## DynamoDB (No-RDS) 

It is a fast and flexible NoSQL database service for all application that need consistent, single-digit millisecond latency at any scale! Supports both document and key-value data model.

- Stored on SSD storage
- Spread across 3 geographically distinct data centers (redundancy)
- Eventual Consistent Reads (Default)
- Strongly consistent Reads

### Eventual consistent reads?

- Consistency across all copies of data is usually reached within a second. Repeating a read after a short time should return the updated data. BEST READ PERFORMANCE

### Strongly consistent reads (differs from eventual ~ )

- returns a result that reflects all writes that received a successful response prior to the read.
- up-to-date data but at cost of high latency

![stronglyConsistentReads](https://i.imgur.com/1bQcSFq.png)

# RedShift (Data warehousing solution for OLAP)

For OLAP, it not desirable to pull in data from a lot of different areas. Move your production database to separate data warehouse!

- Single Node
- Multi-Node (Leader Node & Compute Node)

## Advanced compression

Columnar data stores can be compressed much more than row-based data stores because similar data is stored sequentially on disk. 

So it doesn't require indexes or materialized views, therefore uses less space than traditional relational database systems.

## Massively Parallel Processing (MPP)

Redshift automatically distributes data and query load across all nodes. It is easy to add nodes to your data warehouse and you are able to maintain fast query performance as your data warehouse grows.

## Backups

- Enabled by default with a 1 day retention period
- Max retention period is 35 days
- Redshift always attempts to maintain at least three copies of your data (the original + replica on the computer nodes + backup in Amazon S3)
- Also able to Asynchronously replicate your snapshots to S3 in another region for DR.


## Billing

- Billed for 1 unit per node per hour. Not be charged for leader node hours, only compute nodes.
- Backup
- Data transfer

## Security consideration

- Encrypted in transit using SSL
- Encrypted at rest using AES-256 encryption
- By default, redshift takes care of key management.
- however, you can manage your own keys through hardware service module as well as AWS KMS

## Redshift

- Currently only available in 1 AZ
- Can restore snapshots to new AZ in the event of an outage.


# Aurora

MySQL and PostgreSQL compatible relational database engine. Better performance, cheaper price.

- Storage Auto-scaling
- Compute resources can also be scaled automatically.
- 2 copies per AZ. Minimum of 3 AZs. Ergo, 6 copies of your data! (so Aurora is only available in region that has three AZs)

## Scaling Aurora

It is designed to transparently handle the loss of up to **two copies of data without affecting write availability** and up to **three copies of without affecting read availability** 

It is also self-healing. Data blocks and disks are continuously scanned for errors and repaired automatically

## Two types of Aurora Replicas

- Aurora replicas (automated failover available) (number of replicas up to 15)
- MySQL replicas (not available) (number of replicas up to 5)

## Backups with Aurora

- Automated backups are always enabled on Amazon Aurora DB instances. Backup does not harm the RDS performance
- Able to take snapshots with Aurora. This also doesn't impact on performance.
- share snapshots with other AWS accounts.

## IMPORTANT, how to migrate MySQL database to Aurora?

1. create Aurora read replica
2. then the cluster would consist of reader node and writer node (they have different DNS endpoint, AZs), select write node and *promote read replica*

Or, you can take snapshot and restore it as Aurora.


# ElasticCache

It is a **web service** that makes it easy to deploy, operate and scale an in-memory cache in the cloud. Performance of web applications by allowing you to retrieve information from in-memory caches.

## Memcached vs Redis

Simple caching? Memcached is better. Also it is the only engine that supports multi-threaded performance. E.Z.

If you need advanced data types, need ranking and sorting, multiple AZ, back up and restore capabilities.... You might need Redis


## Just remember its use case

- In order to increase database and web application performance.
- Database overloaded? 1. read replica 2. ElasticCache
