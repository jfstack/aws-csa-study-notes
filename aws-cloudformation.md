# AWS CloudFormation
AWS CloudFormation provides a common language to describe and provision all the infrastructure resources in the cloud. It's a service that gives developers and systems administrators an easy way to create and manage a collection of related AWS resources, provisioning and updating them in an orderly and predictable fashion.

### Authoring Language
- CloudFormation template is just plan test file.
- JSON formatted text file
- YAML formatted file
- There is also designer tool for visual design

### Features
- Safty contol: There are no manual steps or control that can lead to errors. Rollback triggers can be used to roll back the entire stack in case of any error during provisioning.
- Preview changes to the environment
- Dependecy management

### Pricing
There is no additional charge for AWS CloudFormation. You pay for AWS resources (such as Amazon EC2 instances, Elastic Load Balancing load balancers, etc.) created using AWS CloudFormation in the same manner as if you created them manually.

### Why to use CloudFormation
- Simplify infrastructure management
- Quickly replicate the infrastructure
- Easily control and track changes to the infrastructure. (using version controlling)
- saving time and money


## Templates can include several major sections:
- AWSTemplateFormatVersion
- Description
- Metadata
- Parameters
- Mappings
- Conditions
- Outputs
- Resources (__*mandatory*__)

Except "Resources" element, all other elements are optional.

### Simple template example

```json
{
  "Resources": {
    "Name-of-the-bucket": {
      "Type": Resource type
    }
  }
}

Name-of-the-bucket = "cghosh-bucket"
Resource Type = AWS::S3::Bucket

{
  "Resources": {
    "cghosh-bucket": {
      "Type": AWS::S3::Bucket
    }
  }
}
```

```yaml
Resources:
  AppNode:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: t2.micro
      ImageId: ami-a58d0dc5
      KeyName: aws-key1
      SecurityGroup:
        - !Ref AppNodeSG
  AppNodeSG:
    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupDescription: 
      SecurityGroupIngress:
      - IpProtocol: tcp
        FromPort: '80'
        ToPort: '80'
        CidrIp: 0.0.0.0/0
      - IpProtocol: tcp
        FromPort: '22'
        ToPort: '22'
        CidrIp: 0.0.0.0/0        
```