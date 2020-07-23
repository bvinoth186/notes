# AWS SAA-CO2 Notes - 2020

# Shorts 
  - IOPS - MongoDB, Cassandra, Microsoft SQL Server, MySQL, PostgreSQL, Oracle
  - Throughput -  Big data, Data warehouses, Log processing, Apache Kafka


# AWS Fundamentals: IAM & EC2

## AWS Regions 

  - AWS has Regions all around the world 
  - Names can be: us-east-1, eu-west-3… 
  - A region is a cluster of data centers 
  - Most AWS services are region-scoped

## AWS Availability Zones

  - Each region has many availability zones (usually 3, min is 2, max is 6). 
  - Example:
  - ap-southeast-2a
  - ap-southeast-2b
  - ap-southeast-2c
  - Each availability zone (AZ) is one or more discrete data centers with redundant power, networking, and connectivity
  - They’re separate from each other, so that they’re isolated from disasters
  - They’re connected with high bandwidth, ultra-low latency networking

## IAM  

  - IAM (Identity and Access Management) 
  - Your whole AWS security is there: 
  - Users 
  - Groups 
  - Roles 
  - Root account should never be used (and shared) 
  - Users must be created with proper permissions 
  - IAM is at the center of AWS 
  - Policies are written in JSON (JavaScript Object Notation)
  - IAM has a global view
  - Permissions are governed by Policies (JSON)
  - MFA (Multi Factor Authentication) can be setup
  - IAM has predefined “managed policies”
  - It’s best to give users the minimal amount of permissions they need to perform their job (least privilege principles) One IAM User per PHYSICAL PERSON
  - One IAM Role per Application
  - IAM credentials should NEVER BE SHARED
  - Never, ever, ever, ever, write IAM credentials in code. EVER.
  - And even less, NEVER EVER EVER COMMIT YOUR IAM credentials
  - Never use the ROOT account except for initial setup.
  - Never use ROOT IAM Credentials

## IAM Federation

  - Big enterprises usually integrate their own repository of users with IAM
  - This way, one can login into AWS using their company credentials
  - Identity Federation uses the SAML standard (Active Directory)
  
## EC2

  - EC2 is one of most popular of AWS offering
  - It mainly consists in the capability of :
  - Renting virtual machines (EC2)
  - Storing data on virtual drives (EBS)
  - Distributing load across machines (ELB)
  - Scaling the services using an auto-scaling group (ASG)

## EC2 Instance Connect

  - Connect to your EC2 instance within your browser
  - No need to use your key file that was downloaded
  - The “magic” is that a temporary key is uploaded onto EC2 by AWS
  - Works only out-of-the-box with Amazon Linux 2
  - Need to make sure the port 22 is still opened!

## Security Groups

  - Security Groups are the fundamental of network security in AWS
  - They control how traffic is allowed into or out of our EC2 Machines.
  - It is the most fundamental skill to learn to troubleshoot networking issues
  - Security groups are acting as a “firewall” on EC2 instances   - They regulate:   - Access to Ports   - Authorised IP ranges – IPv4 and IPv6   - Control of inbound network (from other to the instance)   - Control of outbound network (from the instance to other)
  - Can be attached to multiple instances
  - Locked down to a region / VPC combination
  - Does live “outside” the EC2 – if traffic is blocked the EC2 instance won’t see it
  - It’s good to maintain one separate security group for SSH access
  - If your application is not accessible (time out), then it’s a security group issue
  - If your application gives a “connection refused“ error, then it’s an application error or it’s not launched
  - All inbound traffic is blocked by default
  - All outbound traffic is authorised by default

## Private vs Public IP (IPv4)

  - Networking has two sorts of IPs. IPv4 and IPv6:
  - IPv4: 1.160.10.240
  - IPv6: 3ffe:1900:4545:3:200:f8ff:fe21:67cf
  - In this course, we will only be using IPv4.
  - IPv4 is still the most common format used online.
  - IPv6 is newer and solves problems for the Internet of Things (IoT).
  - IPv4 allows for 3.7 billion different addresses in the public space
  - IPv4: [0-255].[0-255].[0-255].[0-255]

## Public IP:

  - Public IP means the machine can be identified on the internet (WWW)
  - Must be unique across the whole web (not two machines can have the same public IP).
  - Can be geo-located easily

## Private IP:

  - Private IP means the machine can only be identified on a private network only
  - The IP must be unique across the private network
  - BUT two different private networks (two companies) can have the same IPs.
  - Machines connect to WWW using a NAT + internet gateway (a proxy)
  - Only a specified range of IPs can be used as private IP
  - By default, your EC2 machine comes with:
  - A private IP for the internal AWS Network
  - A public IP, for the WWW.
  - When we are doing SSH into our EC2 machines:
  - We can’t use a private IP, because we are not in the same network
  - We can only use the public IP.
  - If your machine is stopped and then started, the public IP can change

## Elastic IPs

  - When you stop and then start an EC2 instance, it can change its public IP.
  - If you need to have a fixed public IP for your instance, you need an Elastic IP
  - An Elastic IP is a public IPv4 IP you own as long as you don’t delete it
  - You can attach it to one instance at a time 
  - With an Elastic IP address, you can mask the failure of an instance or software by rapidly remapping the address to another instance in your account.
  - You can only have 5 Elastic IP in your account (you can ask AWS to increase that).
  - Overall, try to avoid using Elastic IP:
  - They often reflect poor architectural decisions
  - Instead, use a random public IP and register a DNS name to it
  - Or, use a Load Balancer and don’t use a public IP

## EC2 User Data

  - It is possible to bootstrap our instances using an EC2 User data script.
  - bootstrapping means launching commands when a machine starts
  - That script is only run once at the instance first start
  - EC2 user data is used to automate boot tasks such as:
  - Installing updates
  - Installing software
  - Downloading common files from the internet
  - Anything you can think of
  - The EC2 User Data Script runs with the root user
  
## EC2 Instance Launch Types

  - On Demand Instances: short workload, predictable pricing
  - Reserved: (MINIMUM 1 year)
  - Reserved Instances: long workloads
  - Convertible Reserved Instances: long workloads with flexible instances
  - Scheduled Reserved Instances: example – every Thursday between 3 and 6 pm
  - Spot Instances: short workloads, for cheap, can lose instances (less reliable)
  - Dedicated Instances: no other customers will share your hardware
  - Dedicated Hosts: book an entire physical server, control instance placement

## EC2 On Demand

  - Pay for what you use (billing per second, after the first minute)
  - Has the highest cost but no upfront payment
  - No long term commitment
  - Recommended for short-term and un-interrupted workloads, where you can't predict how the application will behave

## EC2 Reserved Instances

  - Up to 75% discount compared to On-demand
  - Pay upfront for what you use with long term commitment
  - Reservation period can be 1 or 3 years
  - Reserve a specific instance type
  - Recommended for steady state usage applications (think database)
  - Convertible Reserved Instance
    - can change the EC2 instance type
    - Up to 54% discount
  - Scheduled Reserved Instances
    - launch within time window you reserve
    - When you require a fraction of day / week / month
  
## EC2 Spot Instances

  - Can get a discount of up to 90% compared to On-demand
  - Instances that you can “lose” at any point of time if your max price is less than the current spot price
  - The MOST cost-efficient instances in AWS
  - Useful for workloads that are resilient to failure
    - Batch jobs
    - Data analysis
    - Image processing
  - Not great for critical jobs or databases
  - Great combo: Reserved Instances for baseline + On-Demand & Spot for peaks 

## EC2 Dedicated Hosts

  - Physical dedicated EC2 server for your use
  - Full control of EC2 Instance placement
  - Visibility into the underlying sockets / physical cores of the hardware
  - Allocated for your account for a 3 year period reservation 
  - More expensive
  - Useful for software that have complicated licensing model (BYOL – Bring Your Own License)
  - Or for companies that have strong regulatory or compliance needs 
  
## EC2 Dedicated Instances

  - Instances running on hardware that’s dedicated to you
  - May share hardware with other instances in same account
  - No control over instance placement (can move hardware after Stop / Start)  

## Which host is right for me?

  - On demand: coming and staying in resort whenever we like, we pay the full price
  - Reserved: like planning ahead and if we plan to stay for a long time, we may get a good discount.
  - Spot instances: 
    - the hotel allows people to bid for the empty rooms and the highest bidder keeps the rooms. 
    - You can get kicked out at any time
  - Dedicated Hosts: We book an entire building of the resort

## EC2 Spot Instance Requests

  - Can get a discount of up to 90% compared to On-demand
  - Define max spot price and get the instance while current spot price < max
    - The hourly spot price varies based on offer and capacity
    - If the current spot price > your max price you can choose to stop or terminate your instance with a 2 minutes grace period.
  - Other strategy: Spot Block
    - “block” spot instance during a specified time frame (1 to 6 hours) without interruptions
    - In rare situations, the instance may be reclaimed
  - Used for batch jobs, data analysis, or workloads that are resilient to failures.
  - Not great for critical jobs or databases
  - Spot Request 
    - max price 
    - desired no of instances
    - launch spec
    - Request type 
      - one time 
	  - persistent 
    - valid from 
    - valid until
  - You can only cancel Spot Instance requests that are open, active, or disabled.
  - Cancelling a Spot Request does not terminate instances
  - You must first cancel a Spot Request, and then terminate the associated Spot Instances
  
## Spot Fleets

  - Spot Fleets = set of Spot Instances + (optional) On-Demand Instances
  - The Spot Fleet will try to meet the target capacity with price constraints
  - Define possible launch pools: instance type (m5.large), OS, Availability Zone
  - Can have multiple launch pools, so that the fleet can choose
  - Spot Fleet stops launching instances when reaching capacity or max cost
  - Strategies to allocate Spot Instances:
    - lowestPrice: from the pool with the lowest price (cost optimization, short workload)
    - diversified: distributed across all pools (great for availability, long workloads)
    - capacityOptimized: pool with the optimal capacity for the number of instances
  - Spot Fleets allow us to automatically request Spot Instances with the lowest price  

## EC2 Instance Types – Main ones

  - R: applications that needs a lot of RAM – in-memory caches
  - C: applications that needs good CPU – compute / databases
  - M: applications that are balanced (think “medium”) – general / web app
  - I: applications that need good local I/O (instance storage) – databases
  - G: applications that need a GPU – video rendering / machine learning
  - T2 / T3: burstable instances (up to a capacity)
  - T2 / T3 - unlimited: unlimited burst  

## Burstable Instances (T2/T3)

  - AWS has the concept of burstable instances (T2/T3 machines)
  - Burst means that overall, the instance has OK CPU performance.
  - When the machine needs to process something unexpected (a spike in load for example), it can burst, and CPU can be VERY good.
  - If the machine bursts, it utilizes “burst credits”
  - If all the credits are gone, the CPU becomes BAD
  - If the machine stops bursting, credits are accumulated over time
  - Burstable instances can be amazing to handle unexpected traffic and getting the insurance that it will be handled correctly
  - If your instance consistently runs low on credit, you need to move to a different kind of non-burstable instance

## T2/T3 Unlimited 

  - Nov 2017: It is possible to have an “unlimited burst credit balance”
  - You pay extra money if you go over your credit balance, but you don’t lose in performance
  - Overall, it is a new offering, so be careful, costs could go high if you’re not monitoring the health of your instances

## AMI

  - As we saw, AWS comes with base images such as:
    - Ubuntu
    - Fedora
    - RedHat
    - Windows
  - These images can be customised at runtime using EC2 User data
  - But what if we could create our own image, ready to go?
  - That’s an AMI – an image to use to create our instances
  - AMIs can be built for Linux or Windows machines

## Why would you use a custom AMI?

  - Pre-installed packages needed
  - Faster boot time (no need for ec2 user data at boot time)
  - Machine comes configured with monitoring / enterprise software
  - Security concerns – control over the machines in the network
  - Control of maintenance and updates of AMIs over time
  - Active Directory Integration out of the box
  - Installing your app ahead of time (for faster deploys when auto-scaling)
  - Using someone else’s AMI that is optimised for running an app, DB, etc…
  - AMI are built for a specific AWS region (!)

## Using Public AMIs

  - You can leverage AMIs from other people
  - You can also pay for other people’s AMI by the hour
  - These people have optimised the software
  - The machine is easy to run and configure
  - You basically rent “expertise” from the AMI creator
  - AMI can be found and published on the Amazon Marketplace
  - Do not use an AMI you don’t trust!
  - Some AMIs might come with malware or may not be secure for your enterprise

## AMI Storage

  - Your AMI take space and they live in Amazon S3
  - Amazon S3 is a durable, cheap and resilient storage where most of your backups will live (but you won’t see them in the S3 console)
  - By default, your AMIs are private, and locked for your account / region
  - You can also make your AMIs public and share them with other AWS accounts or sell them on the AMI Marketplace

## AMI Pricing

  - AMIs live in Amazon S3, so you get charged for the actual space in takes in Amazon S3
  - Amazon S3 pricing in US-EAST-1:
  - First 50 TB / month: $0.023 per GB
  - Next 450 TB / month: $0.022 per GB
  - Overall it is quite inexpensive to store private AMIs.
  - Make sure to remove the AMIs you don’t use

## Cross Account AMI Copy 

  - You can share an AMI with another AWS account.
  - Sharing an AMI does not affect the ownership of the AMI.
  - If you copy an AMI that has been shared with your account, you are the owner of the target AMI in your account.
  - To copy an AMI that was shared with you from another account, the owner of the source AMI must grant you read permissions for the storage that backs the AMI, either the associated EBS snapshot (for an Amazon EBS-backed AMI) or an associated S3 bucket (for an instance store-backed AMI).
  - Limits:
  - You can't copy an encrypted AMI that was shared with you from another account. Instead, if the underlying snapshot and encryption key were shared with you, you can copy the snapshot while reencrypting it with a key of your own. You own the copied snapshot, and can register it as a new AMI.
  - You can't copy an AMI with an associated billingProduct code that was shared with you from another account. This includes Windows AMIs and AMIs from the AWS Marketplace. To copy a shared AMI with a billingProduct code, launch an EC2 instance in your account using the shared AMI and then create an AMI from the instance.

## Placement Groups

  - Sometimes you want control over the EC2 Instance placement strategy
  - That strategy can be defined using placement groups
  - When you create a placement group, you specify one of the following strategies for the group:
  - Cluster
    - clusters instances into a low-latency group in a single Availability Zone
  - Spread
    - spreads instances across underlying hardware (max 7 instances per group per AZ)
  - Partition
    - spreads instances across many different partitions (which rely on different sets of racks) within an AZ. 
	- Scales to 100s of EC2 instances per group (Hadoop, Cassandra, Kafka)
	
## Placement Groups Cluster

  - Same Rack Same AZ
  - Pros: Great network (10 Gbps bandwidth between instances)
  - Cons: If the rack fails, all instances fails at the same time
  - Use case:
  - Big Data job that needs to complete fast
  - Application that needs extremely low latency and high network throughput	

## Placement Groups Spread

  - Pros:
    - Can span across Availability Zones (AZ)
    - Reduced risk is simultaneous failure
    - EC2 Instances are on different physical hardware
  - Cons:
    - Limited to 7 instances per AZ per placement group
  - Use case:
    - Application that needs to maximize high availability
    - Critical Applications where each instance must be isolated from failure from each other
	
## Placements Groups Partition

  - Up to 7 partitions per AZ
  - Up to 100s of EC2 instances
  - The instances in a partition do not share racks with the instances in the other partitions
  - A partition failure can affect many EC2 but won’t affect other partitions
  - EC2 instances get access to the partition information as metadata 
  - Use cases: HDFS, HBase, Cassandra, Kafka

## Elastic Network Interfaces (ENI)

  - Logical component in a VPC that represents a virtual network card
  - The ENI can have the following attributes:
    - Primary private IPv4, one or more secondary IPv4
    - One Elastic IP (IPv4) per private IPv4
    - One Public IPv4
    - One or more security groups
    - A MAC address
  - You can create ENI independently and attach  them on the fly (move them) on EC2 instances for failover
  - Bound to a specific availability zone (AZ)

## EC2 Hibernate

  - We know we can stop, terminate instances
  - Stop: the data on disk (EBS) is kept intact in the next start
  - Terminate: any EBS volumes (root) also set-up to be destroyed is lost
  - On start, the following happens:
  - First start: the OS boots & the EC2 User Data script is run
  - Following starts: the OS boots up
  - Then your application starts, caches get warmed up, and that can take time!

## Introducing EC2 Hibernate

  - The in-memory (RAM) state is preserved
  - The instance boot is much faster! (the OS is not stopped / restarted)
  - Under the hood: the RAM state is written to a file in the root EBS volume
  - The root EBS volume must be encrypted
  - Use cases:
    - long-running processing
    - saving the RAM state
    - services that take time to initialize 
  - Supported instance families - C3, C4, C5, M3, M4, M5, R3, R4, and R5.
  - Instance RAM size - must be less than 150 GB.
  - Instance size - not supported for bare metal instances.
  - AMI: Amazon Linux 2, Linux AMI, Ubuntu & Windows…
  - Root Volume: must be EBS, encrypted, not instance store, and large
  - Available for On-Demand and Reserved Instances
  - An instance cannot be hibernated more than 60 days	

# Load Balancing, Auto Scaling Groups

## Why use a load balancer?

  - Spread load across multiple downstream instances
  - Expose a single point of access (DNS) to your application
  - Seamlessly handle failures of downstream instances
  - Do regular health checks to your instances
  - Provide SSL termination (HTTPS) for your websites
  - Enforce stickiness with cookies
  - High availability across zones
  - Separate public traffic from private traffic

## Why use an ELB?

  - An ELB (EC2 Load Balancer) is a managed load balancer
  - AWS guarantees that it will be working
  - AWS takes care of upgrades, maintenance, high availability
  - AWS provides only a few configuration knobs
  - It costs less to setup your own load balancer but it will be a lot more effort on your end.
  - It is integrated with many AWS offerings / services

## Health Checks

  - Health Checks are crucial for Load Balancers
  - They enable the load balancer to know if instances it forwards traffic to are available to reply to requests
  - The health check is done on a port and a route (/health is common)
  - If the response is not 200 (OK), then the instance is unhealthy

## Types of load balancer on AWS

  - AWS has 3 kinds of managed Load Balancers
  - Classic Load Balancer (v1 - old generation) – 2009 - HTTP, HTTPS, TCP
  - Application Load Balancer (v2 - new generation) – 2016 -  HTTP, HTTPS, WebSocket
  - Network Load Balancer (v2 - new generation) – 2017 - TCP, TLS (secure TCP) & UDP
  - Overall, it is recommended to use the newer / v2 generation load balancers as they provide more features
  - You can setup internal (private) or external (public) ELBs

## Load Balancer Good to Know

  - LBs can scale but not instantaneously – contact AWS for a “warm-up”
  - Troubleshooting
  - 4xx errors are client induced errors
  - 5xx errors are application induced errors
  - Load Balancer Errors 503 means at capacity or no registered target
  - If the LB can’t connect to your application, check your security groups!
  - Monitoring
  - ELB access logs will log all access requests (so you can debug per request)
  - CloudWatch Metrics will give you aggregate statistics (ex: connections count)

## Classic Load Balancers (v1) 

  - Supports TCP (Layer 4), HTTP & HTTPS (Layer 7)
  - Health checks are TCP or HTTP based
  - Fixed hostname XXX.region.elb.amazonaws.com

## Application Load Balancer (v2)

  - Application load balancers is Layer 7 (HTTP)
  - Load balancing to multiple HTTP applications across machines (target groups)
  - Load balancing to multiple applications on the same machine (ex: containers)
  - Support for HTTP/2 and WebSocket
  - Support redirects (from HTTP to HTTPS for example)
  - Routing tables to different target groups:
  - Routing based on path in URL (example.com/users & example.com/posts)
  - Routing based on hostname in URL (one.example.com & other.example.com)
  - Routing based on Query String, Headers (example.com/users?id=123&order=false)
  - ALB are a great fit for micro services & container-based application (example: Docker & Amazon ECS)
  - Has a port mapping feature to redirect to a dynamic port in ECS
  - In comparison, we’d need multiple Classic Load Balancer per application
  - Fixed hostname (XXX.region.elb.amazonaws.com)
  - The application servers don’t see the IP of the client directly
  - The true IP of the client is inserted in the header X-Forwarded-For
  - We can also get Port (X-Forwarded-Port) and proto (X-Forwarded-Proto)

## Application Load Balancer (v2) Target Groups

  - EC2 instances (can be managed by an Auto Scaling Group) – HTTP
  - ECS tasks (managed by ECS itself) – HTTP
  - Lambda functions – HTTP request is translated into a JSON event
  - IP Addresses – must be private IPs
  - ALB can route to multiple target groups
  - Health checks are at the target group level

## Network Load Balancer (v2)

  - Network load balancers (Layer 4) allow to:
  - Forward TCP & UDP traffic to your instances
  - Handle millions of request per seconds
  - Less latency ~100 ms (vs 400 ms for ALB)
  - NLB has one static IP per AZ, and supports assigning Elastic IP (helpful for whitelisting specific IP)
  - NLB are used for extreme performance, TCP or UDP traffic
  - Not included in the AWS free tier

## Load Balancer Stickiness

  - It is possible to implement stickiness so that the same client is always redirected to the same instance behind a load balancer
  - This works for Classic Load Balancers &  Application Load Balancers
  - The “cookie” used for stickiness has an expiration date you control
  - Use case: make sure the user doesn’t lose his session data
  - Enabling stickiness may bring imbalance to the load over the backend EC2 instances

## Cross-Zone Load Balancing 

  - With Cross Zone Load Balancing: each load balancer instance distributes evenly across all registered instances in all AZ
  - Otherwise, each load balancer node distributes requests evenly across the registered instances in its Availability Zone only.
  - Classic Load Balancer
    - Disabled by default
    - No charges for inter AZ data if enabled
  - Application Load Balancer
    - Always on (can’t be disabled)
    - No charges for inter AZ data
  - Network Load Balancer
    - Disabled by default
    - You pay charges ($) for inter AZ data if enabled
	
## SSL/TLS - Basics

  - An SSL Certificate allows traffic between your clients and your load balancer to be encrypted in transit (in-flight encryption)
  - SSL refers to Secure Sockets Layer, used to encrypt connections
  - TLS refers to Transport Layer Security, which is a newer version
  - Nowadays, TLS certificates are mainly used, but people still refer as SSL
  - Public SSL certificates are issued by Certificate Authorities (CA)
  - Comodo, Symantec, GoDaddy, GlobalSign, Digicert, Letsencrypt, etc…
  - SSL certificates have an expiration date (you set) and must be renewed	

## Load Balancer - SSL Certificates

  - The load balancer uses an X.509 certificate (SSL/TLS server certificate)
  - You can manage certificates using ACM (AWS Certificate Manager)
  - You can create upload your own certificates alternatively
  - HTTPS listener:
  - You must specify a default certificate
  - You can add an optional list of certs to support multiple domains
  - Clients can use SNI (Server Name Indication) to specify the hostname they reach
  - Ability to specify a security policy to support older versions of SSL / TLS (legacy clients)
  - Classic Load Balancer (v1)
    - Support only one SSL certificate
    - Must use multiple CLB for multiple hostname with multiple SSL certificates
  - Application Load Balancer (v2)
    - Supports multiple listeners with multiple SSL certificates
    - Uses Server Name Indication (SNI) to make it work
  - Network Load Balancer (v2)
    - Supports multiple listeners with multiple SSL certificates
    - Uses Server Name Indication (SNI) to make it work

## SSL – Server Name Indication (SNI)

  - SNI solves the problem of loading multiple SSL certificates onto one web server (to serve multiple websites)
  - It’s a “newer” protocol, and requires the client to indicate the hostname of the target server in the initial SSL handshake
  - The server will then find the correct certificate, or return the default one 
  - Only works for ALB & NLB (newer generation), CloudFront
  - Does not work for CLB (older gen)

## ELB – Connection Draining

  - Feature naming:
  - CLB: Connection Draining
  - Target Group: Deregistration Delay (for ALB & NLB)
  - Time to complete “in-flight requests” while the instance is de-registering or unhealthy
  - Stops sending new requests to the instance which is de-registering
  - Between 1 to 3600 seconds, default is 300 seconds
  - Can be disabled (set value to 0)
  - Set to a low value if your requests are short

## Auto Scaling Group

  - Scale out (add EC2 instances) to match an increased load
  - Scale in (remove EC2 instances) to match a decreased load
  - Ensure we have a minimum and a maximum number of machines running
  - Automatically Register new instances to a load balancer
  - Scaling policies can be on CPU, Network… and can even be on custom metrics or based on a schedule (if you know your visitors patterns)
  - ASGs use Launch configurations or Launch Templates (newer)
  - To update an ASG, you must provide a new launch configuration / launch template
  - IAM roles attached to an ASG will get assigned to EC2 instances
  - ASG are free. You pay for the underlying resources being launched
  - Having instances under an ASG means that if they get terminated for whatever reason, the ASG will automatically create new ones as a replacement. Extra safety!
  - ASG can terminate instances marked as unhealthy by an LB (and hence replace them)

## ASGs have the following attributes

  - A launch configuration
  - AMI + Instance Type
  - EC2 User Data
  - EBS Volumes
  - Security Groups
  - SSH Key Pair
  - Min Size / Max Size / Initial Capacity
  - Network + Subnets Information
  - Load Balancer Information
  - Scaling Policies

## Auto Scaling Alarms
  - It is possible to scale an ASG based on CloudWatch alarms
  - An Alarm monitors a metric (such as Average CPU)
  - Metrics are computed for the overall ASG instances
  - Based on the alarm:
  - We can create scale-out policies (increase the number of instances)
  - We can create scale-in policies (decrease the number of instances)

## Auto Scaling New Rules

  - It is now possible to define ”better” auto scaling rules that are directly managed by EC2
  - Target Average CPU Usage
  - Number of requests on the ELB per instance
  - Average Network In
  - Average Network Out
  - These rules are easier to set up and can make more sense

## Auto Scaling Custom Metric

  - We can auto scale based on a custom metric (ex: number of connected users)
  - 1. Send custom metric from application on EC2 to CloudWatch (PutMetric API)
  - 2. Create CloudWatch alarm to react to low / high values
  - 3. Use the CloudWatch alarm as the scaling policy for ASG

## Auto Scaling Groups – Scaling Policies

  - Target Tracking Scaling
    - Most simple and easy to set-up
    - Example: I want the average ASG CPU to stay at around 40%
  - Simple / Step Scaling
    - When a CloudWatch alarm is triggered (example CPU > 70%), then add 2 units
    - When a CloudWatch alarm is triggered (example CPU < 30%), then remove 1
  - Scheduled Actions
    - Anticipate a scaling based on known usage patterns
    - Example: increase the min capacity to 10 at 5 pm on Fridays
	
## Scaling Cooldowns

  - The cooldown period helps to ensure that your Auto Scaling group doesn't launch or terminate additional instances before the previous scaling activity takes effect.
  - In addition to default cooldown for Auto Scaling group, we can create cooldowns that apply to a specific simple scaling policy
  - A scaling-specific cooldown period overrides the default cooldown period.
  - One common use for scaling-specific cooldowns is with a scale-in policy—a policy that terminates instances based on a specific criteria or metric. Because this policy terminates instances, Amazon EC2 Auto Scaling needs less time to determine whether to terminate additional instances.
  - If the default cooldown period of 300 seconds is too long—you can reduce costs by applying a scaling-specific cooldown period of 180 seconds to the scale-in policy.
  - If your application is scaling up and down multiple times each hour, modify the Auto Scaling Groups cool-down timers and the CloudWatch Alarm Period that triggers the scale in

## ASG Default Termination Policy 

  - 1. Find the AZ which has the most number of instances
  - 2. If there are multiple instances in the AZ to choose from, delete the one with the oldest launch configuration
  - ASG tries the balance the number of instances across AZ by default

## ASG Lifecycle Hooks

  - By default as soon as an instance is launched in an ASG it’s in service.
  - You have the ability to perform extra steps before the instance goes in service (Pending state)
  - You have the ability to perform some actions before the instance is terminated (Terminating state)

## Launch Template vs Launch Configuration

  - Both:
    - ID of the Amazon Machine Image (AMI), the instance type, a key pair, security groups, and the other parameters that you use to launch EC2 instances (tags, EC2 user-data...)
  - Launch Configuration (legacy):
    - Must be re-created every time
  - Launch Template (newer):
    - Can have multiple versions
    - Create parameters subsets (partial configuration for re-use and inheritance)
    - Provision using both On-Demand and Spot instances (or a mix)
    - Can use T2 unlimited burst feature
    - Recommended by AWS going forward
	
# EC2 Storage - EBS & EFS	

## EBS Volume
   - An EC2 machine loses its root volume (main drive) when it is manually terminated.
   - Unexpected terminations might happen from time to time (AWS would email you)
   - Sometimes, you need a way to store your instance data somewhere
   - An EBS (Elastic Block Store) Volume is a network drive you can attach to your instances while they run
   - It allows your instances to persist data

## EBS Volume
   - It’s a network drive (i.e. not a physical drive)
   - It uses the network to communicate the instance, which means there might be a bit of latency
   - It can be detached from an EC2 instance and attached to another one quickly
   - It’s locked to an Availability Zone (AZ)
   - An EBS Volume in us-east-1a cannot be attached to us-east-1b
   - To move a volume across, you first need to snapshot it
   - Have a provisioned capacity (size in GBs, and IOPS)
   - You get billed for all the provisioned capacity
   - You can increase the capacity of the drive over time

## EBS Volume Types
   - EBS Volumes come in 4 types
   - GP2 (SSD): General purpose SSD volume that balances price and performance for awide variety of workloads
   - IO1 (SSD): Highest-performance SSD volume for mission-critical low-latency or high throughput workloads
   - ST1 (HDD): Low cost HDD volume designed for frequently accessed, throughput intensive workloads
   - SC1 (HDD): Lowest cost HDD volume designed for less frequently accessed workloads
   - EBS Volumes are characterized in Size | Throughput | IOPS (I/O Ops Per Sec)
   - When in doubt always consult the AWS documentation – it’s good!
   - Only GP2 and IO1 can be used as boot volumes

## EBS Volume Types Use cases - GP2 
   - Recommended for most workloads
   - System boot volumes
   - Virtual desktops
   - Low-latency interactive apps
   - Development and test environments
   - 1 GiB - 16 TiB
   - Small gp2 volumes can burst IOPS to 3000
   - Max IOPS is 16,000…
   - 3 IOPS per GB, means at 5,334GB we are at the max IOPS

## EBS Volume Types Use cases - IO1
   - Critical business applications that require sustained IOPS performance, or more than 16,000 IOPS per volume (gp2 limit)
   - Large database workloads, such as:
   - MongoDB, Cassandra, Microsoft SQL Server, MySQL, PostgreSQL, Oracle
   - 4 GiB - 16 TiB
   - IOPS is provisioned (PIOPS) – MIN 100 - MAX 64,000 (Nitro instances) else MAX 32,000 (other instances)
   - The maximum ratio of provisioned IOPS to requested volume size (in GiB) is 50:1

## EBS Volume Types Use cases - ST1
   - Streaming workloads requiring consistent, fast throughput at a low price.
   - Big data, Data warehouses, Log processing
   - Apache Kafka
   - Cannot be a boot volume
   - 500 GiB - 16 TiB
   - Max IOPS is 500
   - Max throughput of 500 MiB/s – can burst

## EBS Volume Types Use cases - SC1
   - Throughput-oriented storage for large volumes of data that is infrequently accessed
   - Scenarios where the lowest storage cost is important
   - Cannot be a boot volume
   - 500 GiB - 16 TiB
   - Max IOPS is 250
   - Max throughput of 250 MiB/s – can burst

## EBS –Volume Types Summary
   - gp2: General Purpose Volumes (cheap)
     - 3 IOPS / GiB, minimum 100 IOPS, burst to 3000 IOPS, max 16000 IOPS
     - 1 GiB – 16 TiB , +1 TB = +3000 IOPS
   - io1: Provisioned IOPS (expensive)
     - Min 100 IOPS, Max 64000 IOPS (Nitro) or 32000 (other)
     - 4 GiB - 16 TiB. Size of volume and IOPS are independent
   - st1: Throughput Optimized HDD
     - 500 GiB – 16 TiB , 500 MiB /s throughput
   - sc1: Cold HDD, Infrequently accessed data
     - 500 GiB – 16 TiB , 250 MiB /s throughput

## EBS Snapshots
   - Incremental – only backup changed blocks
   - EBS backups use IO and you shouldn’t run them while your application is handling a lot of traffic
   - Snapshots will be stored in S3 (but you won’t directly see them)
   - Not necessary to detach volume to do snapshot, but recommended
   - Max 100,000 snapshots
   - Can copy snapshots across AZ or Region
   - Can make Image (AMI) from Snapshot
   - EBS volumes restored by snapshots need to be pre-warmed (using fio or dd command to read the entire volume)
   - Snapshots can be automated using Amazon Data Lifecycle Manager

## EBS Migration
   - EBS Volumes are only locked to a specific AZ
   - To migrate it to a different AZ (or region):
   - Snapshot the volume
   - (optional) Copy the volume to a different region
   - Create a volume from the snapshot in the AZ of your choice
   
## EBS Encryption
   - When you create an encrypted EBS volume, you get the following:
     - Data at rest is encrypted inside the volume
     -  All the data in flight moving between the instance and the volume is encrypted
     - All snapshots are encrypted
     - All volumes created from the snapshot
   - Encryption and decryption are handled transparently (you have nothing to do)
   - Encryption has a minimal impact on latency
   - EBS Encryption leverages keys from KMS (AES-256)
   - Copying an unencrypted snapshot allows encryption
   - Snapshots of encrypted volumes are encrypted

## Encryption: encrypt an unencrypted EBS volume
   - Create an EBS snapshot of the volume
   - Encrypt the EBS snapshot ( using copy )
   - Create new ebs volume from the snapshot ( the volume will also be encrypted )
   - Now you can attach the encrypted volume to the original instance

## EBS vs Instance Store
   - Some instance do not come with Root EBS volumes
   - Instead, they come with “Instance Store” (= ephemeral storage)
   - Instance store is physically attached to the machine (EBS is a network drive)
   - Pros:
     - Better I/O performance (EBS gp2 has an max IOPS of 16000, io1 of 64000)
     - Good for buffer / cache / scratch data / temporary content
     - Data survives reboots
   - Cons:
     - On stop or termination, the instance store is lost
     - You can’t resize the instance store
     - Backups must be operated by the user

## Local EC2 Instance Store
   - Physical disk attached to the physical server where your EC2 is
   - Very High IOPS (because physical)
   - Disks up to 7.5 TiB (can change over time), stripped to reach 30 TiB (can change over time…)
   - Block Storage (just like EBS)
   - Cannot be increased in size
   - Risk of data loss if hardware fails

## EBS RAID Options
   - EBS is already redundant storage (replicated within an AZ)
   - But what if you want to increase IOPS to say 100 000 IOPS?
   - What if you want to mirror your EBS volumes?
   - You would mount volumes in parallel in RAID settings!
   - RAID is possible as long as your OS supports it
   - Some RAID options are:
     - RAID 0
     - RAID 1
     - RAID 5 (not recommended for EBS – see documentation)
     - RAID 6 (not recommended for EBS – see documentation)

## RAID 0 (increase performance)
   - Combining 2 or more volumes and getting the total disk space and I/O
   - But one disk fails, all the data is failed
   - Use cases would be:
     - An application that needs a lot of IOPS and doesn’t need fault-tolerance
     - A database that has replication already built-in
   - Using this, we can have a very big disk with a lot of IOPS
   - For example
     - two 500 GiB Amazon EBS io1 volumes with 4,000 provisioned IOPS each will create a…
     - 1000 GiB RAID 0 array with an available bandwidth of 8,000 IOPS and 1,000 MB/s of throughput

## RAID 1 (increase fault tolerance)
   - RAID 1 = Mirroring a volume to another
   - If one disk fails, our logical volume is still working
   - We have to send the data to two EBS volume at the same time (2x network)
   - Use case:
     - Application that need increase volume fault tolerance
     - Application where you need to service disks
   - For example:
     - two 500 GiB Amazon EBS io1 volumes with 4,000 provisioned IOPS each will create a…
     - 500 GiB RAID 1 array with an available bandwidth of 4,000 IOPS and 500 MB/s of throughput

## EFS – Elastic File System
   - Managed NFS (network file system) that can be mounted on many EC2
   - EFS works with EC2 instances in multi-AZ
   - can be shared across AZ
   - Highly available, scalable, expensive (3x gp2), pay per use
   - Use cases: content management, web serving, data sharing, Wordpress
   - Uses NFSv4.1 protocol
   - Uses security group to control access to EFS
   - Compatible with Linux based AMI (not Windows)
   - Encryption at rest using KMS
   - POSIX file system (~Linux) that has a standard file API
   - File system scales automatically, pay-per-use, no capacity planning!

## EFS – Performance & Storage Classes
   - EFS Scale
   - 1000s of concurrent NFS clients, 10 GB+ /s throughput
   - Grow to Petabyte-scale network file system, automatically
   - Performance mode (set at EFS creation time)
     - General purpose (default): latency-sensitive use cases (web server, CMS, etc…)
     - Max I/O – higher latency, throughput, highly parallel (big data, media processing)
   - Storage Tiers (lifecycle management feature – move file after N days)
     - Standard: for frequently accessed files
     - Infrequent access (EFS-IA): cost to retrieve files, lower price to store

## EBS vs EFS – Elastic Block Storage
   - EBS volumes…
     - can be attached to only one instance at a time
     - are locked at the Availability Zone (AZ) level
     - gp2: IO increases if the disk size increases
     - io1: can increase IO independently
   - To migrate an EBS volume across AZ
     - Take a snapshot
     - Restore the snapshot to another AZ
     - EBS backups use IO and you shouldn’t run them while your application is handling a lot of traffic
   - Root EBS Volumes of instances get terminated by default if the EC2 instance gets terminated.  (you can disable that)
   - EBS volumes…
     - Mounting 100s of instances across AZ
     - EFS share website files (WordPress)
     - Only for Linux Instances (POSIX)
     - EFS has a higher price point than EBS
     - Can leverage EFS-IA for cost savings
     - Remember: EFS vs EBS vs Instance Store
	 
# RDS, Aurora & ElastiCache

## AWS RDS Overview
   - RDS stands for Relational Database Service
   - It’s a managed DB service for DB use SQL as a query language.
   - It allows you to create databases in the cloud that are managed by AWS
     - Postgres
     - MySQL
     - MariaDB
     - Oracle
     - Microsoft SQL Server
     - Aurora (AWS Proprietary database)

## Advantage over using RDS versus deploying DB on EC2
   - RDS is a managed service:
   - Automated provisioning, OS patching
   - Continuous backups and restore to specific timestamp (Point in Time Restore)!
   - Monitoring dashboards
   - Read replicas for improved read performance
   - Multi AZ setup for DR (Disaster Recovery)
   - Maintenance windows for upgrades
   - Scaling capability (vertical and horizontal)
   - Storage backed by EBS (gp2 or io1)
   - BUT you can’t SSH into your instances

## RDS Backups
   - Backups are automatically enabled in RDS
   - Automated backups:
     - Daily full backup of the database (during the maintenance window)
     - Transaction logs are backed-up by RDS every 5 minutes
     - => ability to restore to any point in time (from oldest backup to 5 minutes ago)
     - 7 days retention (can be increased to 35 days)
   - DB Snapshots:
     - Manually triggered by the user
     - Retention of backup for as long as you want

## RDS Read Replicas for read scalability
   - Up to 5 Read Replicas
   - Within AZ, Cross AZ or Cross Region
   - Replication is ASYNC, so reads are eventually consistent
   - Replicas can be promoted to their own DB
   - Applications must update the connection string to leverage read replicas

## RDS Read Replicas – Use Cases
   - You have a production database that is taking on normal load
   - You want to run a reporting application to run some analytics
   - You create a Read Replica to run the new workload there
   - The production application is unaffected
   - Read replicas are used for SELECT (=read) only kind of statements (not INSERT, UPDATE, DELETE)

## RDS Read Replicas – Network Cost
   - In AWS there’s a network cost when data goes from one AZ to another
   - To reduce the cost, you can have your Read Replicas in the same AZ

## RDS Multi AZ (Disaster Recovery)
   - SYNC replication
   - One DNS name – automatic app failover to standby
   - Increase availability
   - Failover in case of loss of AZ, loss of network, instance or storage failure
   - No manual intervention in apps
   - Not used for scaling
   - Note:The Read Replicas be setup as Multi AZ for Disaster Recovery (DR)

## RDS Security - Encryption
   - At rest encryption
     - Possibility to encrypt the master & read replicas with AWS KMS - AES-256 encryption
     - Encryption has to be defined at launch time
     - If the master is not encrypted, the read replicas cannot be encrypted
     - Transparent Data Encryption (TDE) available for Oracle and SQL Server
   - In-flight encryption
     - SSL certificates to encrypt data to RDS in flight
     - Provide SSL options with trust certificate when connecting to database
     - To enforce SSL:
       - PostgreSQL: rds.force_ssl=1 in the AWS RDS Console (Parameter Groups)
       - MySQL: Within the DB: GRANT USAGE ON *.* TO 'mysqluser'@'%' REQUIRE SSL;

## RDS Encryption Operations
   - Encrypting RDS backups
     - Snapshots of un-encrypted RDS databases are un-encrypted
     - Snapshots of encrypted RDS databases are encrypted
     - Can copy a snapshot into an encrypted one
   - To encrypt an un-encrypted RDS database:
     - Create a snapshot of the un-encrypted database
     - Copy the snapshot and enable encryption for the snapshot
     - Restore the database from the encrypted snapshot
     - Migrate applications to the new database, and delete the old database

## RDS Security – Network & IAM
   - Network Security
     - RDS databases are usually deployed within a private subnet, not in a public one
     - RDS security works by leveraging security groups (the same concept as for EC2 instances) 
     – it controls which IP / security group can communicate with RDS 
   - Access Management
     - IAM policies help control who can manage AWS RDS (through the RDS API)
     - Traditional Username and Password can be used to login into the database
     - IAM-based authentication can be used to login into RDS MySQL & PostgreSQL

## RDS - IAM Authentication
   - IAM database authentication works with MySQL and PostgreSQL
   - You don’t need a password, just an authentication token obtained through IAM & RDS API calls
   - Auth token has a lifetime of 15 minutes
   - Benefits:
     - Network in/out must be encrypted using SSL
     - IAM to centrally manage users instead of DB
     - Can leverage IAM Roles and EC2 Instance profiles for easy integration

## RDS Security – Summary
   - Encryption at rest:
     - Is done only when you first create the DB instance
     - or: unencrypted DB => snapshot => copy snapshot as encrypted => create DB from snapshot
   - Your responsibility:
     - Check the ports / IP / security group inbound rules in DB’s SG
     - In-database user creation and permissions or manage through IAM
     - Creating a database with or without public access
     - Ensure parameter groups or DB is configured to only allow SSL connections
   - AWS responsibility:
     - No SSH access
     - No manual DB patching
     - No manual OS patching
     - No way to audit the underlying instance

## Amazon Aurora
   - Aurora is a proprietary technology from AWS (not open sourced)
   - Postgres and MySQL are both supported as Aurora DB (that means your drivers will work as if Aurora was a Postgres or MySQL database)
   - Aurora is “AWS cloud optimized” and claims 5x performance improvement over MySQL on RDS, 
   - over 3x the performance of Postgres on RDS
   - Aurora storage automatically grows in increments of 10GB, up to 64 TB.
   - Aurora can have 15 replicas while MySQL has 5, and the replication process is faster (sub 10 ms replica lag)
   - Failover in Aurora is instantaneous. It’s HA (High Availability) native.
   - Aurora costs more than RDS (20% more) – but is more efficient

## Aurora High Availability and Read Scaling
   - 6 copies of your data across 3 AZ:
     - 4 copies out of 6 needed for writes
     - 3 copies out of 6 need for reads
     - Self healing with peer-to-peer replication
     - Storage is striped across 100s of volumes
   - One Aurora Instance takes writes (master)
   - Automated failover for master in less than 30 seconds
   - Master + up to 15 Aurora Read Replicas serve reads
   - Support for Cross Region Replication
   - Writer Endpoint 
   - Reader Endpoint

## Features of Aurora
   - Automatic fail-over
   - Backup and Recovery
   - Isolation and security
   - Industry compliance
   - Push-button scaling
   - Automated Patching with Zero Downtime
   - Advanced Monitoring
   - Routine Maintenance
   - Backtrack: restore data at any point of time without using backups

## Aurora Security
   - Similar to RDS because uses the same engines
   - Encryption at rest using KMS
   - Automated backups, snapshots and replicas are also encrypted
   - Encryption in flight using SSL (same process as MySQL or Postgres)
   - Possibility to authenticate using IAM token (same method as RDS)
   - You are responsible for protecting the instance with security groups
   - You can’t SSH

## Aurora Serverless
   - Automated database Client instantiation and autoscaling based on actual usage
   - Good for infrequent, intermittent or unpredictable workloads
   - No capacity planning needed
   - Pay per second, can be more cost-effective

## Global Aurora
   - Aurora Cross Region Read Replicas:
     - Useful for disaster recovery
     - Simple to put in place
   - Aurora Global Database (recommended):
     - 1 Primary Region (read / write)
     - Up to 5 secondary (read-only) regions, replication lag is less than 1 second
     - Up to 16 Read Replicas per secondary region
     - Helps for decreasing latency
     - Promoting another region (for disaster recovery) has an RTO of < 1 minute

## Amazon ElastiCache Overview
   - The same way RDS is to get managed Relational Databases…
   - ElastiCache is to get managed Redis or Memcached
   - Caches are in-memory databases with really high performance, low latency
   - Helps reduce load off of databases for read intensive workloads
   - Helps make your application stateless
   - AWS takes care of OS maintenance / patching, optimizations, setup, configuration, monitoring, failure recovery and backups
   - Using ElastiCache involves heavy application code changes

## ElastiCache Solution Architecture - DB Cache 
   - Applications queries ElastiCache, if not available, get from RDS and store in ElastiCache.
   - Helps relieve load in RDS
   - Cache must have an invalidation strategy to make sure only the most current data is used in there.

## ElastiCache Solution Architecture – User Session Store
   - User logs into any of the application
   - The application writes the session data into ElastiCache
   - The user hits another instance of our application
   - The instance retrieves the data and the user is already logged in

## ElastiCache – Redis vs Memcached
   - REDIS
     - Multi AZ with Auto-Failover
     - Read Replicas to scale reads and have high availability
     - Data Durability using AOF persistence
     - Backup and restore features 
   - MEMCACHED
     - Multi-node for partitioning of data (sharding)
     - Non persistent
     - No backup and restore
     - Multi-threaded architecture

## ElastiCache – Cache Security
   - All caches in ElastiCache:
     - Support SSL in flight encryption
     - Do not support IAM authentication
     - IAM policies on ElastiCache are only used for AWS API-level security
   - Redis AUTH
     - You can set a “password/token” when you create a Redis cluster
     - This is an extra level of security for your cache (on top of security groups)
   - Memcached
     - Supports SASL-based authentication (advanced)

## Patterns for ElastiCache
   - Lazy Loading: all the read data is cached, data can become stale in cache
   - Write Through: Adds or update data in the cache when written to a DB (no stale data)
   - Session Store: store temporary session data in a cache (using TTL features)
   - There are only two hard things in Computer Science: 
     - cache invalidation and 
	 - naming things	 

# Route53

## AWS Route 53 Overview
   - Route53 is a Managed DNS (Domain Name System)
   - DNS is a collection of rules and records which helps clients understand how to reach a server through URLs.
   - In AWS, the most common records are:
     - A: hostname to IPv4
     - AAAA: hostname to IPv6
     - CNAME: hostname to hostname
     - Alias: hostname to AWS resource
   - Route53 can use:
     - public domain names you own (or buy) application1.mypublicdomain.com
     - private domain names that can be resolved by your instances in your VPCs. application1.company.internal
   - Route53 has advanced features such as:
     - Load balancing (through DNS – also called client load balancing)
     - Health checks (although limited…)
     - Routing policy: simple, failover, geolocation, latency, weighted, multi value
   - You pay $0.50 per month per hosted zone

## DNS Records TTL (Time to Live)
   - High TTL: (e.g. 24hr)
     - Less traffic on DNS
     - Possibly outdated records
   - Low TTL: (e.g 60 s)
     - More traffic on DNS
     - Records are outdated for less time
     - Easy to change records
   - TTL is mandatory for each DNS record

## CNAME vs Alias
   - AWS Resources (Load Balancer, CloudFront…) 
   - expose an AWS hostname: lb1-1234.us-east-2.elb.amazonaws.com and you want myapp.mydomain.com
   - CNAME:
     - Points a hostname to any other hostname. (app.mydomain.com => blabla.anything.com)
     - ONLY FOR NON ROOT DOMAIN (aka. something.mydomain.com)
   - Alias:
     - Points a hostname to an AWS Resource (app.mydomain.com => blabla.amazonaws.com)
     - Works for ROOT DOMAIN and NON ROOT DOMAIN (aka mydomain.com)
     - Free of charge
     - Native health check

## Simple Routing Policy
   - Maps a hostname to another hostname
   - Use when you need to redirect to a single resource
   - You can’t attach health checks to simple routing policy
   - If multiple values are returned, a random one is chosen by the client

## Weighted Routing Policy
   - Control the % of the requests that go to specific endpoint
   - Helpful to test 1% of traffic on new app version for example
   - Helpful to split traffic between two regions
   - Can be associated with Health Checks

## Latency Routing Policy
   - Redirect to the server that has the least latency close to us
   - Super helpful when latency of users is a priority
   - Latency is evaluated in terms of user to designated AWS Region
   - Germany may be directed to the US (if that’s the lowest latency)

## Failover Routing Policy
   - Route based on health check 
   - Health check (mandatory)
   - DR

## Health Checks
   - Have X health checks failed => unhealthy (default 3)
   - After X health checks passed => health (default 3)
   - Default Health Check Interval: 30s (can set to 10s – higher cost)
   - About 15 health checkers will check the endpoint health
   - => one request every 2 seconds on average
   - Can have HTTP, TCP and HTTPS health checks (no SSL verification)
   - Possibility of integrating the health check with CloudWatch
   - Health checks can be linked to Route53 DNS queries!

## Geo Location Routing Policy
   - Different from Latency based!
   - This is routing based on user location
   - Here we specify: traffic from the UK should go to this specific IP
   - Should create a “default” policy (in case there’s no match on location)

## Multi Value Routing Policy
   - Use when routing traffic to multiple resources
   - Want to associate a Route 53 health checks with records
   - Up to 8 healthy records are returned for each Multi Value query
   - Multi Value is not a substitute for having an ELB


## Route53 as a Registrar
   - A domain name registrar is an organization that manages the reservation of Internet domain names
   - Famous names:
     - GoDaddy
     - Google Domains
     - And also… Route53 (e.g. AWS)!
   - Domain Registrar != DNS

## 3rd Party Registrar with AWS Route 53
   - If you buy your domain on 3rd party website, you can still use Route53.
   - 1) Create a Hosted Zone in Route 53
   - 2) Update NS Records on 3rd party website to use Route 53 name servers
   - Domain Registrar != DNS
   - (But each domain registrar usually comes with some DNS features)
   
## AWS Elastic Beanstalk 
   - Elastic Beanstalk is a developer centric view of deploying an application on AWS
   - It uses all the component’s we’ve seen before: EC2, ASG, ELB, RDS, etc…
   - But it’s all in one view that’s easy to make sense of!
   - We still have full control over the configuration
   - Beanstalk is free but you pay for the underlying instances
   - Managed service
   - Instance configuration / OS is handled by Beanstalk
   - Deployment strategy is configurable but performed by Elastic Beanstalk
   - Just the application code is the responsibility of the developer
   - Three architecture models:
     - Single Instance deployment: good for dev
     - LB + ASG: great for production or pre-production web applications
     - ASG only: great for non-web apps in production (workers, etc..)
   - Elastic Beanstalk has three components
     - Application
     - Application version: each deployment gets assigned a version
     - Environment name (dev, test, prod…): free naming
   - You deploy application versions to environments and can promote application versions to the next environment
   - Rollback feature to previous application version
   - Full control over lifecycle of environments
   - Support for many platforms:
     - Go
     - Java SE
     - Java with Tomcat
     - NET on Windows Server with IIS
     - Node.js
     - PHP
     - Python
     - Ruby
     - Packer Builder
     - Single Container Docker
     - Multicontainer Docker
     - Preconfigured Docker
   - If not supported, you can write your custom platform (advanced)   

	 
