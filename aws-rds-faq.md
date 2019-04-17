# Amazon Relation Database Service (RDS)
### _Managed relational database service_

### _Amazon RDS provides cost-efficient and scalable relational database capacity while automating time-consuming administration tasks such as hardware provisioning, database setup, patching and backups. It frees you to focus on your applications so you can give them the fast performance, high availability, security and compatibility they need._

## Benefits and Features:
- __Easy to Administer__ - 
Amazon RDS makes it easy to go from project conception to deployment. Access the capabilities of a production-ready relational database in minutes. No need for infrastructure provisioning, and no need for installing and maintaining database software.

- __Highly Scalable__ -
You can scale your database's compute and storage resources with only a few mouse clicks or an API call, often with no downtime. Many Amazon RDS engine types allow you to launch one or more Read Replicas to offload read traffic from your primary database instance.

- __Available and Durable__ -
Amazon RDS lets you deploy a database across multiple Availability Zones and automatically fails over to a standby instance in case of an outage. Amazon RDS has many other features that enhance reliability for critical production databases, including automated backups, database snapshots, and host replacements.

- __Fast__ -
Amazon RDS supports the most demanding database applications. You can choose between two SSD-backed storage options: one optimized for high-performance OLTP applications, and the other for cost-effective general-purpose use.

- __Secure__ -
Amazon RDS makes it easy to control network access to your database. Your databases run in Amazon Virtual Private Cloud (Amazon VPC), which enables you to isolate your databases and securely connect to your existing IT infrastructure. Amazon RDS also offers encryption of data at rest and encryption in transit.

- __Inexpensive__ -
You pay very low rates and only for the resources you actually consume. In addition, you benefit from the option of On-Demand pricing with no up-front or long-term commitments, or even lower hourly rates via our Reserved Instance pricing.

## Support Web links 
--------------------
- [AWS Cloud database](https://aws.amazon.com/products/databases/)
- [AWS RDS FAQ](https://aws.amazon.com/rds/faqs/)


## Types of Databases
### Relational Database
- Database
- Tables
- Columns
- Rows

**_Relational Databases Examples_**
- SQL Server
- Oracle
- MySQL
- PostgreSQL
- Aurora
- MariaDB

### NoSQL Database (Non relational)
- Database
  - Collection => Table
  - Document => Row
  - Key, Value Pairs => Columns

**_Non Relational Databases Examples_**

- Amazon DynamoDB

```json
{
  "_id": "564ejsjkskjdfh9030918dsfhh",
  "name": "Chandan",
  "age": 31,
  "addresses": [
      {
        "country": "USA",
        "state": "Maryland",
        "zipcode": 24023
      }
    ]
}
```
## Data Warehousing

Used for business intelligence and analytics. Tools like Cognos, Jaspersoft, SQL Server, Reporting Services, Oracle Hyperion, SAP NetWeaver etc.

Used to pull in very large and complex data sets. Usually used by management to do queries on data (such as current performance vs targets etc).

> Data Warehousing databases use different type of architecture both from a database perspective and infrastructure layer.

**_Data warehousing Examples_**
- Amazon RedShift

## Type of processing (In general)
- OLTP (Online transaction processing)
- OLAP (Online analytics processing)

> OTLP differs from OLAP in terms of the types of queries 
you will run.



### Elasticache

- ElastiCache is a web service that makes it easy to deploy, operate and scale an in-memory cache in the cloud. 

- The service improves the performance of web applications by allowing you to retrieve information from fast, managed,in-memory caches, instead of relying entirely on slower disk-based databases.

ElasticCache supports two open-source in-memory caching engines.

1. Memcached
2. Redis

## Backups
- Automated
- Snapshot (manual)

### Automated Backups
----------------------

> Automated Backups allow you to recover your database to any point in time within a 'retention period'. The retention period can be between one and 35 days.

> Automated Backups will take a full daily snapshot and will also store transaction logs throughout the day.

> When you do a recovery, AWS will first choose the most recent daily backup, and then apply transaction logs relevant to that day. This allows you to do a point in time recovery down to a second, within a retention period.

### Database Snapshots
-----------------------
> DB Snapshots are done manually (ie they are user initiated) They are stored even after you delete the original RDS instance, unlike automated backups.

### Restoring Backups
----------------------
> Whenever you restore either an Automatic Backup or a manual Snapshot, the restored version of the database will be a new RDS instance with a new DNS endpoint

Original DNS: `original.us-west-1.rds.amazonaws.com`  

Restored DNS: `restored.eu-west-1.rds.amazonaws.com`

## Encyrption

> Encryption at rest is supported for MySQL, Oracle, SQL Server, PostgreSQL, MariaDB & Aurora.

> Encryption is done using the AWS Key Management System (KMS) service. Once your RDS instance is encrypted, the data stored at rest in the underlying storage is encrypted, as are its automated backups, read replicas and snapshots.

> At the present time, encrypting an existing DB Instance is not supported. To use RDS encryption for an existing database, you must first create a snapshot, make a copy of that snapshot and encrypt the copy.

## Multi-AZ Deployment

> Multi-AZ allows you to have an exact copy of your production database in another Availability Zone. AWS handles the replication for you, so when your production database is written to, this write will automatically be synchronized to the stand by database.

> In the event of planned database maintenance, DB instance failure, or AZ failure, RDS will automatically failover to the standby so that database operations can resume quickly without admin intervention.

> It's for **disaster recovery** only.

> For performance improvement, you need **Read Replicas**

**Multi-AZ Deployment Available DBs**

- SQL Server
- Oracle
- MySQL Server
- PostgreSQL
- MariaDB

> Amazon Aurora is default Multi-AZ

## Read Replicas

Read replicas allow you to have a read-only copy of your production database. This is achieved by using asynchronous replication from the primay RDS instance to the Read Replica. You use Read Replicas primarily for very read-heavy database workloads.

- Used for scaling out, not disaster control!
- Must have auto backups turned on in order to deploy a Read Replica
- You can have up to 5 Read Replica copies of any database.
- You can have Read Replicas of Read Replicas _(involves higher latency)_
- Each Read Replica will have its own DNS end point.
- You can have Read Replicas that have Multi-AZ
- You can create Read Replicas of Mulit-AZ source databases
- Read Replicas can be promoted to be their own databases. This breaks the replication.
- You can have a Read Replica in a second region.

**Read Replica Available DBs**

- MySQL Server
- PostgreSQL
- MariaDB
- Aurora
- Oracle

> For the MySQL, MariaDB, PostgreSQL, and Oracle database engines, Amazon RDS creates a second DB instance using a snapshot of the source DB instance. It then uses the engines' native asynchronous replication to update the read replica whenever there is a change to the source DB instance. The read replica operates as a DB instance that allows only read-only connections; applications can connect to a read replica just as they would to any DB instance. Amazon RDS replicates all databases in the source DB instance.

> Amazon Aurora employs an SSD-backed virtualized storage layer purpose-built for database workloads. Amazon Aurora replicas share the same underlying storage as the source instance, lowering costs and avoiding the need to copy data to the replica nodes.

[Amazon RDS Read Replica](https://aws.amazon.com/rds/details/read-replicas/)

## Amazon Aurora

### What is Aurora?

Aurora is a MySQL-and PostgreSQL-compatible,relational database engine that combines the speed and availability of high-end commercial databases with the simplicity and cost effectiveness of open source databases. Aurora provides up to 5x better performance than MySQL at a price point of 1/10 that of a commercial database while delivering similar performance and availability

### Scaling

- Start with 10G, Scales in 10G increments to 64 TB (Storage Autoscaling)
- Compute resource can scale up to 32vCPUs and 244G of Memory.
- 2 copies of your data is contained in each availability zone, with minimum of 3 AZ -> 6 copies of your data! Highly redundant
- Designed to transparently handle the loss of up to 2 copies of data without affecting database write availability and up to 3 copies without affecting read availability.
- Aurora storage is also self-healing. Data blocks and disks are continuously scanned for errors and repaird automatically.
- You can share Aurora snapshots with other AWS account
- Aurora has automated backup turned on by default. You can also create snapshot of Aurora DB



### Aurora Replicas

- 2 Types of Replicas are available
- Aurora Replicas - Up to 15 replicas currently
- MySQL Replicas - Up to 5 replicas currently
- Automated fail-over is only available on Aurora replicas

## Migration of an existing MySQL database to Aurora database

1. Create an Aurora replica from the existing MySQL db instance and promoting it _(the write node in the cluster)_
2. Or by creating a snapshot of the existing MySQL DB instance and restore it as Aurora DB

# Amazon Athena
- Interactive query service
- Makes easy to analyze data in Amazon S3 using standard ANSI SQL to quickly analyze large-scale dataset.
- There is no need for a complex ETL(Extract-Transform-Load) jobs to prepare data analysis
- Serverless, no infrastructure to manage
- Pay only for the queries to run


# Exam Tips
You will be given a scenario where a particular database is under a lot of stress/load. You may be asked which service you should use to alleviate this.

ElastiCache is a good choice if your database is particularly read heavy and not prone to frequent changing.

Redshift is a good answer if the reason your database is feeling stress is because management keep running OLAP transactions on it etc.

