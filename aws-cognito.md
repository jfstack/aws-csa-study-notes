# [Amazon Cognito](https://aws.amazon.com/cognito/)
### Simple and Secure User Sign-Up, Sign-In, and Access Control

Amazon Cognito lets you add user sign-up, sign-in, and access control to your web and mobile apps quickly and easily. Amazon Cognito scales to millions of users and supports sign-in with social identity providers, such as Facebook, Google, and Amazon, and enterprise identity providers via SAML 2.0.

With the Amazon Cognito SDK, you just write a few lines of code to enable your users to sign-up and sign-in to your mobile and web apps.

With Amazon Cognito, your users can sign-in through social identity providers such as Google, Facebook, and Amazon, and through enterprise identity providers such as [Microsoft Active Directory](https://aws.amazon.com/directoryservice/active-directory/) using [SAML](https://aws.amazon.com/identity/saml/).

In addition, Amazon Cognito enables you to synchronize data across a user’s devices so that their app experience remains consistent when they switch between devices or upgrade to a new device.

### Features
- Sign-up and Sign-in to your mobile or web-based app.
- Access to guest users
- Acts as a Identity Broker between you app and Web Identity Providers so you dont need to build and maintain your own.
- Data Sync service across all user's devices (authenticated users only).


## What is Web Identity Fedaration?
Web Identity Fedaration lets you give your users access to AWS resources after they have successfully authenticated with a web-based identity provider (IdP) such as Amazon, Facebook or Google.


## How it works?
1. Authenticate with identity provider: After the end user is authenticated with the provider, an OAuth or OpenID Connect token returned from the provider is passed by your
application to Amazon Cognito.
2. Obtain temprary security credentials: Cognito returns a new Amazon Cognito ID for the user and a set of temporary, limited-privilege AWS credentials. (AssumeRoleWithWebIdentity)
3. Access AWS resources: Using temporary credentials (Secret Access Key, Access Key ID and Session Token), app user will be authorized to access AWS resources like, S3, DynamoDB or Lambda.


By default, Amazon Cognito creates a new role with limited permissions; end users only have access to the Amazon Cognito Sync service and Amazon Mobile Analytics. If your application needs access to other AWS resources, such as
Amazon S3 or Amazon DynamoDB, you can modify your roles directly from the IAM console.

With Amazon Cognito, there is no need to create individual AWS accounts or even IAM accounts for every one of your web/mobile application end users who will need to access your AWS resources.

### Integration
- Amazon Cognito Client side SDK (AWS Mobile SDK)
- Server-side APIs

### User Pools
Users pools are user directories used to manage sign-up and sign-in functionality for mobile and web applications.
Users can sign-in directly to user pool or using Facebook, Google or Amazon IdPs.

Amazon Cognito User Pool makes it easy for developers to add sign-up and sign-in functionality to web and mobile applications. It serves as your own identity provider to maintain a user directory. It supports user registration and sign-in, as well as provisioning identity tokens for signed-in users.

### Identity Pools or Cognito Federated Identities
Amazon Cognito Federated Identities enables developers to create unique identities for your users and authenticate them with federated identity providers. With a federated identity, you can obtain temporary, limited-privilege AWS credentials to securely access other AWS services such as Amazon DynamoDB, Amazon S3, and Amazon API Gateway.

## Relation and differences between User and Identity pools
---
Read this link: [Cognito User Pool vs Identity Pool](https://serverless-stack.com/chapters/cognito-user-pool-vs-identity-pool.html)

