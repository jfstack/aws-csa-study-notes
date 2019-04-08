# Notes on DynamoDB
Amazon DynamoD is a
- Fully managed NoSQL database service
- Provides consistent, single-digit millisecond latency at any scale.
- Supports both document and key-value data model
- Its flexible data model and reliable performance make it a great fit for mobile, web, gaming, ad-tech, IoT etc.
- Stored on SSD Storage
- Spread Across **3** geographically distinct data centers
- Eventual Consistent Read (Default)
  - Consistency across all copies of data is usually reached within a second. Repeating a read after a short amount of time should return the updated data. (Best Read Performance)

- Strongly Consistent Reads
  - A stronly consistent read returns a result that reflects all writes that received a successful response prior to the read.

**NOTE:** Super easy to scale! Push button scaling

## DynamoDB Pricing

Pricing is based on provision throughput capacity

- Write Throughput $0.0065 per hour for every 10 units
- Read Throughput $0.0065 per hour for every 50 units
- Storage costs of $0.25G per month

_Pricing Example:_

```
Constraint: 1 million WRITEs and 1 million READs per day, while storing 3G of data.

First, calculate how many writes and reads per second you need.

1 million evenly spread writes per day is equivalent to 1,000,000 (writes) /24 (hours) / 60 (minutes) / 60 (seconds) = 11.6 writes per second.

-- BREAKDOWN --

DynamoDB WRITE Capacity Unit - 1 per second = 12
DynamoDB READ Capacity Unit - 1 per second = 12

READ Capacity Units - billed in blocks of 50
WRITE Capacity Units - billed in blocks of 10

Calc WRITE Capacity Units = (0.0065 / 10) x 12 x 24 = $0.1872
Calc READ Capacity Units = (0.0065 / 10) x 12 x 24 = $0.0374
```


# My curated list of FAQs on AWS DynamoDB

## What is partition key?
The primary key is made up of a partition key (hash key) and an optional sort key. The partition key is used to partition data across hosts for scalability and availability. Choose an attribute which has a wide range of values and is likely to have evenly distributed access patterns. For example CustomerId is good while GameId is bad if most of your traffic relates to a few popular games.

## What is sort key?
The sort key allows for searching within a partition. For example, an Orders table with primary attribute CustomerId and sort attribute OrderTimestamp would allow for queries for all orders by a specific customer in a given date range.

## DynamoDB default setting
+ No secondary indexes.
+ Provisioned capacity set to 5 reads and 5 writes.
+ Basic alarms with 80% upper threshold using SNS topic "dynamodb".
+ Encryption at Rest with DEFAULT encryption type

## Sample record
```json
[
  {
    "status": "PROCESSING",
    "text": "Hello Cloud Gurus!",
    "voice": "Joanna",
    "id": "d97e244c-0b49-4374-aa7a-fbf30c36dd17"
  },
  {
    "status": "PROCESSING",
    "text": "Hello Cloud Gurus!",
    "voice": "Joanna",
    "id": "267dad5f-2488-4457-bf14-8eff6d4b7ac6"
  }
]
```
## DynamoDB vs MongoDB
+ [7 Reasons You Should Use MongoDB over DynamoDB](http://www.masonzhang.com/2013/08/7-reasons-you-should-use-mongodb-over.html)
+ [3 Reasons You Should Use DynamoDB over MongoDB](http://www.masonzhang.com/2013/08/3-reasons-you-should-use-dynamodb-over.html)
+ [Migrate from MongoDB to AWS DynamoDB + SimpleDB](http://www.masonzhang.com/2013/07/lean7-migrate-from-mongodb-to-dynamodb.html)
+ [2 reasons why we select SimpleDB instead of DynamoDB](http://www.masonzhang.com/2013/06/2-reasons-why-we-select-simpledb.html)

