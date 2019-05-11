# AWS Amazon Machine Image (AMI)
An Amazon Machine Image (AMI) provides the information required to launch an instance.

- You must specify an AMI when you launch an instance.
- You can launch multiple instances from a single AMI when you need multiple instances with the same configuration. 
- You can use different AMIs to launch instances when you need instances with different configurations.
- AMIs are specific to the architecture of the processor e.g. 32-bit/64-bit AMI


AMI includes following information:
- A template for the root volume for the instance (for example, an operating system, an application server, and applications)
- Launch permissions that control which AWS accounts can use the AMI to launch instances
- A block device mapping that specifies the volumes to attach to the instance when it's launched

## AMI Lifecycle
---
- Create an AMI
- Register the AMI (_Note: When you create your AMI, it will automatically be registered to My AMI_)
- Launch instance from the AMI
- Copy an AMI (_Note: You can copy an AMI within the same region or to a different region)
- Deregister AMI when you no longer need it

## How to create your own AMI
---
First you launch an instance from an existing AMI, customize the instance, then save this updated configuration as a custom AMI. Creating an AMI process is determined by root storage device of the instance.

- EBS-backed Linux AMI
- Instance store-backed Linux AMI

