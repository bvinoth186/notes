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

# S3 Storage and Data Management
  
## Section introduction
   - Amazon S3 is one of the main building blocks of AWS
   - It’s advertised as ”infinitely scaling” storage
   - It’s widely popular and deserves its own section
   - Many websites use Amazon S3 as a backbone
   - Many AWS services uses Amazon S3 as an integration as well
  
## Amazon S3 Overview - Buckets
   - Amazon S3 allows people to store objects (files) in “buckets” (directories)
   - Buckets must have a globally unique name
   - Buckets are defined at the region level
   - Naming convention
     - No uppercase
     - No underscore
     - 3-63 characters long
     - Not an IP
     - Must start with lowercase letter or number

  
## Amazon S3 Overview - Objects
   - Objects (files) have a Key    
   - The key is the FULL path:    
     - s3://my-bucket/my_file.txt    
     - s3://my-bucket/my_folder1/another_folder/my_file.txt    
   - The key is composed of prefix + object name    
   - s3://my-bucket/my_folder1/another_folder/my_file.txt    
   - There’s no concept of “directories” within buckets (although the UI will trick you to think otherwise)
   - Just keys with very long names that contain slashes (“/”)
   - Object values are the content of the body:
     - Max Object Size is 5TB (5000GB)
     - If uploading more than 5GB, must use “multi-part upload”
   - Metadata (list of text key / value pairs – system or user metadata)
   - Tags (Unicode key / value pair – up to 10) – useful for security / lifecycle
   - Version ID (if versioning is enabled)

  
## Amazon S3 -Versioning
   - You can version your files in Amazon S3
   - It is enabled at the bucket level
   - Same key overwrite will increment the “version”: 1, 2, 3….
   - It is best practice to version your buckets
     - Protect against unintended deletes (ability to restore a version)
     - Easy roll back to previous version
   - Notes:
     - Any file that is not versioned prior to enabling versioning will have version “null”
     - Suspending versioning does not delete the previous versions

  
## S3 Encryption for Objects
   - There are 4 methods of encrypting objects in S3
     - SSE-S3: encrypts S3 objects using keys handled & managed by AWS
     - SSE-KMS: leverage AWS Key Management Service to manage encryption keys
     - SSE-C: when you want to manage your own encryption keys
     - Client Side Encryption
   - It’s important to understand which ones are adapted to which situation for the exam

  
## SSE-S3
   - SSE-S3: encryption using keys handled & managed by Amazon S3
   - Object is encrypted server side
   - AES-256 encryption type
   - Must set header: “x-amz-server-side-encryption": "AES256"

## SSE-KMS
   - SSE-KMS: encryption using keys handled & managed by KMS
   - KMS Advantages: user control + audit trail
   - Object is encrypted server side
   - Must set header: “x-amz-server-side-encryption": ”aws:kms"

## SSE-C
   - SSE-C: server-side encryption using data keys fully managed by the customer outside of AWS
   - Amazon S3 does not store the encryption key you provide
   - HTTPS must be used
   - Encryption key must provided in HTTP headers, for every HTTP request made

## Client Side Encryption    
   - Client library such as the Amazon S3 Encryption Client    
   - Clients must encrypt data themselves before sending to S3    
   - Clients must decrypt data themselves when retrieving from S3    
   - Customer fully manages the keys and encryption cycle
  
## Encryption in transit (SSL/TLS)
   - Amazon S3 exposes:
     - HTTP endpoint: non encrypted
     - HTTPS endpoint: encryption in flight
   - You’re free to use the endpoint you want, but HTTPS is recommended
   - Most clients would use the HTTPS endpoint by default
   - HTTPS is mandatory for SSE-C
   - Encryption in flight is also called SSL / TLS 

  
## S3 Security
   - User based
     - IAM policies 
	 - which API calls should be allowed for a specific user from IAM console
   - Resource Based
     - Bucket Policies 
	 - bucket wide rules from the S3 console 
	 - allows cross account
     - Object Access Control List (ACL) – finer grain
     - Bucket Access Control List (ACL) – less common
   - Note: 
     - an IAM principal can access an S3 object if
     - the user IAM permissions allow it OR the resource policy ALLOWS it
     - AND there’s no explicit DENY

  
## S3 Bucket Policies    
   - JSON based policies    
     - Resources: buckets and objects    
	 - Actions: Set of API to Allow or Deny    
	 - Effect: Allow / Deny    
	 - Principal: The account or user to apply the policy to
   - Use S3 bucket for policy to:    
     - Grant public access to the bucket    
	 - Force objects to be encrypted at upload    
	 - Grant access to another account (Cross Account)

  
## Bucket settings for Block Public Access
   - Block public access to buckets and objects granted through
     - new access control lists (ACLs)
     - any access control lists (ACLs)
     - new public bucket or access point policies
   - Block public and cross-account access to buckets and objects through any public bucket or access point policies
   - These settings were created to prevent company data leaks
   - If you know your bucket should never be public, leave these on
   - Can be set at the account level

  
## S3 Security - Other
   - Networking:
     - Supports VPC Endpoints (for instances in VPC without www internet)
   - Logging and Audit:
     - S3 Access Logs can be stored in other S3 bucket
     - API calls can be logged in AWS CloudTrail
   - User Security:
     - MFA Delete: MFA (multi factor authentication) can be required in versioned buckets to delete objects
     - Pre-Signed URLs: URLs that are valid only for a limited time 
	 - (ex: premium video service for logged in users)

  
## S3 Websites
   - S3 can host static websites and have them accessible on the www
   - The website URL will be:
   - <bucket-name>.s3-website-<AWS-region>.amazonaws.com
   - OR
   - <bucket-name>.s3-website.<AWS-region>.amazonaws.com
   - If you get a 403 (Forbidden) error, make sure the bucket policy allows public reads!

  
## CORS - Explained
   - An origin is a scheme (protocol), host (domain) and port
   - E.g.: https://www.example.com (implied port is 443 for HTTPS, 80 for HTTP)
   - CORS means Cross-Origin Resource Sharing
   - Web Browser based mechanism to allow requests to other origins while visiting the main origin
   - Same origin: http://example.com/app1 & http://example.com/app2
   - Different origins: http://www.example.com & http://other.example.com
   - The requests won’t be fulfilled unless the other origin allows for the requests, using CORS Headers (ex: Access-Control-Allow-Origin)


  
## S3 CORS
   - If a client does a cross-origin request on our S3 bucket, we need to enable the correct CORS headers
   - You can allow for a specific origin or for * (all origins)
  
## Amazon S3 - Consistency Model
   - Read after write consistency for PUTS of new objects
     - As soon as a new object is written, we can retrieve it
     - ex: (PUT 200 => GET 200)
     - This is true, except if we did a GET before to see if the object existed
     - ex: (GET 404 => PUT 200 => GET 404) – eventually consistent
   - Eventual Consistency for DELETES and PUTS of existing objects
     - If we read an object after updating, we might get the older version
     - ex: (PUT 200 => PUT 200 => GET 200 (might be older version))
     - If we delete an object, we might still be able to retrieve it for a short time
     - ex: (DELETE 200 => GET 200)
   - Note: there’s no way to request “strong consistency”
	 
 
# Developing on AWS

## How to Develop
   - Developing and performing AWS tasks against AWS can be done in several ways
   - Using the AWS CLI on our local computer
   - Using the AWS CLI on our EC2 machines
   - Using the AWS SDK on our local computer
   - Using the AWS SDK on our EC2 machines
   - Using the AWS Instance Metadata Service for EC2
   - In this section, we’ll learn:
 
## AWS EC2 Instance Metadata
   - AWS EC2 Instance Metadata is powerful but one of the least known features to developers
   - It allows AWS EC2 instances to ”learn about themselves” without using an IAM Role for that purpose.
   - The URL is http://169.254.169.254/latest/meta-data
   - You can retrieve the IAM Role name from the metadata, but you CANNOT retrieve the IAM Policy.
   - Metadata = Info about the EC2 instance
   - Userdata = launch script of the EC2 instance
  
## AWS SDK Overview
   - What if you want to perform actions on AWS directly from your applications code ? (without using the CLI).
   - You can use an SDK (software development kit) !
   - Official SDKs are…
     - Java
     - .NET
     - Node.js
     - PHP
     - Python (named boto3 / botocore)
     - Go
     - Ruby
     - C++
   - We have to use the AWS SDK when coding against AWS Services such as DynamoDB
   - Fun fact… the AWS CLI uses the Python SDK (boto3)
   - Good to know: if you don’t specify or configure a default region, 
   - then us-east-1 will be chosen by default

  
## AWS SDK Credentials Security
   - It’s recommend to use the default credential provider chain
     - The default credential provider chain works seamlessly with:
     - AWS credentials at ~/.aws/credentials (only on our computers or on premise)
     - Instance Profile Credentials using IAM Roles (for EC2 machines, etc…)
     - Environment variables (AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY)
   - Overall, NEVER EVER STORE AWS CREDENTIALS IN YOUR CODE.
   - Best practice is for credentials to be inherited from mechanisms above, 
   - and 100% IAM Roles if working from within AWS Services

  
## Exponential Backoff
   - Any API that fails because of too many calls needs to be retried with Exponential Backoff
   - These apply to rate limited API
   - Retry mechanism included in SDK API calls

   
# Advanced S3, Glacier, Athena

  
## S3 MFA-Delete
   - MFA (multi factor authentication) forces user to generate a code on a device 
   - (usually a mobile phone or hardware) before doing important operations on S3
   - To use MFA-Delete, enable Versioning on the S3 bucket
   - You will need MFA to
     - permanently delete an object version
     - suspend versioning on the bucket
   - You won’t need MFA for
     - enabling versioning
     - listing deleted versions
   - Only the bucket owner (root account) can enable/disable MFA-Delete
   - MFA-Delete currently can only be enabled using the CLI

  
## S3 Default Encryption vs Bucket Policies
   - The old way to enable default encryption was to use a bucket policy and refuse any HTTP command without the proper headers:
   - The new way is to use the “default encryption” option in S3
   - Note: Bucket Policies are evaluated before “default encryption”

  
## S3 Access Logs
   - For audit purpose, you may want to log all access to S3 buckets
   - Any request made to S3, from any account, authorized or denied, will be logged into another S3 bucket
   - That data can be analyzed using data analysis tools…
   - Or Amazon Athena 
  
## S3 Access Logs: Warning
   - Do not set your logging bucket to be the monitored bucket
   - It will create a logging loop, and your bucket will grow in size exponentially
  
## S3 Replication (CRR & SRR)
   - Must enable versioning in source and destination
   - Cross Region Replication (CRR)
   - Same Region Replication (SRR)
   - Buckets can be in different accounts
   - Copying is asynchronous
   - Must give proper IAM permissions to S3
   - CRR - Use cases: compliance, lower latency access, replication across accounts
   - SRR – Use cases: log aggregation, live replication between production and test accounts

  
## S3 Replication – Notes
   - After activating, only new objects are replicated (not retroactive)
   - For DELETE operations:
   - If you delete without a version ID, it adds a delete marker, not replicated
   - If you delete with a version ID, it deletes in the source, not replicated
   - There is no “chaining” of replication
   - If bucket 1 has replication into bucket 2, which has replication into bucket 3
   - Then objects created in bucket 1 are not replicated to bucket 3

  
## S3 Pre-Signed URLs
   - Can generate pre-signed URLs using SDK or CLI
   - For downloads (easy, can use the CLI)
   - For uploads (harder, must use the SDK)
   - Valid for a default of 3600 seconds, can change timeout with --expires-in [TIME_BY_SECONDS] argument
   - Users given a pre-signed URL inherit the permissions of the person who generated the URL for GET / PUT
   - Examples :
   - Allow only logged-in users to download a premium video on your S3 bucket
   - Allow an ever changing list of users to download files by generating URLs dynamically
   - Allow temporarily a user to upload a file to a precise location in our bucket

  
## S3 Storage Classes
   - Amazon S3 Standard - General Purpose
   - Amazon S3 Standard-Infrequent Access (IA)
   - Amazon S3 One Zone-Infrequent Access
   - Amazon S3 Intelligent Tiering
   - Amazon Glacier
   - Amazon Glacier Deep Archive
   - Amazon S3 Reduced Redundancy Storage (deprecated - omitted)

  
## S3 Standard – General Purpose
   - High durability (99.999999999%) of objects across multiple AZ
   - If you store 10,000,000 objects with Amazon S3, 
   - you can on average expect to incur a loss of a single object once every 10,000 years
   - 99.99% Availability over a given year
   - Sustain 2 concurrent facility failures
   - Use Cases: Big Data analytics, mobile & gaming applications, content distribution…

  
## S3 Standard – Infrequent Access (IA)
   - Suitable for data that is less frequently accessed, but requires rapid access when needed
   - High durability (99.999999999%) of objects across multiple AZs
   - 99.9% Availability
   - Low cost compared to Amazon S3 Standard
   - Sustain 2 concurrent facility failures
   - Use Cases: As a data store for disaster recovery, backups…

  
## S3 One Zone - Infrequent Access (IA)
   - Same as IA but data is stored in a single AZ
   - High durability (99.999999999%) of objects in a single AZ; 
   - data lost when AZ is destroyed
   - 99.5% Availability
   - Low latency and high throughput performance
   - Supports SSL for data at transit and encryption at rest
   - Low cost compared to IA (by 20%)
   - Use Cases: Storing secondary backup copies of on-premise data, or storing data you can recreate

  
## S3 Intelligent Tiering
   - Same low latency and high throughput performance of S3 Standard
   - Small monthly monitoring and auto-tiering fee
   - Automatically moves objects between two access tiers based on changing access patterns
   - Designed for durability of 99.999999999% of objects across multiple Availability Zones
   - Resilient against events that impact an entire Availability Zone
   - Designed for 99.9% availability over a given year

  
## Amazon Glacier    
   - Low cost object storage meant for archiving / backup    
   - Data is retained for the longer term (10s of years)    
   - Alternative to on-premise magnetic tape storage    
   - Average annual durability is 99.999999999%    
   - Cost per storage per month ($0.004 / GB) + retrieval cost     
   - Each item in Glacier is called “Archive” (up to 40TB)    
   - Archives are stored in ”Vaults”

  
## Amazon Glacier & Glacier Deep Archive
   - Amazon Glacier – 3 retrieval options:
   - Expedited (1 to 5 minutes)
   - Standard (3 to 5 hours)
   - Bulk (5 to 12 hours)
   - Minimum storage duration of 90 days
   - Amazon Glacier Deep Archive – for long term storage – cheaper:
   - Standard (12 hours)
   - Bulk (48 hours)
   - Minimum storage duration of 180 days

## S3 – Moving between storage classes
   - You can transition objects between storage classes
   - For infrequently accessed object, move them to STANDARD_IA
   - For archive objects you don’t need in real -time, GLACIER or DEEP_ARCHIVE
   - Moving objects can be automated using a lifecycle configuration

  
## S3 Lifecycle Rules
   - Transition actions: 
     - It defines when objects are transitioned to another storage class.
     - Move objects to Standard IA class 60 days after creation
     - Move to Glacier for archiving after 6 months
   - Expiration actions: 
     - configure objects to expire (delete) after some time
     - Access log files can be set to delete after a 365 days
     - Can be used to delete old versions of files (if versioning is enabled)
     - Can be used to delete incomplete multi-part uploads
   - Rules can be created for a certain prefix (ex - s3://mybucket/mp3/*)
   - Rules can be created for certain objects tags (ex - Department: Finance)

  
## S3 Lifecycle Rules – Scenario 1
   - Your application on EC2 creates images thumbnails after profile photos are uploaded to Amazon S3. 
   - These thumbnails can be easily recreated, and only need to be kept for 45 days. 
   - The source images should be able to be immediately retrieved for these 45 days, and afterwards, the user can wait up to 6 hours. 
   - How would you design this?
     - S3 source images can be on STANDARD, with a lifecycle configuration to transition them to GLACIER after 45 days.
     - S3 thumbnails can be on ONEZONE_IA, with a lifecycle configuration to expire them (delete them) after 45 days. 

  
## S3 Lifecycle Rules – Scenario 2
   - A rule in your company states that you should be able to recover your deleted S3 objects immediately for 15 days, 
   - although this may happen rarely. After this time, and for up to 365 days, 
   - deleted objects should be recoverable within 48 hours.
   - You need to enable S3 versioning in order to have object versions, 
   - so that “deleted objects” are in fact hidden by a “delete marker” and can be recovered
     - You can transition these “noncurrent versions” of the object to S3_IA
     - You can transition afterwards these “noncurrent versions” to DEEP_ARCHIVE

  
## S3 – Baseline Performance
   - Amazon S3 automatically scales to high request rates, latency 100-200 ms
   - Your application can achieve at least 3,500 PUT/COPY/POST/DELETE 
   - and 5,500 GET/HEAD requests per second per prefix in a bucket.
   - There are no limits to the number of prefixes in a bucket.
   - Example (object path => prefix):
     - bucket/folder1/sub1/file => /folder1/sub1/
     - bucket/folder1/sub2/file => /folder1/sub2/
     - bucket/1/file => /1/
     - bucket/2/file => /2/
   - If you spread reads across all four prefixes evenly, you can achieve 22,000 requests per second for GET and HEAD

  
## S3 – KMS Limitation
   - If you use SSE-KMS, you may be impacted by the KMS limits
   - When you upload, it calls the GenerateDataKey KMS API
   - When you download, it calls the Decrypt KMS API
   - Count towards the KMS quota per second (5500, 10000, 30000 req/s based on region)
   - As of today, you cannot request a quota increase for KMS
  
## S3 Performance
   - Multi-Part upload:
     - recommended for files > 100MB, must use for files > 5GB
     - Can help parallelize uploads (speed up transfers)
   - S3 Transfer Acceleration (upload only)
     - Increase transfer speed by transferring file to an AWS edge location which will forward the data to the S3 bucket in the target region
     - Compatible with multi-part upload
  
## S3 Performance – S3 Byte-Range Fetches
   - Parallelize GETs by requesting specific byte ranges
   - Better resilience in case of failures
   - Can be used to speed up downloads
   - Can be used to retrieve only partial data (for example the head of a file)
  
## S3 Select & Glacier Select
   - Retrieve less data using SQL by performing server side filtering
   - Can filter by rows & columns (simple SQL statements)
   - Less network transfer, less CPU cost client-side
  
## S3 Event Notifications
   - S3:ObjectCreated, S3:ObjectRemoved, S3:ObjectRestore, S3:Replication…
   - Object name filtering possible (*.jpg)
   - Use case: generate thumbnails of images uploaded to S3
   - Can create as many “S3 events” as desired
   - S3 event notifications typically deliver events in seconds but can sometimes take a minute or longer
   - If two writes are made to a single non-versioned object at the same time, it is possible that only a single event notification will be sent
   - If you want to ensure that an event notification is sent for every successful write, you can enable versioning on your bucket.
   - Lambda 
   - SQS
   - SNS

  
## AWS Athena
   - Serverless service to perform analytics directly against S3 files
   - Uses SQL language to query the files
   - Has a JDBC / ODBC driver
   - Charged per query and amount of data scanned
   - Supports CSV, JSON, ORC, Avro, and Parquet (built on Presto)
   - Use cases: Business intelligence / analytics / reporting, analyze & query VPC Flow Logs, ELB Logs, CloudTrail trails, etc...
   - Exam Tip: Analyze data directly on S3 => use Athena

  
## S3 Object Lock & Glacier Vault Lock
   - S3 Object Lock
     - Adopt a WORM (Write Once Read Many) model
     - Block an object version deletion for a specified amount of time
   - Glacier Vault Lock
     - Adopt a WORM (Write Once Read Many) model
     - Lock the policy for future edits (can no longer be changed)
     - Helpful for compliance and data retention
	 

# CloudFront & AWS Global Accelerator


## AWS CloudFront
   - Content Delivery Network (CDN)
   - Improves read performance, content is cached at the edge
   - 216 Point of Presence globally (edgelocations)
   - DDoS protection, integration with Shield, AWS Web Application Firewall
   - Can expose external HTTPS and can talk to internal HTTPS backends
  
## CloudFront – Origins
   - S3 bucket    
     - For distributing files and caching them at the edge    
	 - Enhanced security with CloudFront Origin Access Identity (OAI)    
	 - CloudFront can be used as an ingress (to upload files to S3)    
   - Custom Origin (HTTP)    
     - Application Load Balancer    
	 - EC2 instance    
	 - S3 website (must first enable the bucket as a static S3 website)    
	 - Any HTTP backend you want
  
## CloudFront Geo Restriction
   - You can restrict who can access your distribution
   - Whitelist: 
     - Allow your users to access your content only if they're in one of the countries on a list of approved countries.
   - Blacklist: 
     - Prevent your users from accessing your content if they're in one of the countries on a blacklist of banned countries.
   - The “country” is determined using a 3rd party Geo-IP database
   - Use case: Copyright Laws to control access to content

  
## CloudFront vs S3 Cross Region Replication
   - CloudFront:
     - Global Edge network
     - Files are cached for a TTL (maybe a day)
     - Great for static content that must be available everywhere
   - S3 Cross Region Replication:
     - Must be setup for each region you want replication to happen
     - Files are updated in near real-time
     - Read only
     - Great for dynamic content that needs to be available at low-latency in few regions


  
## CloudFront Signed URL / Signed Cookies
   - You want to distribute paid shared content to premium users over the world
   - We can use CloudFront Signed URL / Cookie. We attach a policy with:
     - Includes URL expiration
     - Includes IP ranges to access the data from
     - Trusted signers (which AWS accounts can create signed URLs)
   - How long should the URL be valid for?
     - Shared content (movie, music): make it short (a few minutes)
     - Private content (private to the user): you can make it last for years
   - Signed URL = access to individual files (one signed URL per file)
   - Signed Cookies = access to multiple files (one signed cookie for many files)

  
## CloudFront Signed URL vs S3 Pre-Signed URL
   - CloudFront Signed URL:
     - Allow access to a path, no matter the origin
     - Account wide key-pair, only the root can manage it
     - Can filter by IP, path, date, expiration
     - Can leverage caching features
   - S3 Pre-Signed URL:
     - Issue a request as the person who pre-signed the URL
     - Uses the IAM key of the signing IAM principal
     - Limited lifetime

## Global users for our application
   - You have deployed an application and have global users who want to access it directly.
   - They go over the public internet, which can add a lot of latency due to many hops
   - We wish to go as fast as possible through AWS network to minimize latency
  
## Unicast IP vs Anycast IP    
   - Unicast IP: one server holds one IP address
   - Anycast IP: all servers hold the same IP address and the client is routed to the nearest one

  
## AWS Global Accelerator
   - Leverage the AWS internal network to route to your application
   - 2 Anycast IP are created for your application
   - The Anycast IP send traffic directly to Edge Locations
   - The Edge locations send the traffic to your application
   - Works with Elastic IP, EC2 instances, ALB, NLB, public or private
   - Consistent Performance
     - Intelligent routing to lowest latency and fast regional failover
     - No issue with client cache (because the IP doesn’t change)
     - Internal AWS network
   - Health Checks
     - Global Accelerator performs a health check of your applications
     - Helps make your application global (failover less than 1 minute for unhealthy)
     - Great for disaster recovery (thanks to the health checks)
   - Security
     - only 2 external IP need to be whitelisted
     - DDoS protection thanks to AWS Shield

  
## AWS Global Accelerator vs CloudFront
   - They both use the AWS global network and its edge locations around the world
   - Both services integrate with AWS Shield for DDoS protection.
   - CloudFront
     - Improves performance for both cacheable content (such as images and videos)
     - Dynamic content (such as API acceleration and dynamic site delivery)
     - Content is served at the edge
   - Global Accelerator
     - Improves performance for a wide range of applications over TCP or UDP
     - Proxying packets at the edge to applications running in one or more AWS Regions.
     - Good fit for non-HTTP use cases, such as gaming (UDP), IoT (MQTT), or Voice over IP
     - Good for HTTP use cases that require static IP addresses
     - Good for HTTP use cases that required deterministic, fast regional failover	 

# AWS Storage
  
## Snowball    
   - Physical data transport solution that helps moving TBs or PBs of data in or out of AWS
   - Alternative to moving data over the network (and paying network fees)
   - Secure, tamper resistant, uses KMS 256 bit encryption
   - Tracking using SNS and text messages. E -ink shipping label
   - Pay per data transfer job    
   - Use cases: large data cloud migrations, DC decommission, disaster recovery
   - If it takes more than a week to transfer over the network, use Snowball devices!

  
## Snowball Process
   - 1. Request snowball devices from the AWS console for delivery
   - 2. Install the snowball client on your servers
   - 3. Connect the snowball to your servers and copy files using the client
   - 4. Ship back the device when you’re done (goes to the right AWS facility)
   - 5. Data will be loaded into an S3 bucket
   - 6. Snowball is completely wiped
   - 7. Tracking is done using SNS, text messages and the AWS console
  
## Snowball Edge    
   - Snowball Edges add computational capability to the device
   - 100 TB capacity with either:    
     - Storage optimized – 24 vCPU    
	 - Compute optimized – 52 vCPU & optional GPU    
   - Supports a custom EC2 AMI so you can perform processing on the go
   - Supports custom Lambda functions    
   - Very useful to pre-process the data while moving    
   - Use case: data migration, image collation, IoT capture, machine learning

## AWS Snowmobile
   - Transfer exabytes of data (1 EB = 1,000 PB = 1,000,000 TBs)
   - Each Snowmobile has 100 PB of capacity (use multiple in parallel)
   - Better than Snowball if you transfer more than 10 PB

## Snowball into Glacier
   - Snowball cannot import to Glacier directly
   - You have to use Amazon S3 first, and an S3 lifecycle policy Snowball Amazon S3 Amazon Glacier import S3 lifecycle policy

  
## Hybrid Cloud for Storage
   - AWS is pushing for ”hybrid cloud”
     - Part of your infrastructure is on the cloud
     - Part of your infrastructure is on-premise
   - This can be due to
     - Long cloud migrations
     - Security requirements
     - Compliance requirements
     - IT strategy
     - BLOCK
       - EBS,  EC2 Instance Store
     - FILE
       - Amazon EFS
     - OBJECT
       -S3,  Glacier

  
## AWS Storage Gateway
   - Bridge between on-premise data and cloud data in S3
   - Use cases: disaster recovery, backup & restore, tiered storage
   - 3 types of Storage Gateway:
     - File Gateway
     - Volume Gateway
     - Tape Gateway
   
## File Gateway
   - Configured S3 buckets are accessible using the NFS and SMB protocol
   - Supports S3 standard, S3 IA, S3 One Zone IA
   - Bucket access using IAM roles for each File Gateway
   - Most recently used data is cached in the file gateway
   - Can be mounted on many servers

## Volume Gateway
   - Block storage using iSCSI protocol backed by S3
   - Backed by EBS snapshots which can help restore on-premise volumes!
   - Cached volumes: low latency access to most recent data
   - Stored volumes: entire dataset is on premise, scheduled backups to S3
  
## Tape Gateway
   - Some companies have backup processes using physical tapes (!)
   - With Tape Gateway, companies use the same processes but in the cloud
   - Virtual Tape Library (VTL) backed by Amazon S3 and Glacier
   - Back up data using existing tape-based processes (and iSCSI interface)
   - Works with leading backup software vendors 
  
## AWS Storage Gateway Summary
   - Exam tip: Read the question well, it will hint at which gateway to use
   - On premise data to the cloud => Storage Gateway
   - File access / NFS => File Gateway (backed by S3)
   - Volumes / Block Storage / iSCSI => Volume gateway (backed by S3 with EBS snapshots)
   - VTL Tape solution / Backup with iSCSI = > Tape Gateway (backed by S3 and Glacier)

  
## Amazon FSx for Windows (File Server)
   - EFS is a shared POSIX system for Linux systems.
   - FSx for Windows is a fully managed Windows file system share drive
   - Supports SMB protocol & Windows NTFS
   - Microsoft Active Directory integration, ACLs, user quotas
   - Built on SSD, scale up to 10s of GB/s, millions of IOPS, 100s PB of data
   - Can be accessed from your on-premise infrastructure
   - Can be configured to be Multi-AZ (high availability)
   - Data is backed-up daily to S3
  
## Amazon FSx for Lustre
   - Lustre is a type of parallel distributed file system, for large-scale computing
   - The name Lustre is derived from “Linux” and “cluster”
   - Machine Learning, High Performance Computing (HPC)
   - Video Processing, Financial Modeling, Electronic Design Automation
   - Scales up to 100s GB/s, millions of IOPS, sub-ms latencies
   - Seamless integration with S3
   - Can “read S3” as a file system (through FSx)
   - Can write the output of the computations back to S3 (through FSx)
   - Can be used from on-premise servers
  
## Storage Comparison
   - S3: Object Storage
   - Glacier: Object Archival
   - EFS: Network File System for Linux instances, POSIX filesystem
   - FSx for Windows: Network File System for Windows servers
   - FSx for Lustre: High Performance Computing Linux file system
   - EBS volumes: Network storage for one EC2 instance at a time
   - Instance Storage: Physical storage for your EC2 instance (high IOPS)
   - Storage Gateway: File Gateway, Volume Gateway (cache & stored), Tape Gateway
   - Snowball / Snowmobile: to move large amount of data to the cloud, physically
   - Database: for specific workloads, usually with indexing and querying

# AWS Integration & Messaging

## AWS SQS – Standard Queue
   - Oldest offering (over 10 years old)
   - Fully managed
   - Scales from 1 message per second to 10,000s per second
   - Default retention of messages: 4 days, maximum of 14 days
   - No limit to how many messages can be in the queue
   - Low latency (<10 ms on publish and receive)
   - Horizontal scaling in terms of number of consumers
   - Can have duplicate messages (at least once delivery, occasionally)
   - Can have out of order messages (best effort ordering)
   - Limitation of 256KB per message sent

  
## AWS SQS – Delay Queue
   - Delay a message (consumers don’t see it immediately) up to 15 minutes
   - Default is 0 seconds (message is available right away)
   - Can set a default at queue level
   - Can override the default using the DelaySeconds parameter Producer Consumer Send messages Poll messages

## Attributes SQS – Producing Messages
   - Define Body    - Add message attributes (metadata – optional)
   - Provide Delay Delivery (optional)
   - Get back    
     - Message identifier
	 - MD5 hash of the body
  
## SQS – Consuming Messages
   - Consumers…
   - Poll SQS for messages (receive up to 10 messages at a time)
   - Process the message within the visibility timeout
   - Delete the message using the message ID & receipt handle

## SQS –Visibility timeout
   - When a consumer polls a message from a queue, the message is “invisible” to other consumers for a defined period… the Visibility Timeout:
     - Set between 0 seconds and 12 hours (default 30 seconds)
     - If too high (15 minutes) and consumer fails to process the message, you must wait a long time before processing the message again
     - If too low (30 seconds) and consumer needs time to process the message (2 minutes), another consumer will receive the message and the message will be processed more than once
   - ChangeMessageVisibility API to change the visibility while processing a message
   - DeleteMessage API to tell SQS the message was successfully processed

## AWS SQS – Dead Letter Queue
   - If a consumer fails to process a message within the Visibility Timeout… the message goes back to the queue!
   - We can set a threshold of how many times a message can go back to the queue 
   – it’s called a “redrive policy”
   - After the threshold is exceeded, the message goes into a dead letter queue (DLQ)
   - We have to create a DLQ first and then designate it dead letter queue
   - Make sure to process the messages in the DLQ before they expire! 
  
## AWS SQS - Long Polling
   - When a consumer requests message from the queue, it can optionally “wait” for messages to arrive if there are none in the queue
   - This is called Long Polling 
   - LongPolling decreases the number of API calls made to SQS while increasing the efficiency and latency of your application.
   - The wait time can be between 1 sec to 20 sec (20 sec preferable)
   - Long Polling is preferable to Short Polling
   - Long polling can be enabled at the queue level or at the API level using WaitTimeSeconds
  
## AWS SQS – FIFO Queue
   - Newer offering (First In - First out)
   – not available in all regions!
   - Name of the queue must end in .fifo    
   - Lower throughput (up to 3,000 per second with batching, 300/s without)
   - Messages are processed in order by the consumer    
   - Messages are sent exactly once    
   - No per message delay (only per queue delay)    
   - Ability to do content based de-duplication    
   - 5-minute interval de-duplication using “Duplication ID”    
   - Message Groups:    
     - Possibility to group messages for FIFO ordering using “Message GroupID”
     - Only one worker can be assigned per message group so that messages are processed in order
     - Message group is just an extra tag on the message!

  
## AWS SNS
   - What if you want to send one message to many receivers?
   - The “event producer” only sends message to one SNS topic
   - As many “event receivers” (subscriptions) as we want to listen to the SNS topic notifications
   - Each subscriber to the topic will get all the messages (note: new feature to filter messages)
   - Up to 10,000,000 subscriptions per topic
   - 100,000 topics limit
   - Subscribers can be:
     - SQS
     - HTTP / HTTPS (with delivery retries – how many times)
     - Lambda
     - Emails
     - SMS messages
     - Mobile Notifications
  
## SNS integrates with a lot of Amazon Products
   - Some services can send data directly to SNS for notifications
   - CloudWatch (for alarms)
   - Auto Scaling Groups notifications
   - Amazon S3 (on bucket events)
   - CloudFormation (upon state changes => failed to build, etc)
   - Etc…
  
## AWS SNS – How to publish
   - Topic Publish (within your AWS Server – using the SDK)
     - Create a topic    
	 - Create a subscription (or many)    
	 - Publish to the topic
   - Direct Publish (for mobile apps SDK)    
     - Create a platform application    
	 - Create a platform endpoint    
	 - Publish to the platform endpoint    
	 - Works with Google GCM, Apple APNS, Amazon ADM…
  
## SNS + SQS: Fan Out
   - Push once in SNS, receive in many SQS
   - Fully decoupled
   - No data loss
   - Ability to add receivers of data later
   - SQS allows for delayed processing
   - SQS allows for retries of work
   - May have many workers on one queue and one worker on the other queue

  
## AWS Kinesis Overview
   - Kinesis is a managed alternative to Apache Kafka
   - Great for application logs, metrics, IoT, clickstreams
   - Great for “real-time” big data
   - Great for streaming processing frameworks (Spark, NiFi, etc…)
   - Data is automatically replicated to 3 AZ
   - 3 components
     - Kinesis Streams: low latency streaming ingest at scale
     - Kinesis Analytics: perform real-time analytics on streams using SQL
     - Kinesis Firehose: load streams into S3, Redshift, ElasticSearch… 
   - Streams are divided in ordered Shards / Partitions
   - Data retention is 1 day by default, can go up to 7 days
   - Ability to reprocess / replay data
   - Multiple applications can consume the same stream
   - Real-time processing with scale of throughput
   - Once data is inserted in Kinesis, it can’t be deleted (immutability)

## Kinesis Streams Shards
   - One stream is made of many different shards
   - 1MB/s or 1000 messages/s at write PER SHARD
   - 2MB/s at read PER SHARD
   - Billing is per shard provisioned, can have as many shards as you want
   - Batching available or per message calls.
   - The number of shards can evolve over time (reshard / merge)
   - Records are ordered per shard
   - Choose a partition key that is highly distributed (helps prevent “hot partition”)
     - user_id if many users
     - Not country_id if 90% of the users are in one country

## AWS Kinesis API – Put records
   - PutRecord API + Partition key that gets hashed
   - The same key goes to the same partition (helps with ordering for a specific key)
   - Messages sent get a “sequence number” 
   - Choose a partition key that is highly distributed (helps prevent “hot partition”)
     - user_id if many users
     - Not country_id if 90% of the users are in one country
   - Use Batching with PutRecords to reduce costs and increase throughput
   - ProvisionedThroughputExceeded if we go over the limits
   - Can use CLI, AWS SDK, or producer libraries

## AWS Kinesis API – Exceptions
   - ProvisionedThroughputExceeded Exceptions
     - Happens when sending more data (exceeding MB/s or TPS for any shard)
     - Make sure you don’t have a hot shard (such as your partition key is bad and too much data goes to that partition)
   - Solution:
     - Retries with backoff
     - Increase shards (scaling)
     - Ensure your partition key is a good one

  
## AWS Kinesis API – Consumers
   - Can use a normal consumer (CLI, SDK, etc…)
   - Can use Kinesis Client Library (in Java, Node, Python, Ruby, .Net)
   - KCL uses DynamoDB to checkpoint offsets
   - KCL uses DynamoDB to track other workers and share the work amongst shards
  
## Kinesis Security    
   - Control access / authorization using IAM policies    
   - Encryption in flight using HTTPS endpoints    
   - Encryption at rest using KMS    
   - Possibility to encrypt / decrypt data client side (harder)    
   - VPC Endpoints available for Kinesis to access within VPC

  
## AWS Kinesis Data Firehose
   - Fully Managed Service, no administration, automatic scaling, serverless
   - Load data into Redshift / Amazon S3 / ElasticSearch / Splunk
   - Near Real Time
     - 60 seconds latency minimum for non full batches
     - Or minimum 32 MB of data at a time
   - Supports many data formats, conversions, transformations, compression
   - Pay for the amount of data going through Firehose
  
## Kinesis Data Streams vs Firehose    
   - Streams    
     - Going to write custom code (producer / consumer)    
     - Real time (~200 ms)    
     - Must manage scaling (shard splitting / merging)    
	 - Data Storage for 1 to 7 days, replay capability, multi consumers    
   - Firehose    
     - Fully managed, send to S3, Splunk, Redshift, ElasticSearch    
	 - Serverless data transformations with Lambda    
	 - Near real time (lowest buffer time is 1 minute)    
	 - Automated Scaling    
	 - No data storage

## Kinesis Data Analytics    
   - Perform real-time analytics on Kinesis Streams using SQL    
   - Kinesis Data Analytics:    
     - Auto Scaling    
     - Managed: no servers to provision    
     - Continuous: real time    
   - Pay for actual consumption rate    
   - Can create streams out of the real-time queries

  
## Ordering data into Kinesis
   - Imagine you have 100 trucks (truck_1, truck_2, … truck_100) on the road sending their GPS positions regularly into AWS.
   - You want to consume the data in order for each truck, so that you can track their movement accurately.
   - How should you send that data into Kinesis?
   - Answer: send using a “Partition Key” value of the “truck_id”
   - The same key will always go to the same shard
   
## Ordering data into SQS
   - For SQS standard, there is no ordering.
   - For SQS FIFO, if you don’t use a Group ID, messages are consumed in the order they are sent, with only one consumer
   - You want to scale the number of consumers, but you want messages to be “grouped” when they are related to each other
   - Then you use a Group ID (similar to Partition Key in Kinesis)
   - https://aws.amazon.com/blogs/compute/solving-complex-ordering-challenges-with-amazon-sqs-fifo-queues/
  
## Kinesis vs SQS ordering
   - Let’s assume 100 trucks, 5 kinesis shards, 1 SQS FIFO
   - Kinesis Data Streams:
     - On average you’ll have 20 trucks per shard
     - Trucks will have their data ordered within each shard
     - The maximum amount of consumers in parallel we can have is 5
     - Can receive up to 5 MB/s of data
   - SQS FIFO
     - You only have one SQS FIFO queue
     - You will have 100 Group ID
     - You can have up to 100 Consumers (due to the 100 Group ID)
     - You have up to 300 messages per second (or 3000 if using batching)
  
## SQS vs SNS vs Kinesis
   - SQS:
     - Consumer “pull data”
     - Data is deleted after being consumed
     - Can have as many workers (consumers) as we want
     - No need to provision throughput
     - No ordering guarantee (except FIFO queues)
     - Individual message delay capability
   - SNS:
     - Push data to many subscribers
     - Up to 10,000,000 subscribers
     -  Data is not persisted (lost if not delivered)
     - Pub/Sub
     - Up to 100,000 topics 
     - No need to provision throughput
     - Integrates with SQS for fan- out architecture pattern
   - Kinesis:
     - Consumers “pull data”
     - As many consumers as we want
     - Possibility to replay data
     - Meant for real-time big data, analytics and ETL
     - Ordering at the shard level
     - Data expires after X days
     - Must provision throughput
  
## Amazon MQ
   - SQS, SNS are “cloud-native” services, and they’re using proprietary protocols from AWS.
   - Traditional applications running from on-premise may use open protocols such as: MQTT, AMQP, STOMP, Openwire, WSS
   - When migrating to the cloud, instead of re-engineering the application to use SQS and SNS, we can use Amazon MQ
   - Amazon MQ = managed Apache ActiveMQ
   - Amazon MQ doesn’t “scale” as much as SQS / SNS
   - Amazon MQ runs on a dedicated machine, can run in HA with failover
   - Amazon MQ has both queue feature (~SQS) and topic features (~SNS)

# Serverless 
  
## What’s serverless?
   - Serverless is a new paradigm in which the developers don’t have to manage servers anymore…
   - They just deploy code
   - They just deploy… functions !
   - Initially... Serverless == FaaS (Function as a Service)
   - Serverless was pioneered by AWS Lambda but now also includes anything that’s managed: “databases, messaging, storage, etc.”
   - Serverless does not mean there are no servers… it means you just don’t manage / provision / see them

## Serverless in AWS    
   - AWS Lambda    
   - DynamoDB    
   - AWS Cognito    
   - AWS API Gateway    
   - Amazon S3    
   - AWS SNS & SQS    
   - AWS Kinesis Data Firehose    
   - Aurora Serverless    
   - Step Functions    
   - Fargate
  
## AWS Lambda
   - Easy Pricing:
   - Pay per request and compute time
   - Free tier of 1,000,000 AWS Lambda requests and 400,000 GBs of compute time
   - Integrated with the whole AWS suite of services
   - Integrated with many programming languages
   - Easy monitoring through AWS CloudWatch
   - Easy to get more resources per functions (up to 3GB of RAM!)
   - Increasing RAM will also improve CPU and network!

  
## AWS Lambda language support    
   - Node.js (JavaScript)   
   - Python    
   - Java (Java 8 compatible)    
   - C# (.NET Core)    
   - Golang    
   - C# / Powershell    
   - Ruby    
   - Custom Runtime API (community supported, example Rust)    
   - Important: Docker is not for AWS Lambda, it’s for ECS / Fargate

  
## AWS Lambda Integrations
   - CloudWatch Logs SNS SQS Cognito API Gateway Kinesis DynamoDB S3 CloudFront CloudWatch Events EventBridge
  
## AWS Lambda Pricing: example    
   - Pay per calls :
     - First 1,000,000 requests are free    
	 - $0.20 per 1 million requests thereafter ($0.0000002 per request)
   - Pay per duration: (in increment of 100ms)    
     - 400,000 GB-seconds of compute time per month if FREE    
     - == 400,000 seconds if function is 1GB RAM    
     - == 3,200,000 seconds if function is 128 MB RAM    
     - After that $1.00 for 600,000 GB-seconds    
     - It is usually very cheap to run AWS Lambda so it’s very popular

  
## AWS Lambda Limits to Know - per region
   - Execution:
     - Memory allocation: 128 MB – 3008 MB (64 MB increments)
     - Maximum execution time: 900 seconds (15 minutes)
     - Environment variables (4 KB)
     - Disk capacity in the “function container” (in /tmp): 512 MB
     - Concurrency executions: 1000 (can be increased)
   - Deployment:
     - Lambda function deployment size (compressed .zip): 50 MB
     - Size of uncompressed deployment (code + dependencies): 250 MB
     - Can use the /tmp directory to load other files at startup
     - Size of environment variables: 4 KB
  
## Lambda@Edge
   - You have deployed a CDN using CloudFront
   - What if you wanted to run a global AWS Lambda alongside?
   - Or how to implement request filtering before reaching your application?
   - For this, you can use Lambda@Edge: deploy Lambda functions alongside your CloudFront CDN
     - Build more responsive applications
     - You don’t manage servers, Lambda is deployed globally
     - Customize the CDN content
     - Pay only for what you use
   - You can use Lambda to change CloudFront requests and responses:
     - After CloudFront receives a request from a viewer (viewer request)
     - Before CloudFront forwards the request to the origin (origin request)
     - After CloudFront receives the response from the origin (origin response)
     - Before CloudFront forwards the response to the viewer (viewer response)
   - You can also generate responses to viewers without ever sending the request to the origin.
  
## Lambda@Edge: Use Cases    
   - Website Security and Privacy    
   - Dynamic Web Application at the Edge    
   - Search Engine Optimization (SEO)    
   - Intelligently Route Across Origins and Data Centers    
   - Bot Mitigation at the Edge    
   - Real-time Image Transformation    
   - A/B Testing    
   - User Authentication and Authorization    
   - User Prioritization    
   - User Tracking and Analytics
  
## DynamoDB
   - Fully Managed, Highly available with replication across 3 AZ
   - NoSQL database - not a relational database
   - Scales to massive workloads, distributed database
   - Millions of requests per seconds, trillions of row, 100s of TB of storage
   - Fast and consistent in performance (low latency on retrieval)
   - Integrated with IAM for security, authorization and administration
   - Enables event driven programming with DynamoDB Streams
   - Low cost and auto scaling capabilities
  
## DynamoDB - Basics
   - DynamoDB is made of tables
   - Each table has a primary key (must be decided at creation time)
   - Each table can have an infinite number of items (= rows)
   - Each item has attributes (can be added over time – can be null)
   - Maximum size of a item is 400KB
   - Data types supported are:
     - Scalar Types: String, Number, Binary, Boolean, Null
     - Document Types: List, Map
     - Set Types: String Set, Number Set, Binary Set
  
## DynamoDB – Provisioned Throughput
   - Table must have provisioned read and write capacity units
   - Read Capacity Units (RCU): throughput for reads ($0.00013 per RCU)
     - 1 RCU = 1 strongly consistent read of 4 KB per second
     - 1 RCU = 2 eventually consistent read of 4 KB per second
   - Write Capacity Units (WCU): throughput for writes ($0.00065 per WCU)
     - 1 WCU = 1 write of 1 KB per second
   - Option to setup auto-scaling of throughput to meet demand
   - Throughput can be exceeded temporarily using “burst credit”
   - If burst credit are empty, you’ll get a “ProvisionedThroughputException”.
   - It’s then advised to do an exponential back-off retry

## DynamoDB - DAX
   - DAX = DynamoDB Accelerator    
   - Seamless cache for DynamoDB, no application re- write
   - Writes go through DAX to DynamoDB    
   - Micro second latency for cached reads & queries    
   - Solves the Hot Key problem (too many reads)    
   - 5 minutes TTL for cache by default    
   - Up to 10 nodes in the cluster    
   - Multi AZ (3 nodes minimum recommended for production)
   - Secure (Encryption at rest with KMS, VPC, IAM, CloudTrail…)
  
## DynamoDB Streams
   - Changes in DynamoDB (Create, Update, Delete) can end up in a DynamoDB Stream
   - This stream can be read by AWS Lambda, and we can then do:
     - React to changes in real time (welcome email to new users)
     - Analytics
     - Create derivative tables / views
     - Insert into ElasticSearch
   - Could implement cross region replication using Streams
   - Stream has 24 hours of data retention
  
## DynamoDB - New Features
   - Transactions (new from Nov 2018)
     - All or nothing type of operations
     - Coordinated Insert, Update & Delete across multiple tables
     - Include up to 10 unique items or up to 4 MB of data
   - On Demand (new from Nov 2018)
     - No capacity planning needed (WCU / RCU) – scales automatically
     - 2.5x more expensive than provisioned capacity (use with care)
     - Helpful when spikes are un-predictable or the application is very low throughput
  
## DynamoDB – Security & Other Features
   - Security:
     - VPC Endpoints available to access DynamoDB without internet
     - Access fully controlled by IAM
     - Encryption at rest using KMS
     - Encryption in transit using SSL / TLS
   - Backup and Restore feature available
     - Point in time restore like RDS
     - No performance impact
   - Global Tables
     - Multi region, fully replicated, high performance
   - Amazon DMS can be used to migrate to DynamoDB (from Mongo, Oracle, MySQL, S3, etc…)
   - You can launch a local DynamoDB on your computer for development purposes
  
## DynamoDB – Other features
   - Global Tables: (cross region replication)
     - Active Active replication, many regions
     - Must enable DynamoDB Streams
     - Useful for low latency, DR purposes
   - Capacity planning:
     - Planned capacity: provision WCU & RCU, can enable auto scaling
     - On-demand capacity: get unlimited WCU & RCU, no throttle, more expensive
	 
## AWS API Gateway    
   - AWS Lambda + API Gateway: No infrastructure to manage    
   - Support for the WebSocket Protocol    
   - Handle API versioning (v1, v2…)    
   - Handle different environments (dev, test, prod…)    
   - Handle security (Authentication and Authorization)    
   - Create API keys, handle request throttling    
   - Swagger / Open API import to quickly define APIs    
   - Transform and validate requests and responses    
   - Generate SDK and API specifications    
   - Cache API responses
  
## API Gateway – Integrations High Level
   - Lambda Function
     - Invoke Lambda function
     - Easy way to expose REST API backed by AWS Lambda
   - HTTP
     - Expose HTTP endpoints in the backend
     - Example: internal HTTP API on premise, Application Load Balancer…
     - Why? Add rate limiting, caching, user authentications, API keys, etc…
   - AWS Service
     - Expose any AWS API through the API Gateway?
     - Example: start an AWS Step Function workflow, post a message to SQS
     - Why? Add authentication, deploy publicly, rate control… 
  
## API Gateway - Endpoint Types
   - Edge-Optimized (default): For global clients
     - Requests are routed through the CloudFront Edge locations (improves latency)
     - The API Gateway still lives in only one region
   - Regional:
     - For clients within the same region
     - Could manually combine with CloudFront (more control over the caching strategies and the distribution)
   - Private:
     - Can only be accessed from your VPC using an interface VPC endpoint (ENI)
     - Use a resource policy to define access
  
## API Gateway – Security IAM Permissions
   - Create an IAM policy authorization and attach to User / Role
   - API Gateway verifies IAM permissions passed by the calling application
   - Good to provide access within your own infrastructure
   - Leverages “Sig v4” capability where IAM credential are in headers

## API Gateway – Security Lambda Authorizer (formerly Custom Authorizers)
   - Uses AWS Lambda to validate the token in header being passed
   - Option to cache result of authentication
   - Helps to use OAuth / SAML / 3rd party type of authentication
   - Lambda must return an IAM policy for the user
  
## API Gateway – Security Cognito User Pools    
   - Cognito fully manages user lifecycle    
   - API gateway verifies identity automatically from AWS Cognito    
   - No custom implementation required    
   - Cognito only helps with authentication, not authorization
  
API Gateway – Security – Summary
   - IAM:
     - Great for users / roles already within your AWS account
     - Handle authentication + authorization
     - Leverages Sig v4
   - Custom Authorizer:
     - Great for 3rd party tokens
     - Very flexible in terms of what IAM policy is returned
     - Handle Authentication + Authorization
     - Pay per Lambda invocation
   - Cognito User Pool:
     - You manage your own user pool (can be backed by Facebook, Google login etc…)
     - No need to write any custom code
     - Must implement authorization in the backend 
  
## AWS Cognito
   - We want to give our users an identity so that they can interact with our application.
   - Cognito User Pools:
     - Sign in functionality for app users
     - Integrate with API Gateway
   - Cognito Identity Pools (Federated Identity):
     - Provide AWS credentials to users so they can access AWS resources directly
     - Integrate with Cognito User Pools as an identity provider
   - Cognito Sync:
     - Synchronize data from device to Cognito.
     - May be deprecated and replaced by AppSync 
  
## AWS Cognito User Pools (CUP)
   - Create a serverless database of user for your mobile apps
   - Simple login: Username (or email) / password combination
   - Possibility to verify emails / phone numbers and add MFA
   - Can enable Federated Identities (Facebook, Google, SAML…)
   - Sends back a JSON Web Tokens (JWT)
   - Can be integrated with API Gateway for authentication

## AWS Cognito – Federated Identity Pools
   - Goal:
     - Provide direct access to AWS Resources from the Client Side
   - How:
   - Log in to federated identity provider (Google, FB, Amazon or CUP) – or remain anonymous  
   - Get temporary AWS credentials back from the Federated Identity Pool
   - These credentials come with a pre-defined IAM policy stating their permissions
   - Example:
     - provide (temporary) access to write to S3 bucket using Facebook Login
     - login to facebook 
	 - facebook provide token 
	 - send token to cognito Federated Identity Pools
	 - cognito validates the token with facebook
	 - if token valid, requests STS to provide temp credentials to access S3
	 
## AWS Cognito Sync
   - Deprecated – use AWS AppSync now
   - Store preferences, configuration, state of app
   - Cross device synchronization (any platform – iOS, Android, etc…)
   - Offline capability (synchronization when back online)
   - Requires Federated Identity Pool in Cognito (not User Pool)
   - Store data in datasets (up to 1MB)
   - Up to 20 datasets to synchronise

## AWS SAM - Serverless Application Model
   - SAM = Serverless Application Model
   - Framework for developing and deploying serverless applications
   - All the configuration is YAML code
     - Lambda Functions
     - DynamoDB tables
     - API Gateway
     - Cognito User Pools
   - SAM can help you to run Lambda, API Gateway, DynamoDB locally
   - SAM can use CodeDeploy to deploy Lambda functions   
