# IAM policies vs. S3 bucket policies
### IAM Policy
> what actions are allowed or deny on what aws resources

> e.g. allow ec2:TerminateInstance on the EC2 instance with instance_id=i-8b3620ec

> you attach IAM policies to IAM users, groups or roles

> in other words, IAM policy define what a principal can do in your AWS environment

### S3 bucket Policy
> attached only to S3 bucket

> defines what actions are allowed or denied for which principals on the bucket that the bucket policy is attached to

> e.g. allow user Alice to PUT but not DELETE objects in the bucket

> You attach S3 bucket policies at the bucket level (i.e. you can’t attach a bucket policy to an S3 object), but the permissions specified in the bucket policy apply to all the objects in the bucket

### Similarities
- IAM policies and S3 bucket policies are both used for access control
- they’re both written in JSON using the AWS access policy language

### Sample S3 bucket policy
_This S3 bucket policy enables the root account 111122223333 and the IAM user Alice under that account to perform any S3 operation on the bucket named “my_bucket”, as well as that bucket’s contents_

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": ["arn:aws:iam::111122223333:user/Alice",
                "arn:aws:iam::111122223333:root"]
      },
      "Action": "s3:*",
      "Resource": ["arn:aws:s3:::my_bucket",
                   "arn:aws:s3:::my_bucket/*"]
    }
  ]
}
```

### Sample IAM policy
_This IAM policy grants the IAM entity (user, group, or role) it is attached to permission to perform any S3 operation on the bucket named “my_bucket”, as well as that bucket’s contents_

```json
{
  "Version": "2012-10-17",
  "Statement":[{
    "Effect": "Allow",
    "Action": "s3:*",
    "Resource": ["arn:aws:s3:::my_bucket",
                 "arn:aws:s3:::my_bucket/*"]
    }
  ]
}
```

### Notes
- Note that the S3 bucket policy includes a “Principal” element, which lists the principals that bucket policy controls access for.
- The “Principal” element is unnecessary in an IAM policy, because the principal is by default the entity that the IAM policy is attached to.
- S3 bucket policies only control access to S3 resources, whereas IAM policies can specify nearly any AWS action.
- In AWS, you can actually apply both IAM policies and S3 bucket policies simultaneously, with the ultimate authorization being the least-privilege union of all the permissions.

# Sample bucket policy to make a S3 bucket (example-bucket) public
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": [
                "s3:GetObject"
            ],
            "Resource": [
                "arn:aws:s3:::example-bucket/*"
            ]
        }
    ]
}
```

# When to use IAM policies vs. S3 policies
__Use IAM policies if:__
- You need to control access to AWS services other than S3. IAM policies will be easier to manage since you can centrally manage all of your permissions in IAM, instead of spreading them between IAM and S3.
- You have numerous S3 buckets each with different permissions requirements. IAM policies will be easier to manage since you don’t have to define a large number of S3 bucket policies and can instead rely on fewer, more detailed IAM policies.
- You prefer to keep access control policies in the IAM environment.

__Use S3 bucket policies if:__
- You want a simple way to grant cross-account access to your S3 environment, without using IAM roles.
- Your IAM policies bump up against the size limit (up to 2 kb for users, 5 kb for groups, and 10 kb for roles). S3 supports bucket policies of up 20 kb.
- You prefer to keep access control policies in the S3 environment.

__If you’re still unsure of which to use, consider which audit question is most important to you:__

> If you’re more interested in __“What can this user do in AWS?”__ then IAM policies are probably the way to go. You can easily answer this by looking up an IAM user and then examining their IAM policies to see what rights they have.

> If you’re more interested in __“Who can access this S3 bucket?”__ then S3 bucket policies will likely suit you better. You can easily answer this by looking up a bucket and examining the bucket policy.

# What about S3 ACLs?
> As a general rule, AWS recommends using S3 bucket policies or IAM policies for access control.

> S3 ACLs is a legacy access control mechanism that predates IAM.

>> However, if you already use S3 ACLs and you find them sufficient, there is no need to change.

> When you create a bucket or an object, Amazon S3 creates a default ACL that grants the resource owner full control over the resource.

> You can attach S3 ACLs to individual objects within a bucket to manage permissions for those objects.

# S3 bucket policy vs S3 ACL
- Access Control Lists (ACLs) are legacy (but not deprecated)
- bucket/IAM policies are recommended by AWS
- ACLs give control over buckets AND objects but policies are only at the bucket level


# How does authorization work with multiple access control mechanisms?

- Whenever an AWS principal issues a request to S3, the authorization decision depends on the union of all the IAM policies, S3 bucket policies, and S3 ACLs that apply.

- In accordance with the principle of least-privilege, decisions default to DENY and an explicit DENY always trumps an ALLOW.

- For example, if an IAM policy grants access to an object, the S3 bucket policies denies access to that object, and there is no S3 ACL, then access will be denied. Similarly, if no method specifies an ALLOW, then the request will be denied by default. Only if no method specifies a DENY and one or more methods specify an ALLOW will the request be allowed.

![Authorization process](AuthZDiagram.png)

## Resource
---
- <https://aws.amazon.com/blogs/security/iam-policies-and-bucket-policies-and-acls-oh-my-controlling-access-to-s3-resources/>
- <https://stackoverflow.com/questions/47815526/s3-bucket-policy-vs-access-control-list>