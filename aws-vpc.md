# AWS Virtual Private Cloud (VPC)
### __Amazon's definition of VPC:__

*Amazon Virtual Private Cloud (VPC) enables you to launch AWS resources into a virtual network that you define. This virtual network closely resembles traditional network that you'd operate in your own data center, with the benefit of using scalable infrastructure of AWS.*


- Logically isolated virtual IP networking for AWS resources like EC2, RDS instances
- AWS root account can have many VPCs (soft limit is 5 per region)
- AWS root account created after Dec. 4 2013 supports VPCs and comes with a default VPC per region
- Each VPC is divided into *subnets* each bound to a AZ
- However, AZ can host multiple subnets
- Each instance launched connects to a subnet with a Elastic Network Interface (ENI)

### VPC capability
- Isolate AWS resources from other account
- Routing network traffic to and from your instances
- Protecting your instances from network intrusion



## Benefits of VPC
- Ability to launch instance in a subnet
- Ability define custom IP address ranges inside each subnets
- Ability configure route table between subnets
- Ability to configure Internet gateway and attach it to subnets 
- Ability to create layered network of resources
- More fine grain security settings to protect cloud assets
- Extend you existing data center to cloud with IPsec VPN connection
- Layered security
  - Security group at instance level
  - Network ACL at subnet level

## Default VPC
- Designed to make it easy for AWS users to setup networking for resources without having to create it from scratch
- Associated with an IPv4 CIDR block 172.31.0.0/16 address range which gives 65,536 IP addresses minus some AWS reserved addressed
- Default subnets are created with /20 CIDR block address range which gives 4091 minus 5 AWS reserved IP address 
- All subnets inside default VPC are associated with default Internet gateway (IGW)
- Each instance launched in default VPC receives a private IP address from the pool of address of the subnet and private DNS hostname
- Each instance launched in default VPC also receives a public IP from the pool of addresses owned by AWS and public DNS hostname which facilitate internet access to the instance
- You can delete default VPC, but try not to do so. You'd need to contact AWS support to restore it if it's deleted.

![](images/default_vpc_diagram.png)

- Default VPC is okay for testing AWS resources, training and low-critical single tier application.
- It's recomended not to use default VPC for production workload.
  - default VPC lacks proper security and auditing controls
  - Doesn't enable VPC-Flowlog - controls allows admin to track network flow in the VPC for auditing and troubleshooting purposes. There are third-party tools in AWS ecosystem for analyzing these logs
  - Unrestricted NACLs
  - No tagging - using tags and closely related resource groups simplifies the management of resources in VPC and helps analyzing costs.
  

> CIDR (Classless Inter Domain Routing) IP address range defines a range of IP addresses which AWS will use to assign *__private__* IP address to EC2 instances.

### Private Address Space (RFC 1918)
The Internet Assigned Numbers Authority (IANA) has reserved following 3 blocks of IP address space for private internets.

- 10.0.0.0        -   10.255.255.255  (10/8 prefix)
- 172.16.0.0      -   172.31.255.255  (172.16/12 prefix)
- 192.168.0.0     -   192.168.255.255 (192.168/16 prefix)

AWS recomends that VPC to use this private address space, however, AWS VPC supports /28 CIDR blocks to /16 CIDR block of IP address.

## VPC Peering
- Allows to connect one VPC to another using direct network route with private IP address
- Instances behave as if they are on same private network
- Peering is possible with VPCs on other AWS account as well as VPCs on the same account.
- Peering support STAR configuration. NO TRANSITIVE PEERING POSSIBLE

![](images/vpc_peering_start_config.jpg)

## VPC Components
There are couple of core components which are fundamental to VPC as follows:
- VPC CIDR Block
- Subnet
- Route table
- Internet Gateway
- NAT Gateway/NAT Instance
- Virtual Private Gateway
- Network Access Control List (NACL)
- Security Group
- Peering Connection
- VPC Endpoint
- Egress-Only Internet Gateway

__VPC CIDR Block:__ CIDR (Classless Inter Domain Routing) IP address range defines a range of IP addresses which AWS will use to assign *__private__* IP address to EC2 instances.

__Subnet:__ A segnet of a VPC's IP address range where you can place group of isolated resources.

__Route Table:__ A table of defined routes which routes the traffic to and from subnets.

__Internet Gateway:__ The Amazon VPC side of a connection to the public Internet.

__NAT Gateway/NAT Instance:__ A NAT Gateway is highly available, managed Network Address Translation service for your resources in private subnets to access the Internet. However, NAT instance is not managed and needs to be provisioned and high availabilty to be guranteed by user. AWS recomends to use NAT gateway as NAT instance is phasing out soon.

__Virtual Private Gateway:__ The Amazon VPC side of a VPN connection.

__NACL:__ Network Access Control List acts as a firewall to subnet level. It is stateless meaning inbout as well as outboud access rule has to be defined explicitly.

__Security Group:__ Acts as a firewall to instance level. It's stateful meaning inbound rule will be membered for traffic out.

__Peering Connection:__ A peering conection enables you to route traffic via private IP address between two peered VPCs

__VPC Endpoint:__ Enables private connectivity to services hosted in AWS, from within your VPC without using an Internet Gateway, VPN, NAT devices or firewall

__Egress-only Internet Gateway:__ A sateful gateway to provide egress only access for IPv6 traffic from the VPC to the Internet.

## Custom VPC
When we create a new custom VPC, following components are created by default:
- A route table (called main route table)
- A default NACL
- A default Security group (Security group can't span across VPC but can span across AZ)

No subnets or IGW is created that we need to create by yourself.

When we create subnets in our custom VPC, 
- Auto assign IPv4/IPv6 IP address is diabled
- First 4 and last IP of the range is reserved by AWS

# Network Address Translation (NAT)

## NAT Instance
- You need to choose a NAT AMI from community tab to create NAT instance
- When create a NAT instance, disable Source/Destination check on the EC2 instance (Enabled by default)
- Must be in public subnet
- There must be a route out of the private subnet (route table attached to private subnet) to the NAT, in order for this to work.
- The amount of traffic a NAT instance can support depends on the instance type. If there is bottleneck, try increase the instance size/type.
- You can create high availability using Auto Scaling Groups, multiple subnets in different AZs and a script to automate failover.
- Always sits behind a security group

## NAT Gateway
- Redundant inside the AZ. It can't span across AZs
- Preferred by enterprise and recomended by AWS as well
- Starts with 5 gbps and scales automatically upto 45 gbps
- No need to patch
- Not associated with security group
- Associated with an Elastic IP address while creating it
- Update the route table of the private subnet with a route out to NAT gateway
- Must be in public subnet
- No need to disable Source/Destination Check
- More Secure than NAT Instance
- If you have resources in multiple AZs, and they share one NAT gateway, in the event that NAT gateway's AZ is down, resources in other AZ loose Internet access. To create an AZ-independent architecture, create a NAT gateway in each AZs and configure your routing to ensure that resources use the NAT gateway in the same AZ.



