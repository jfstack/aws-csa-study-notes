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

