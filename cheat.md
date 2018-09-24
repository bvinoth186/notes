# AWS Global Infrastructure *
- Region – Geographical location
- Availability Zone – Data center, 2 or more ny region
- Edge Location – Cashing,  Cloud Front, CDN- Content Delivery Network

# Compute *
- EC2 – vm
- EC2 container service
- Elastic BeanStalk
- Lamda
- Lightsail – Virtual private service, fixed IP address 
- Batch –  Batch computing in cloud 
# Storage *
- S3 – simple storage service, object based stored, bucket
- EFS – Elastic File System -  Network based storage 
- Glacier – Data Archival 
- Snowball – bring large data into datacenter 
- Storage Gateway – Virtual appliance, replicates date to S3
# Database *
- RDS – Relational Db service, mysql, postgress, oracle 
- DynamoDB – Non relational 
- Elasticache -  caching data 
- Red Shift – Data Warehousing 
# Migration 
- AWS Migration Hub – track the migration 
- Application Discovery service - discover the dependencies. Eg: sharepoint or dependency with DB
-  Database migration service
- Server migration service 
- Snowball
# Networking & Content Delivery*

- VPC – virtual privacy cloud.  Configure your own private cloud
- CloudFront – CDN,  stores your image/video closer  to your AZ, edgelocation
- Route53 – DNS service 
- API Gateway – 
- Direct Connect – direct line from hdq to amazon cloud

# Developer Tools 

- CodeStar – colabrating code
- CodeCommit – store code. Version controller 
- Code Build
- Code Deploy
- CodePipeline
- X-Ray - debug
- Cloud9 – IDE, browser

# Management Tools*	

- Cloud watch – 
- Cloudformation – 
- CloudTrail – log the changes 
- Config – monitor the configuration, visual 
- OpsWorks 
- Service Catalog 
- Systems Manager – patch all the EC2
- Trusted Advisor – advice security, risks, capacity 
- Managed services 

# Media Sevices
- Elastic Transcoder – resize video to android /ios

# Machine Learning 

- Sagemaker – 
- Comprehend 
- Deeplens – physical hardware – camara
- Lex
- Machine learning 
- Polly – text to voice 
- Rekognition – upload image / video, will tell you what it is
- Amazon translate – translator
- Amazon Transcribe – speech recognition – speech to text  

# Analytics *
- Athena
- EMR – big data solution. To process large data 
- Cloudsearch 
- ElasticSearchService
- Kinesis – bigdata, ingesting large amount of data in cloud
- Kinesis Video Streams
- Quicksight – BI tool
- Data Pipleline – moving the data bw to amazon services
- Glue – ETL

# Security & Identity & Compliance *
- IAM – Identify Access Management
- Cognito – Auth service, gives temp access to AWS
- GuardDuty – malicious activity
- Inspector – governance tool, check the vulnerability 
- Macie – scans S3 for PII data
- Certificate Manager – manage SSL 
- CloudHSM – Hardware security Module - stores encryption keys 
- Directory Service – integrating Active directory
- WAF – Web Application Firewall – prevent XSS, Sql injection
- Shield – prevent Ddos attacks 
- Artifact – compliance report, PCI report 
# Mobile Service 

- Mobile Hub – management console for mobile apps, 
- PinPoint – push notifications 
- AWS Appsync – automatically updates the data (offline).  
- Device Farm 
- Mobile Analytics 

# AR / VR 
- Sumerian – code name 
# Application Integration*
- Step functions
- Amazon MQ
- SNS – Notification service 
- SQS -  Decoupling 
- SWF – Simple Workflow Service 

# Customer Engagement 
- Amazon Connect
- SES - Simple Email Service 

# Business Productivity 
- Alexa for Business 
- Chime – Video conference like xoom
- Work Docs 
- WorkMail – like office 365

# Desktop and App Streaming 

-  Workspaces 
- AppStream 2.0 like citrix 

# IOT
-  IOT device management 
- Amazon FreeRtos – OS for microcontroller 
- GreenGrass 

# Game Development
- GameLift

# IAM 
- Identity Access Management
- Its Global 
- Allows to manage users and their level of access to AWS console
- Users
- Groups 	- collection of users under one set of permissions
- Role
- Policy – document defines one or more permissions 
  - Managed policies 
    - Created and administrated by AWS
    - Can be attached to multiple users, roles, groups with aws or different aws accounts
    - You cannot change the permissions
    - Recommended
  - Customer Managed policies 
    - Policy that you create 
    - Can be attached to multiple users, roles, groups, but only with in your own aws account
  - Inline policies 
    - Policy embedded with user, role or group
    - When you delete, user, role or group, inline policy also will be deleted 
- Users – Programmatic, console
- Programmatic -  Access Key Id &  Secret Access Key 
- Console – User & Password
- For region specific settings, add conditions as part of policy
- For new users by default no permissions
- https://<yourcompany or accountNo>.signin.aws.amazon.com/console - IAM users signin link
- Power user - Access to all the AWS resources except management of groups,users with in IAM

# STS – Security Token Service
- Grants users limited & temp access to AWS resources
- Federation – SAML, AD, Single sign on 
- Federation Mobile – FB, amazon, google or openId providers to login
- Cross account access – one aws account to access another resource 
- Federation – Combining list of users from one domain with list of users in another domain. Like from IAM to AD or FB
- Identity Broker – service that allows you take an identity from point A and join (federate) it with point B, you have to develop, it will not come out of the box
- Identity Store – AD, FB, Google 
- Identity – user of service like FB
- Identity Broker always authenticate with federation (AD, LDAP, FB, Google)  first then STS
- Temp Security Credentials 
  - Access Key ID
  - Secret access key 
  - Security token
- GetFederationToken 
  - Returns the set of temp security credentials for federated user
  - you must call GetFederationToken operation using the long term security credentials of IAM user. not IAM role 

# Active Directory Federation 
- AssumeRoleWithSAML API 
- Always authenticated with AD first then AWS
- SAML – Security Assertion Markup language 

# Web Identity Federation 
- Authenticate using Google, FB, LinkedIn 
- AssumeRoleWithWebIdentity API
- ARN – Amazon Resource Name

# Cognito
- Web identity federation service 
- User first authenticated with web identity provider (FB). Received auth token, exchanged for temp. aws credentials allowing them to assume an IAM role
- Provide signup / signin for your apps
- Access for guest users 
- Acts identity broker between your app and web ID providers (FB or Google)
- Recommended for mobile apps runs on aws
- Uses Push synchronization – to push updates and synchronize user data across multiple devices 
- SNS is used to silent push notification to all the devices associated with the given user identity 
- User pools - A user pool is a user directory in Amazon Cognito. With a user pool, your users can sign in to your web or mobile app through Amazon Cognito. Your users can also *sign in through social identity providers* like Facebook or Amazon, and through SAML identity providers.  
- Identity pools  - Identity pools provide AWS credentials to grant your users access to other AWS services. To enable users in your user pool to access AWS resources, you can configure an identity pool to exchange user pool tokens for AWS credentials  
- 


# EC2
- Elastic Compute Cloud 
- Allowing only to pay capacity you used 
- OnDemand – Pay a fixed rate by hour or second, low cost, flexible, short time, unpredictable workload,  first time, no upfront payment, no long-term commitment
- Linux by second,  windows are by hour
- Reserved – Reserve 1 year to 3 year terms, steady state, predictable usage, and reserved capacity, standard RI, convertible RI, and Scheduled RI.  (RI – Reserved Instances) 
- Spot – bid price,  flexible start and end times
  - If you terminate the instance, you have pay for the partial hour
  - If amazon terminate the instance (bid range), you don’t have to pay
- Dedicated – Physical, use existing software licenses. 
- Public IP or Elastic IP address 
- Evaluation logic – default deny 
- For penetration test – request to amazon and get the approval 
- Test allowed (after approval) only for EC2 and RDS 
- Vulnerability and penetration test to Other resources are prohibited 
- If the instance is terminated, you can find the reasons under ‘State transition reason’ label
- By default all accounts are limited to 5 elastic ip addresses per region 
- Xen - underlying hypervisor 
- Auto scaling group of spot instaces in primary and Auto scaling group of OnDemand instances in secondary is cost effective 

# Instance Families 
- D2 – Density – File servers / Dataware / Hadoop
- R4 – RAM – Memory optimized Apps/DBs
- M5 – main choice General – app servers
- C5 – CPU – CPU intensive Apps / DNs
- G3 – Graphics – Video Encoding, 3D
- I3 – IOPS, high speed storage – NoSQL, DB, DataWare
- F1 – FPGA – Field Programmable Gate Array – Hardware Acceleration 
- T2 – cheap general purpose – web servers, small DB’s 
- P3 – Graphics (pics) – Machine Learning, Bit Coin
- X1 – Extreme Memory – SAP HANA,  Apache spark
- H1 – High disk throughput - HDFS
- DIRTMCGFPX – dirty Melbourne ground (old)
- DR Mc GIFT PX  - 2017 acronym 
- FIGHT DR Mc PX
- F  - FPGA
- I – IOPS 
- G – Graphics
- H – High Disk Throughput
- T – Cheap general purpose (Think T2 micro)
- D – Density 
- R – RAM 
- M – Main choice for general purpose
- C – Compute
-  P – Graphics (think pics)
- X – Extreme memory 

# EBS 
- Elastic Block Store - allows to create to storage volumes and attach with EC2 instance
- Virtual disk
- Think of disk in the cloud attached to EC2
- Automatically replicated in AZ
- IOPS 
- General Purpose SSD - GP2 – upto 10000 IOPS
  - boot volumes 
  - low latency apps
  - dev and test environments
- Provisional IOPS SSD (IO1) – more than 10000 IOPS – designed for I/O intensive applications, 
  - large relational or nosql DB, Mongo, Cassandra, Mysql, oracle etc
  - Can provision upto 20000 IOPS per volumn
- IOPS – input output operation per second 
- Throughput Optimized HDD (ST1) – magnetic not SSD
  - Big Data 
  - Frequently accessed workloads
  - Data warehouse 
  - Cant boot volume 
- Cold HDD (SC1)
  - Lowest cost 
  - Less Frequently accessed workloads
  - File server 
  - Cant boot volume
- Magnetic (standard) 
  - Bootable 
  - HDD
  - Cheap
  - Data accessed Infrequently 
- Cannot mount 1 EBS volume to multiple EC2 instances – instead us EFS
- Can transfer reserved instance from one AZ to another
- Snapshot life cycle policy - for backups
- EBS optimized instance provides additional, dedicated capacity for EBS I/O
- Encryption availible only in certain instance types
- To improve performance
  - EBS optimized instances
  - Modern linux Kernal 
  - RAID 0 to maximize utilization of instance resources 
  

# Instance Store
- Called as Ephemeral storage 
- Volume cannot be stopped. If the underlying host fails you will lose the data.  But EBS backed instance can be stopped. You will not lose the data if the instance stopped. 
- Cannot be attached or detached to other instances 
- We can reboot both.  Will not lose the data ??
- By default root volume will be deleted on termination.  But EBS we can have option
- Usecase 
  - boot volumes
  - transactional and no sql Db
  - datawarehosuing
  - ETL 
- Cannot attach instance store volumes once the EC2 is launched
- few EC2 types doesnt support instace store volumes 
 
# EC2 Lab

- AMI – Amazon Machine Instance  (when you create AMI, registerImage is the final process)
- AMI can be shared to other AWS account 
- AMI can be changed to public
- If you make AMI, it will not be immediately available across all regions
- AMI’s can be only be shared within region.  For other regions do copy
- Instance type – t2 micro 
- Default VPC
- One subnet always will be in one AZ.  Same subnet range will not be shared across AZ.
- Termination protection – by default its Off
- Monitoring – cloud watch 
- Tenancy – dedicated 
- Advanced Details – user input, Boot instructions (install PHP SDK, apache)
- Storage – Root, Boot volume
- By default Root volume can’t be encrypted.  Either bit locker, or encrypt when creating AMI or API.  
- By Default Root EBS volume will be deleted on termination.  Option can be changed
- Tags
- Security Group, SSH, SSL, HTTP. 
- Source – Custom, Anywhere, My IP.  Anywhere means open to world 
- Key Pair – Pem file
- Same private key can be used for multiple EC2
- Status check –System Status check, Instance Status check 
- Monitoring, Cloud watch – basic monitoring, CPU. Disk,Nw
- Standard monitoring – 5 mins 
- Private key should be protected from read/write from other users. 
- Unprotected private key file – do chmod 400
- Unix
  - Change the permission chmod 400 *.pem
  - ssh ec2-user@<ip> -i <pem>
  - sudo yum update -y ->   to update security packages, -y to force update 
  - yum install httpd –y –>  install apache 
  - cd /var/www/html 
  - nano – text editor 
  - nano index.html 
  - Add something 
  - Ctrl X to exit and save 
  - service httpd start
  - chkconfig httpd on -  starts automatically with boot
- Windows
  - Putty Keygen to convert pem to ppk

  # Security Group 
- Its virtual  firewall
- State Full
- 1 instance can have multiple security groups
- 1 security group can be assigned to multiple instances. 
- Rules will be applied immediately.  Restarting the instance not required. 
- Inbound rule will automatically add outbound – State full
- In VPC, Network Access control list, State less, we need to add both inbound and outbound. State less
- Can’t deny the traffic. Everything denied by default. Add rule to allow traffic.
- Can’t block specific IP address, instead use Network access control
- All inbound traffic are blocked by default 
- All Outbound traffic are allowed by default 

# EBS Volumn
- Its virtual hard disk
- Snapshot – point in time copy of volume. 
- Volumns exists on EBS
- Snapshots exists in S3. 
- Snapshots are incremental.  This means only the blocks changed since last snapshot will be moved to S3
- First snapshot will takes time
- Snapshots encrypted automatically 
- Volumes restored from encrypted snapshots also encrypted automatically 
- You can share snapshots, only if unencrypted. This can be shared to other AWS accounts or public 
- EC2 and EBS Volumes has to be in same AZ. Always 
- Root volume – snapshot 
- Modify volume -  no downtime 
- EBS volume can be changed on the fly. Including Size and Storage type 
- From SSD, can’t change to HDD,  but HDD can be changed to both SSD and HDD
- Standard (Magnetic)  - you can’t modify the volume,  others can 
- To create / move volume to another AZ, create snapshot of root volume 
- From snapshot, create volume, image, copy 
- With Copy, you can move it to another region 
- TO create snapshot for root, you should stop the instance. however you can create the snapshot when instance is running 
- From Snapshot, both volume and image can be created
- AMI are regional, AMI can be launched where it’s stored. 
- AMI can be copied to another region, using console, cli and aws EC2 api’s  
- Snapshot of RAID array – take an application consistent snapshot.   Stop the application and flush all cashes to disk.  (Freeze the file system 0r unmount the RAID array or stop EC2 before snapshot) – stop any kind of IO 	
- you cant delete the snapshot of EBS volume that is used as root device of a registered AMI

# Encrypt EBS Volume

- While creating volume, option to encrypt.  Volumes can be attached or detached to running EC2 (Volume and EC2 should be in same AZ)
- Volume from encrypted snapshot will be automatically encrypted
- Volume from non-encrypted snapshot will be not be encrypted
- Can create encrypted snapshot from non-encrypted snapshot by creating encrypted copy of the non-encrypted snapshot
- Root volume will not be encrypted by default
- To encrypt root volume, 2 ways
- 1. OS level like bit locker 
- 2.  Create snapshot, Copy, while copying to do encrypt, from encrypted snapshot, create image and launch Image.  Launching will not supported in free tier 
- Encryption – Master Key – Default – aws/ebs
- lsblk – List the block devices 
- file –s  {/dev/xvdf}  - to view the filesystem 
- create file system before mount 
- create file system – mkfs –t ext4 /dev/xvdf
- to mount go to root and -  mount {/dev/xvdf}  /filesystem (create filesystem dir in root)
- to unmount – umount –d {/dev/xvdf} 
- You cannot encrypt the existing volume  

# RDS 

- Relational Database Service 
- SQLServer 
- Oracle
- MySQL server 
- PostgreSQL
- Amazon Aurora
- MariaDB
- Elastic Cache – cache the frequently accessed data 
- Supports Memcached, Redis (open source caching engines)
- RDS us OLTP
- RedShift – OLAP
- RDS will always give you DNS.  No ip address 
- Assign security group. Important to access RDS from EC2
- RDS backup - 1. Automatic backup 2.  DB Snapshot 
- Automatic Backup - by default, point in time backup in a regular interval, can recover to any point within retention period, retention period is 1 to 35 days. It will take full daily snapshot and will store transactions throughout the day.  So when we recover, aws choose most recent daily backup and apply transactions relevant to that day. 
- Backup data stored in S3.  You will free storage space in S3 equal to your DB size
- During backup window, there will be some latency 
- Snapshot – Manual, user initiated.  They are stored even after RDS is deleted.  (in Automatic, backups will be deleted in RDS is deleted)
- When you restore it will be new RDS instance with new RDS endpoint. (both auto and snapshot)
- Encryption – all RDS supports encryption
- Encryption is done using KMS – AWS Key Management Service 
- Once RDS is encrypted, data from snapshot or back up also will be encrypted. 
- We can’t encrypt existing RDS instance. we need take snapshot, copy, encrypt and restore
- Multi AZ-RDS – it allows you have an exact copy of your DB in another AZ.  AWS does the replication for you.  in case of DR, aws automatically failover to backup RDS.  
- RDS endpoint is always a DNS name 
- Multi AZ is only for DR not for performance 
- Multi AZ is synchronous  
- For performance go for read replica 
- Read replica – it allows you to have read only copy of production DB
- Read replica  - Asynchronous
- Read Replica – must have auto backup turned on, can have upto 5 read replicas, can have read replica’s of read replica (latency),  each replica will have own endpoint, , can be promoted has own DB but this will break replication, can have read replica in second region. 
- Can set read replica’s  as multi AZ for mysql and mariaDB. PostgreSql is not supported yet. 
- Read replica can be encrypted 
- RDS cannot be paused or stopped. Take the snapshot for future use and terminate 
- Supports 
  - Auto backup 
  - Auto software patching
  - Auto failure detection and recovery 
- Scaling is not automated. User has to do some clicks
- When creating RDS, user must specify, multi AZ or not
- you cant RDP or SSH into RDS instance 
- no charge to data transfer to replicas
- RDS reserved instances are available for multi AZ deployments 

# Aurora

- will run only in AWS infrastructure
- MySQL compatible 
- 5 times better performance then MYSql
- storage autoscaling - 10 GB to start with
- 2 copies of your data in each AZ, with minimum of 3 AZ. so 6 copies of your data
- Storage is self healing. data blocks and disks are continuously scanned for errors and repaired automatically
- designed to transparently handle the loss of upto 2 copies of data without affecting database write availability and upto 3 copies without affecting read availability
- 2 types of replica 
  - Aurora replicas (currently 15) - in case if primary aurora is down automatically failover to replica
  - MySql read replicas (currently 5) - will not fail over

# Elastic Cache 

- Elastic Cache – cache the frequently accessed data 
- Good if your app / db is read heavy work loads – social networking, gaming 
- Supports Memcached, Redis (open source caching engines)
- Memcached – Memory object caching system,  No persistence, can grow or shrink simila to Ec2 auto scaling, individual nodes are expandable, automatic node replacement and auto discovery. Multi-threaded, no multi AZ capability  
- Memcached usecases – caching is primary goal or offload your DB or simple cashing model as possible or planning to run large cache nodes and require multithreaded performance with utilization of multiple cores or you want to ability to scale you cache horizontally as you grow then use Memcached.  
- Redis – In memory key value store, supports data structures like sorted set and List. It supports master slave replication and Multi AZ.  So it’s used to achieve cross AZ redundancy. 
- Redis – replication and persistence features 
- Elastic Cache manages redis as a relational database, stateful, supports failover
- Redis usecases – more advance datatypes as lists, hashsets, sets or sorting and ranking datasets in memory like leaderboards or persistence of keystore is important or pub sub capabilities or  you want to run in multiple AZ with failover  then go with Redis. 
- Caching strategies 
  - Lazy loading 
  - Write through 
- Lazy loading – loads the data into cache only when necessary. Only at the time its requested 
- Cache hit – returns the data 
- Cache miss or expired – returns null – so your application  needs to get the data form DB and update to elastic cache
- Write through – adds or update to the cache whenever data is written to the database 
- Data In the cache is never stale 




# EFS – Elastic File System
-  File storage service 
- Elastic – grow and shrink based on the need. 
- Can share EFS across multiple EC2.  EC2 instances should be in same security group as EFS
- Block based storage (S3 is object based storage)
- Read after write consistency 
- Pay only what you use 
- Scale upto petabytes 
- Supports NFSv4
- Supports 1000 of concurrent NFS connections 
- Data stored in multiple AZ within the region 
- EFS can be mounted in multiple AZ
- EFS can be used only within one VPC at a time
- with EFS, both file system and VPC should be in the same region 
- EFS will not be available for all VPC's within region by default. 
- EFS does not support cross region peered VPC's
- For EFS, can work with VPC peering within single AWS region when using C5 or M5 EC2 instances 
- Security group to open NFS port 2049 to access EFS 
- Encryption at rest can be enabled only at the time of EFS creation 
- NFS not an encrypted protocol 
- Encryption at transit canNOT be enabled only at the time of EFS creation 
- To enable encryption at Transit, 
  - Enable encryption during mounting on EC2 using amazon mount helper
  - unmount unencrypted mount 
  - remount using mount helper encryption during transit option 
  - encryption at rest cannot be done during mounting. it can be done only during craetion. 
- Performance Mode 
  - General purpose - recommended for most file system, low latency 
  - Max I/O - recommended for tens or hundreds or thousands of EC2 sharing EFS. slightly higher latency. big data 
- Throughput mode 
  - Provisioned - can configure specific throughput irrespective of EFS data size
  - Bursting - throughput on EFS scales as file system grows 
- Usecases 
  - Bigdata and analytics
  - media processing workflows
  - content managment
  - web serving 
  - home directories 

# AWS – CLI 
- aws configure 
- cd ~,  cd .aws, credentials and config file
- aws <service> help
- aws ec2 describe-instances  will show terminated accounts also
- aws ec2 describe-images  will show all the available images (both public and private) 
- aws ec2 run-instances  To provision the instance
- aws ec2 terminate-instances –-instance-ids <id>
- role can be assigned to running instance 
- With role, acceskey and password not required.  Just configure with region
- aws s3 cp –recursive s3://<s3_bucket> <destination> --region <region where S3 is located>
- Best practice is to use region.  Some will work without region 
Bash Scripting 
#!/bin/bash
yum update -y
yum install httpd -y
service httpd start
chkconfig httpd on
aws s3 cp s3://indexbucket-186/index.html /var/www/html --region ap-south-1

# Meta Data

- curl http://169.254.169.254/latest/meta-data/
- ami-id
- ami-launch-index
- ami-manifest-path
- block-device-mapping/
- hostname
- iam/
- instance-action
- instance-id
- instance-type
- local-hostname
- local-ipv4
- mac
- metrics/
- network/
- placement/
- profile
- public-hostname
- public-ipv4
- public-keys/
- reservation-id
- security-groups
- services/
- https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-metadata.html
- https://github.com/acloudguru/metadata
# Cloud Watch
- Monitoring, Cloud watch – basic monitoring, CPU. Disk,Nw
- Standard monitoring – 5 mins 
- Detailed monitoring – for every 1 min (not free)
- Cloud Watch – performance monitoring
- Cloud Trail – Audit trail 
- Dashboards
- Alarms 
- Events 
- Logs
- 


# Elastic Load Balancer - ELB

- No public IP. Only DNS name. Amazon manages public IP
- Health check.   In service, out of service
- It will cost if its running
- Classic load balancer – TCP/IP layer or Http/Https, layer 7 or 4, legacy, not recommended. Important for exam
- Application Load balancer – application layer,  operates at layer 7, best suited for http/https, Can look at till application level to decide routing 
- Network Load balancer – level 4, most expensive, TCP, extreme performance, can handle millions of requests per second, supports only TCP. doesnt support http/https 
- Healthy threshold – no of success healthy checks before declare the instance as in service
- UnHealthy threshold – no of failure healthy checks before declare the instance as out of service
- Timeout
- Interval
- Error code 504 – Gateway Time out error. Issue is with application (web server or DB)
- X-forwarded-For Header - Load balancer sending user’s source IP to EC2 in X forwarded for header. 
- If you ipv4 public address of end user, look for x-forwarded-for header in classic load balancer. 
- To maintain session state or even distribution session in ELB – use Elastic Cashe to store transient session data 
- ELB use ports from 1024 to 65535
- You can specify only one subnet per AZ
- Atleast 2 subnets must be specified
- Subnet should have internet access. Private Subnet will not be allowed
- If doing load test on ELB, 
  - Ensure to re-resolve DNS before every request
  - Send requests from globally distributed clients or multiple test clients 
- To split the traffic accross the AZ - Enable Cross Zone load balancing (for classic) 
- Cross Zone load balancing 
  - Enabled in Application and Network load balancer by default 
  - Not Enabled in classic load balancer by default
- To monitor application load balancers 
  - Cloudwatch metrics 
  - Access logs
  - request tracing 
  - CloudTrail logs 
- Target Type 
  - instance Id 
  - Ip address 
  

# SDK 
- Software Development Kit
- https://aws.amazon.com/tools/
- Java, .Net, Node.js, PHP, Python, Ruby, Browser, GO, C++
- AWS IOT
- Android, IOS, React Native, Mobile web, AWS Mobile 
- Default Region US-East-1, North Virginia 
- Some have default regions (java)
- Some do not (Node.js)

# Lambda 
- Serverless (lambda, S3, DynamoDB, Api gateway).  EC2 & RDS is not 
- Its compute service, where you can upload the code and create lambda function
- Aws lambda takes care of provisioning and managing servers 
- We don’t have to worry about OS, patching, scaling etc
- AWS Lambda – data center, hardware, protocols, OS
- Lamda events can trigger other lamda events 
- Can communicate to aws services 
- Scale up – increase RAM, memory, storage 
- Scale out – spawn EC2 instance based on load
- Lambda scales out automatically. If your lambda run out of memory, you need to upgrade the configuration 
- Lambda triggers (eg: api gatway, S3, Cloudfront, dynamoDB) important exam topic, memories all the triggers 
- Event Driven – when u upload Meme to S3 lambda can be triggered to add text. After adding text another lambda can be triggered to show to user and another lambda to replicate the meme in another S3
- Api Gateway – when http request comes to api gateway, lambda can be triggered to give the response
- If 1 million requests to API gateway, I million lambda functions will be triggered.  Automatically scale out
- Languages – Java, Node.js, python, Go and C#
- Lambda priced based on no of requests
- 1st 1 million requests are free. $.20 per 1 million there after
- Also priced based on duration. Max threshold is 5 mins. (300 seconds)  If the function takes more than 5 mins, split into multiple functions default timeout is 3 seconds 
- No servers
- Super cheap 
- Amazon echo is lambda 
- AWS x-ray to debug lambdas 
- Lambda can do things globally 
- Lambda versioning – version control 
- Can have multiple versions of lambda function
- Lambda versioning immutable. Once published can’t be changed  
- $LATEST – latest version 
- Qualified ARN – the function ARN with the version suffix 
- UnQualified ARN – the function ARN without the version suffix 
- Alias – can create alias and map the version behind the screen 
- Can split traffic using alias to different version.  Alias can 50 % to v1 and 50% traffic to v2
- Cannot spilt traffic with $latest
- Lamda default timeout – 3 seconds 
- You can set memory in 64 MB increments from 128 MB to 3 GB
- If you choose 256 MB, it allocates approximately twice as much CPU power to your lambda function as requesting 128 MB of memory and half as much CPU power as choosing 512 MB of memory 

# API Gateway
 
- Fully manages service used to publish, maintain, monitor, and secure api’s at any scale \
- Low cost and scales automatically 
- Can throttle API gateway to prevent attacks 
- Can log results to cloudwatch 
- Configure API gateway : 
  - Define an API (container)
  - Define resources and nested resources (URL paths)
  - For each resource, select HTTP method, security, target (EC2,lamabda) and request response transformation 
  - Deploy API to STAGE & PROD 
- Uses API gateway domain by default.  Can use custom domain 
- Now supports AWS certificate manager (free SSL TLS certs)
- API caching – cache your endpoint response for the specific TTL (time to live) period in seconds 
- Same Origin policy – under the policy, web browser permits, scripts in first web page to access data in second web page. But only if both the web pages have the same origin. This is to prevent cross site scripting (XSS)
- CORS policy – CORS is enforced by the client
- Can import API from an external definition file 
- Currently (import) supports swagger v2.0
- API Throttling 
  - By default API gateway limits 10000  rps (requests per second)
  - By default Max concurrent requests is 5000 across all api’s with in aws account 
  - If you go over 10000 rps or 5000 concurrent requests – you will get 429 –Too many requests 
- if the caller sends 10000 requests in the first milli seconds, API gateways serves 5000 of those requests and throttles the rest in the one second period. 
- API gateway can be configured as soap webservice pass through
- Integration sources 
  - Lambda 
  - Http 
  - Mock
  - AWS service
  - VPC link 
- Use VPC Link to integrate on premises backed solutions through direct connect and private VPC. i.e, Rest api's are exposed to internet and hosted in on-premises. using VPCLink the api's can be integrated with api gateway.
- Controlling access to api in api gateway 
  - resource policy - add policy to allow or deny access from specified IP addresses
  - IAM roles and policies 
  - CORS
  - lambda autorizers
  - cognito user pools
  - client side SSL certs
  - usage plans 
- Automatically protect your backend systems from Distributed denial of service (DDoS) attacks. 
- Access logging - logs who has accessed your API and how they accessed. 
- Automatically integrates with CloudFront to ensure better response to the calls made to the API
  

# Step Function
- Allows to visualize and test your serverless applications
- Automatically triggers and tracks each step
- Retries if error
- Logs each step – diagnosis and debug easy 
  - Sequential steps
  - Branching steps 
  - Parallel steps 
# X-Ray
- AWS X-Ray makes it easy for developers to analyze the behavior of their distributed applications by providing request tracing, exception collection, and profiling capabilities.
- Debugging 
- Tracing 
- Service Map – Visual map
- X-Ray SDK provides,
  - Interceptor – To add your code to trace incoming http requests
  - Client Handler –  to call other AWS services
  - Http Client – to call other internal or external http webservices

- X-Ray integrates with following AWS services
  - Elastic Load balancer 
  - Lambda
  - API Gateway 
  - Ec2
  - Elastic beanstalk 
- X-Ray languages supported – Java, Go, Node.js, Python, Ruby, .Net

# S3
- Simple storage service
- Object based 
- Files can be 0 bytes to 5 TB
- Unlimited storage 
- Files stored in buckets 
- Not suitable for OS or DB
- S3 is universal. Bucket name should be unique
- https://s3-us-west-1.amazonaws.com/aacloudguru
- Read after write consistency for PUTS of new objects -  for new uploaded file, can be accessed immediately 
- Eventual consistency for overwrite PUTS and DELETES – for update and delete files, retrieve is will take little time.  
- After update or delete, retrieve is atomic. Which means it will return either updated file or old file. it will not return partial or corrupted file (reason, sync with all AZ)
- S3 contains 
  - Key – name of the object 
  - Value –data (sequence of bytes)
  - Version Id – versioning 
  - Meta data
  - Sub resources – bucket specific configuration (policies, access control lists), CORS, transfer acceleration 
  - Torrent
- Built for 99.99 % availability 
- Amazon guarantees 99.99% availability  
- Amazon guarantees 99.999999999% durability (remember 11 x 9 s)
- Tiered storage available 
- Life cycle management 
- Versioning
- Encryption
- Secure your data – access control list and bucket policies 
S3 storage tiers / classes 
- S3 
  - 99.99 % availability
  - 99.9999999999 % durability  
  - 
  - Designed to sustain the loss of 2 facilities concurrently
  - Currently cost effective 
- S3 – IA 
  - Infrequently accessed 
  - But rapid access when needed 
  - Lower fee then S2 and but charged retrieval fee  (every retrieval) 
- S3 – One Zone IA 
  - Same has IA but data stored in single AZ 
  - 99.5 % availability only 
  - 99.9999999999 % durability  
  - Cost is 20% less the S3-IA
- Reduced Redundancy Storage (legacy - one zone IA)
  - 99.99 % availability
  - 99.99 % durability only
  - Used for data that can be created if lost 
  - Ex: thumbnails 
  - Low cost 
  - Not recommended 
- Glacier 
  - Archiving
  - Not part of S3, but linked 
  - Very cheap 
  - 99.99 % availability
  - 99.9999999999 % durability  
  - Infrequently accessed 
  - No real time access 
  - it takes 3 to 5 hours to restore data from glacier 
- charged for 
  - Storage per GB
  - No of request (get, put, copy , delete etc
  - Storage management – inventory, Analytics, Object tags
  - Data Management pricing – Data transferred out of S3
  - Transfer Acceleration – cloudfront
- S3 – Security 
  - BY default – private
  - Access control 
    - Bucket policies – applied at bucket level, json, applicable to objects in bucket 
    - Access control lists – applied at object level 
-  Can configure to create access logs, which log all requests to S3.  These logs can be written into another bucket 
- S3 Policies – policy generator 
- S3 Encryption 
  - In Transit – SSL/TLS
  - Type of block cypher – Advanced encryption standard 256 – AES 256
  - At Rest, Server side encryption
    - SSE – S3  - S3 managed keys, AWS manage the keys and enc/dec – AES256
- Strong multi factor encryption
- S3 encrypts each object with unique key 
- It encrypts key itself with master key that it regularly rotates 
    - S3-KMS  - Key Management Service, AWS manage the keys. Additional benefits.  Can track the encryption, decryption activities 
    - SSE-C  - Customer provided keys. AWS will do enc/dec based on the keys provided by customer 
  - Client side encryption – client will encrypt the object and upload 
- 
- To enforce encryption, add bucket policy to deny all PUT requests if the header doesn’t have x-amz-server-side-encryption tag 
- Every upload is PUT request to S3
- x-amz-server-side-encryption – if this tag is included in header of PUT request, it tells s3 to encrypt the object with the specified encryption method. 
- x-amz-server-side-encryption:AES256 – this means SSE-S3, S3 managed keys 
- x-amz-server-side-encryption:ams:kms – this means SSE-KMS, KMS managed keys 
- Expect:100-continue – S3 will reject if the content type is invalid
- CORS – Cross Orgin Resource Sharing. To share the objects from one bucket to another 
  - Requires versioning enabled
- Static website hosting - <bucket-name>.s3-website-<region>.amazonaws.com
- S3 versioning 
  - Once enabled ypu cant disable. You can only suspend it 
  - Version no for objects before enabling versioning will be null
  - Can enable MFA for any deletes
  - each version is charageble 
- S3 life cycle management 
  - Eg : Move the file to S3-IA after 30 days and move to Glacier after 60 days 
  - Can delete permenantly after x days 
  - Can be applied to current as well as previous version

- Tp upload file more than  5 GB – Multipart upload API (recommended for more then 100MB)
- Multipart allows stop and resume uploads 
- Single put max size is 5 GB
- Multi Object Delete – to delete large number of objects, no additional charge
- By default 100 buckets per aws account. Contact aws for more
- S3 Naming 
  - 3 to 63 chars long 
  - Only lower case 
  - Number, period, dash allowed 
  - Must start with lower case letter or number
  - Underscore, end with dash, consecutive period, dash adjacent to period not allowed
  - IP address format not allowed
  - Key names are stored lexicographically (alphabetical order)
- S3 bucket ownership is NOT transferable. Owner can grant access to others
- Referrer policy – to allows access from specific domains only 
- 409 conflict error – when bucket you trying to delete is not empty through api. But through console, you can delete the non-empty bucket 
- S3 does support redirects 
- Costs are as follows( from most expensive to least) 
  - 1. S3-RRS .024 
  - 2. S3 .023 
  - 3. S3-IA .0125 
  - 4. S3OneZone IA .01.
# CoundFront
-  CDN 
- Can be used to deliver static, dynamic, streaming and interactive content 
- Geographically disbursed data centers
- Edge location – location where content is cached.  Different from Region / AZ
- Edge locations are not just read only. We can put the objects to sync to origin. 
- Edge Location will cache the content for the first time use, and serve the content from edge location for the subsequent requests. Request will not goto server. Improve latency 
- TTL - time to live – objects are cached for the life of TTL. by default 24 hours
- Manually clearing cache is allowed. But its chargeable
-  Origin – Origin of the files, CDN will distribute. This can be S3 or Ec2 or Elastic Load balancer or Route 53, Can work with non aws services as well
- Distribution – name of CDN, which consists of collection of edge locations
  - Web distribution  - for websites, http/https
  - RTMP	- for media streaming. Audio/Video
- S3 transfer acceleration make a use of cloudfront to accelerate transfer from S3 to end user.  
- Say for example, if geographically distributed users are uploading multiple files to S3 which is located in UK region. 
- Instead of uploading the files to S3 directly, they can upload to their nearest edge location and then edge locations accelerate the transfer to S3 trough optimized aws network.  
- Eg.  prefix.s3-accelerate.amazonaws.com
- S3 - https://s3-eu-west-1.amazonaws.com/test
  - https
- Website - http://test.s3-website-eu-west-1.amazonaws.com
  - http 
  - can be secured through cloudfront 
- Restrict Bucket Access – bucket can be accessed only via CloudFront
- Origin access identity 
- Grant Read Permission on Bucket. By default No. change it to Yes, update the bucket policy.  Important step. 
- Protocol policy – http or https, https only or convert http to https
- Restrict Viewer Access – Signed URL or Signed cookies – for specific paid content 
- Price class – use all edge locations or specific 
- Alternate domain names
- SSL certificate – default or custom (upload custom cert to aws cert manager)
- Enable IPv6
- Blocklist or whitelist specific geo locations
- Invalidate – manually invalidating the content to remove the content from the cache (chargeable) 
- S3 performance optimization 
  - GET-intensive workload
    - If most of the workload is GET, use cloud front to optimize the performance 
  - Mixed workload 
    - Means mix of GET, PUT, DELETE etc
    - Avoid sequential key names for S3 objetcs
    - S3 uses key name to determine the which partition an object stored 
    - For heavy workload, this might cause i/o issues
    - So add random prefix to key names, to prevent multiple objects stored in the same partition 
- Bucket level properties 
  - Versioning
  - Server access logging 
  - Object level logging 
  - Static website hosting 
  - Tags
  - Transfer acceleration
  - Events
- Bucket level properties 
  - Storage class
  - Meta Data 
- Virtual hosted S3 URL 
  - http://bucket.s3.amazonaws.com
  - http://bucket.s3-aws-region.amazonaws.com
- Path style S3 URL
  - http://s3.amazonaws.com/bucket --> for US East - N.Virginia 
  - http://s3-aws-region.amazonaws.com/bucket --> for other regions
  - http://s3.aws-region.amazonaws.com/bucket --> This also works
- S3 - https://s3-eu-west-1.amazonaws.com/test
  - https
- Website - http://test.s3-website-eu-west-1.amazonaws.com
  - http 
  - can be secured through cloudfront 
- Versioning 
  - Pricing includes all the versions
  - Delete api will not delete the object permanently. it will just add the delete marker. so this object eligible for year end billing 
  - To delete permanently, you must delete object version id
- Cross region replication requires versioning enabled on both source and destination buckets   

  
- https://aws.amazon.com/s3/faqs/


# Storage Gateway
- To back up the data
- availiable for download as image
- You can install on a host on your datacenter 
- supports either VMware ESXi or Microsoft Hyper-V
- once installed, associate with aws service through activation process
- Replicates data from your own data center (on premise) to AWS. You install it as a host on your data center.
- 4 types 
  - File Gateway (NFS)
    - Stores flat files directly in S3
	- Word, pdf, image, video 
  - Volumes Gateway (iSCSI)
	- block based 
	- disk volumes 
	- OS, sql server stored in virtual hard disk 
	- Stores into S3 in the form of EBS snapshots
	- its blockbased. so cant store into S3 directly 
	- snapshots are incremental
	  - Stored Volumes 
	    - Entire data is stored in on site (on prime) and SG then backups this data up asynchronously to Amazon S3 (as EBS snapshot). 
		- GS volumes provide durable and inexpensive off-site backups that you can recover locally or on Amazon EC2.
		- 1 GB to 16 GB
	  - Cached Volumes 
	    - Only your most frequently accessed data is stored in on prime and Your entire data set is stored in S3 (as EBS snapshot). 
		- You don't have to go out and buy large SAN arrays for your office/data center, so you can get significant cost savings. 
		- If you lose internet connectivity however, you will not be able to access all of your data.
		- 1 GB to 32 GB
  - Tape Gateway (VTL)
    - Used for backup
    - Limitless collection or VT. Each VT can be stored in a VTL. If it is stored in Glacier, is it a VT Shelf. 
	- If you use products like NetBackup etc you can do away with this and just the VTL. It will get rid of your physical tapes and create the virtual ones.
	- supported by Netbackup, Backup Exec, Veeam
  
  
# SnowBall
- previously import/export disk (legacy)
- can transfer your data (both import and export) in aws using amazon's high speed internal network avoiding internet
- Snowball
  - onBoard storage
  - Petabyte-scale data transport solution 
  - 80 TB snowball in all regions
  - tamper resistant enclosures
  - 256 bit encryption 
  - Trusted platform module (TPM)
  - once data transfer completed, aws does the erasure of snowball appliance
- Snowball Edge 
  - onBoard storage and compute capabilities
  - AZ in on prime (like)
  - can ensure to continue run your applications even they are not able to access cloud. can collect transfer the data to aws later
  - 100 TB data transfer device
- SnowMobile
  - Exabyte-scale data transport solution
  - can transfer upto 100 PetaByte per snow mobile
  - 45 foot long shipping container truck 
  - secure, fast and cost effective
- Snowball can import to S3 and export from S3
- if your data is in glacier then export to S3 and export using snowball

# Dynamo DB
- No SQL DB 
- You can’t specify AZ when you create DynamoDB table 
- Supports both document and Key-Value data models 
- Stored SSD storage - always
- Spread across 3 geo locations
- Choice of 2 consistency models
  - Eventual consistent reads (default) – all copies of data will reach all the data centers within second. Read after short time will always return the updated data – best read performance 
  - Strongly consistent reads 
- It uses conditional writes for consistency.  For PutItem, DeleteItem, UpdateItem operations. Operations succeeds only if attributes meet one or more conditions. Otherwise it returns error. 
- It uses optimistic concurrency control – optimistic locking – its strategy – your DB writes are protected from being overwritten by writes of others and vice versa 
- Supports atomic counters – all write requests are applied in the order they received 
-  Tables 
- Items – row 
- Attributes – key-value
- Json, html or XML 
- Stores and retrieves data based on primary key 
-  2 types of primary key
  - Partition key – key will determine which partition the data is stored.  Unique, eg: userId, productId

  - Composite Key – Partition Key + Sort Key, situations where partition key cannot be unique, E.g.: same user adds multiple entries in online forum. Here composite key will be user id + timestamp when the post posted 

- Partition Key – Hash 
- Sort Key - Range

- Access control can be managed via AWS IAM. Create user or Role to manage dynamoDb. Can also use special iam condition to restrict user access to only their own records. (Partition id = userId) – IAM policy parameter :  dynamodb:LeadingKeys 
- Has index features, 2 types of index
  - Local secondary index 
    - Can be created only at the time of table creation.  Cannot modify later
    - It has same partition key as your original table 
    - But different sort key 
  - Global secondary index
    - can be created at anytime
    - can have different partition key and different sort key 
- scan vs query
- Query – 
  - Query by partition key or partition key + sorted key. 
  - By default returns all the attributes. 
  - You can use the project expression parameter to return only the specific parameters 
  - By default sorted by sort key – numeric ascending – 1 2 3 4
  - Sorting order can be reversed by setting ScanIndexForward parameter to false.  This is applicable only to query not for scan 
  - By default – eventually consistent 
  - Query is more efficient then scan 
  - 
- Scan 
  - Returns entire table.
  - By default returns all the attributes. 
  - You can use the project expression parameter to return only the specific parameters 
  - Filter = to refine the results
  - Scan – dumps the entire table and add the filter – filter is extra step to remove the data 
- To improve performance 
  - Avoid scan 
  - Use page size 
  - Parallel scan – by default scan process sequentially, get 1 MB of data and retrieve next 1 MB, can configure parallel scan by logically dividing table or index into segments and scan each segment in parallel. Avoid parallel if your table already busy 
-  Provisioned throughputs – is measured by capacity units 
- Pricing based on the  capacity units 
- When you create a table, you specify your requirements in terms of read capacity units and write capacity units 
- If your application reads or writes larger items it will consume more capacity units and it will cost you more as well
- I read capacity unit represents 1 strongly consistent or 2 eventually consistent read per second for items upto 4 kb 
- Write capacity unit – 
  - 1 * write capacity unit = 1 * 1 KB write per second
- Read capacity unit 
  - 1 * read capacity unit = 2 * eventually consistent reads of 4kb per second (by default)
  - 1 * read capacity unit = 1 * strongly consistent reads of 4kb per second (by default)
- If table with 5 read capacity units and 5 write capacity units then 
  - 5 * 4kb strongly consistent reads = 20 kb per second
  - 5 * 2 * 4kb eventually consistent reads = 40 kb per second (twice as strongly consistent)
  - 5 * 1kb writes = 	5 kb per second 
- To calculate capacity units 
- If your application need to read 80 items (table rows) per second and each item is 3 kb size 
  - For strongly consistence reads 
    - Calculate how many read capacity required for  each read – 
    - size of each item /4 kb
    - 3kb / 4kb = 0.75 – round to 1 
    - For 80 items = 80 read capacity units required
  - For eventual consistent reads
    - Double the throughput of strongly consistence reads
    - Divide 80/ 2
    - 40 read capacity units required 
- If your application want to write 100 items per second and each item  is 512 bytes in size
  - Calculate how many capacity units for each write 
  - Size of each item / 1 kb 
  - 512 bytes  / 1 kb = 0.5 = round to 1
  - For 100 items = 100 capacity  units required
- Dynamo DB Accelerator – DAX
  - Fully managed, clustered, in-memory  cache for DynamoDb
  - More for read only purpose 
  - Delivers upto 10x read performance improvement 
  - Ideal for read –heavy and burst workloads 
  - E.g. auction apps, gaming, retails sites during black Friday promotions
  - It’s a write – through caching service – means data written to the cache and as well as back end store at the same  time 
  - Allows you to point your dynamoDb api calls at the DAX cluster 
  - If the item available in cache – cache hit
  - If the item not available in cache – cache miss
  - In cache miss – DAX performs eventually consistent getItem operation against dynamoDB and update cache and return 
  - DAX reduces read load on dynamoDB
  - May be able to reduce provisioned read capacity and save money 
  - Not suitable for 
    - Strongly consistent reads
    - Write intensive applications
    - Apps do not perform many reads 
    - Apps do not require micro second response time
- getItem api 
  - returns set of attributes for the item with the given primary key
  - eventual consistent read by default 
  - set consistentRead to true for strongly consistent 
- BatchGetItem Api 
  - Read multiple items 
  - Upto 100 items or 1 MB of data
- for any aws account, limit is 256 tables per region – contact aws for more
- cumulative (concurrent) number of tables and indexes in the CREATING, UPDATING, DELETING state cannot exceed 10 – if so you will receive “LimitExceededException”
- If you want to create more than one table with secondary index you must do sequentially. Create one wait till it became active and proceed to next
- Global secondary index is index with hash and range key that can be different from those on the table 
- You can create upto 5 global secondary index when you create table
- Each table can have upto 5 local secondary indexes 
- So totally 10 secondary index. Cant increase the limit beyond 10
- Number. String, Boolean data types can be indexed 
- Set, list, map types cannot be Indexed
- Max limit of item collection is 10 GB
- Smallest amount of capacity unit can be purchased is 100 (both reads and writes)
- Max size of item in dynamoDB = 400 kb
- Number if attributes item can have = no limit, but total size including attribute names and values should not exclude 400 KB
- Doesn’t support cross table joins (core RDS features not supported)
- ItemCollectionSizeLimitExceededException – for a table with local secondary index, exceeded the maximum size limit of 10 GB
- Scan is always eventually consistent 
- Secondary index are optional 
- If you do more reads/writes then provisioned capacity units, requests will be throttled and you will receive 400 error code  - ProvisionedThroughputExceededException
- Push button scaling - can scaled your DB on the fly. no down time. 
- No need to explicitly to create Multi AZ.  Dynamo DB is highly available
- It automatically replicate the data across mutplie AZ
- Better to support stateless web/app apps (RDS too but DynamoDB is better option) 
- Global Table - for good latency, if data is accessed from diff geo locations 

# RedShift

- Dataware housing service 
- for Business intelligence 
- OLAP
- Configuration
  - Single node - 160 GB
  - Multi node 
    - Leader node - manage client connections and receive queries 
	- Compute node - store data and perform queries and computations. upto 128 compute nodes
- Columnar data stroage 
  - redshift organizes the data by column 
  - row based systems for for transaction processing 
  - column based systems for datawarehosuing and analytics
  - 10 times faster 
- Advanced compression 
  - doesnt require index or materialized views 
  - uses less space
  - when loading the data into empty table, Redshift automatically samples your data and selects appropriate compression scheme. 
- Massive parallel processing (MPP)
- Easy to add nodes
- Pricing 
  - you will not be charged for leader node
  - charged for computer node
  - charged for backup 
  - charged for data transfer (only within VPC)
- Security 
  - transit - SSL
  - rest - AES 256 
  - by default RedShift takes care of key management
    - can manage your own keys by HSM 
	- or AWS KMS
- Availability
  - currently available in 1 AZ
  - can restore snapshots to new AZ's during outage
  
# KMS
- Key management service
- Keys are region based.  
- Customer Master Key - CMK
  - Alias
  - Creation date 
  - Description 
  - Key state 
  - Key Material (customer provided or aws provided)
- Can never be exported. Keys in HSM can be exported
- aws kms encrypt
- aws kms decrypt 
- aws kms re-encrypt
- aws kms enable-key-rotation 
- Envelope encryption key – data key – encrypted customer master key (CMK)
- Encrypt the data – get the envelop key by encryption cmk and encrypt the data using envelop key
- Decrypt the data – decrypt the envelop key using CMK and decrypt the data using decrypted master key 
- key cannot be deleted immediately.  Need to disable the key and schedule for deletion (7 to 30 days)

# SQS
- simple queue service 
- first, oldest aws service 
- Pull based system 
- Massages can contain upto 1 kb to  256kb of text in any format
- Billed at 64kb chunks
- I request can have 1 to 10 messages but max size is 256kb
- For message size larger then 256Kb use Amazon SQS Extended client library for Java – this library lets you to send a sqs message that contains reference of object in S3 that can be large as 2 GB
- Types 
  - Standard queues (default) – order is not guaranteed, message delivered at least once
  - FIFO queues (First – In – First – Out) – order guaranteed. Limited to 300 tps, message delivered once, no duplicates (ends with .fifo extension
- Messages can be kept in queue from 1 min to 14 days
- Default retention period is 4 days 
- Visibility Timeout – amount of time the message is invisible in sqs queue.  If the message once pulled it will be invisible. If the message is processed within visibility timeout, message will be deleted from sqs otherwise message will became visible for next thread to process
- ChangeMessageVisibility – api to extend visibility timeout
- Default visibility timeout is 30 seconds, max is 12 hours
- Short polling – will poll for messages frequently and return immediately with empty response if  the sqs is empty 
- Long polling – doesn’t return response until the message comes or long poll time out,  can save your money 
- To enable long polling, set ReceiveMessageWaitTimeSeconds to greater then 0 or equal or less then 20
- Max long poll time out – 20 seconds 
- Can use JMS
- No limit on the number of Queues 
- Can configure access IAM policy that allows anonymous users to access the queue 
- Free tier provides 1 million requests per month at no charge 
- SQS is PCI DSS level 1 certified 
- When consumes receives and processes message from queue, message remains in the queue after the visibility time out. Amazon doesn’t automatically delete the message. Since it’s a distributed system and there no guarantee component processed the message. It will be deleted only after the retention period. 
- So consumer must delete the message from queue – DeleteMessage API
- Dead letter queues – can target for messages which cant processed successfully 
- User can delete the queue at any time, whether its empty or not 
- Queue Name 
  - Limited to 80 characters 
  - Alphanumeric, -, _ allowed
  - Unique within aws account 
- SQS can trigger lamabda function
- to selece message to delete, use receiptHandle of the message ( not the messageId whcih you receive when you send the message) 
- SQS can delete message from the queue even if a visibility timeout setting causes the message to be locked by another consumer. 
- SQS doesnt encrypt the messages by default. there is option to encrypt the messages. 
  
# SNS

- Simple notification service
- Topic
- SMS, EMAIL, SQS, email, http/https, email-json, application, lamabda
- Push based system 
- To send notifications from cloud
- Push notifications
- Also can deliver notifications by text, email, sqs or http endpoint 
- SNS can trigger lambda functions – when message published sns topic that has lambda function subscribed, then lambda function will be invoked with the payload of the message published 
- All the messages published to SNS topic will be stored redundantly across multiple availability zones 
- Pay as you go model 
- Pub – sub model 
- O.50$ per 1 million amazon SNS requests
- Fanout pattern – message published to SNS topic is distributed to number of SQS in parallel, by using this pattern, can build the application can process parallel asynchronous processing. 
- SNS message body would have 
  - Type 
  - MessageId
  - Subject 
  - Message 
  - Signature Version
  - Signature
  - SigningCertURL
  - UnSubscribeURL
- SNS to send notification to mobile endpoints (whether its direct or subscription based, first we need to register the app with aws). To register mobile with AWS,
  - Enter name to represent your app
  - Select platform 
  - Provide your credentials for the notification service to platform
  - After registration, create an endpoint for the app and mobile device 
  - Then endpoint will be used in SNS to send the notifications.  
- If SNS distribute the messages to SQS then, below attributes should not be empty or null
  - Name 
  - Value 
  - Type 
  - MessageBody
- To receive messages published to topic, you have to subscribe to the endpoint of the topic
- Topic name 
  - Should be unique within aws account 
  - Limited to 256 characters 
  - Alphanumeric, -, _ are allowed 
- Subscription requests are valid for = 3 days for confirmation
- Once message published cannot be recalled
- CreatePlatformEndpoint API – to  register multiple device tokens 
- Platform supported
  - GCM – Google Cloud Messaging 
  - APNS – Apple push notification service
  - ADM – Amazon device messaging 
  - WNS – Windows Push notification service
  - MPNS – Microsoft Push notification service
  - Baidu cloud push – android in china 


# SWF
-  Workers
- Deciders
- They can run independently and scale quickly 
- SWF ensures task is assigned only once and never duplicated 
- Guarantees the delivery order
- SWF domains – isolate set of types, executions, tasks from others within aws account 
- Can register domain by console or registerdomain action in swf api
- Json format
- Maximum workflow can be one year, value always measured in seconds
- Decision task occur when the state of the workflow changes
- Ec2 can perform worker task 
- Server resides outside aws can perform EC2 task
- Humans can perform activity task not decision task 
- Max 100 SWF domains
- Max 10000 workflow and activity types (in total)
- Use case – Video encoding 
- Actors 
  - workflow starters 
  - deciders
  - activity workers - could be humans 

# SES
- Simple Email Service
- Email only 
- To send marketing, notification, and transactional emails 
- Can also use to receive emails. Automatically delivered to S3 bucket. Can trigger lamba and SNS
- Use cases – 
  - Automated emails 
  - purchase confirmations, shipping notifications, order status updates 
  - Marketing communications, advertisements, newsletters, special offers 
- Not subscription based. All you need to know is email address 

# Elastic Transcoder 

- Media transcoder in the cloud
- convert media files into different formats that will play on smartphone, tablet, pC etc
- supports all the popular output formats
- you dont need to guess, aws will take care of the format, configurations work best on particular devices 
- pay by mins that you transcode and resolution at which you transcode. 

# Kinesis 
- To send your streaming data
- used to consume big data 

- purchase from online store, stock prices, game data, social network data, IOT data, geo data (uber)
- Kinesis streams – 
  - Video streams – securely stream video from connected devices to AWS for analytics and machine learning
  - Data Streams – build custom applications to process data in real time
  - Producer will send the data to Streams (Producers – Ec2, mobile, laptop or dedicated server)
  - Stream will have shards 
  - In shards data can be retained for 24 hours (default )
  - Can change upto 7 days 
  - And then consumes will consume the data to analyses it (consumer – Ec2)
  - And send it S3 or dynamoDb or Redshit or EMR
- Kinesis Firehose 
  - Capture, transform, load data streams into aws data stores for near real time analytics with BI tools
  - No shards, no consumers, no retention period – don’t have manage. Automatic
  - Analyze the data within and automatically uses lambda 
  - Analyze is optional – its within firehose 
  - Producers will send the data to Firehose 
  - Firehose will send to S3 or Red shift
- Kinesis Analytics 
  - Allows you run sql queries on the data (sent to Firehose or Streams) and the result data can be stored in S3, Redshift 

# Elastic Bean Stalk 

- Java, .net, php, node, python, ruby, go and docker
- Apache tomcat, Nginx, passenger , Puma and IIS 
- Tomcat for Java
- Apache HTTP server for PHP & Python & Node js 
- Nginx also node js
- Passanger or puma for ruby apps 
- Microsoft IIS for .Net
- Will handle deployment, capacity provisioning, load balancing, auto scaling and application health 
- You still retail full control of underlying amazon resources 
- You pay only for the resources required to store and run your application 
- Integrated cloud watch and X-Ray 
- Deployment policies 
  - All the once
    - Deploys the new versions to all instances simultaneously 
    - All of your instances will be out of service during the deployment. Outage 
    - If the update fails, you need rollback by redeploying the previous version to all the instances 
    - Not suitable for critical prod environments
    - 
  - Rolling 
    - Deploys new version in batches 
    - Each batch of instances will be out of service during the deployment 
    - Your environment capacity will be reduced by the number of instances in a batch during the deployment
    - If the update fails, you need to perform additional rolling update to revert the changes 
    - Not suitable for performance sensitive systems 
  - Rolling with additional batch 
    - Launces additional batch of instances 
    - Deploys the new version in batches 
    - Maintains full capacity during the deployment 
    - No downtime
    - If the update fails, you need to perform additional rolling update to revert the changes 
  - Immutable 
    - Deploys the newer version to fresh group of instances in their own auto scaling group 
    - When the new instances passed the health checks, they are moved to existing auto scaling group and the old instances are terminated 
    - Full capacity during deployment and no down time 
    - If the update fails, just terminate the new instances 
    - Suitable for mission critical production systems 
  - Elastic beanstalk configuration
    - You can define packages to install 
    - Create linux users and groups 
    - Run shell commands
    - Enable service 
    - Configure load balancer 
    - Files are written in yaml or json 
    - Extension - .config 
    - Saved inside the folder .ebextensions 
    - Name can be anything
    - .ebextensions folder must be included in the top level directory of your app source code bundle 
- RDS can be integrated with EBS is two ways 
  - Can launch RDS instance with in EBS.  RDS instance will be created with in EBS environement 
    - Good option for Dev and Test 
    - If you terminate EBS, RDS instances also will be terminated. So not suitable for prod 
  - Launch RDS outside EBS and integrate with EBS
    - Decouple RDS and EBS
    - Additional security group must be added 
    - Need to provide connection string 
- Ec2, RDS, ELB, S3, SNS and Auto scaling group can be deployed in beanstalk (SQS – cant) 
- AWS toolkit for eclipse – to update running app in beanstalk 

# Code Commit

- Private GIT repository 
- Regional 
- Data is encrypted in transit and in rest (TLS)
- Events (pull, comment, commit comments) can be notified through SNS topic
- Connection type – SSH, HTTPS
- As roor cannot configure to connect with SSH
- To connect 
  - Install GIT 
  - Install AWS CLI
  - IAM user with CodeCommit policy 
  - Configure the user access key and secret access key in AWS CLI 
  - And GIT commands 
# Code Deploy 
- Automated deployment service 
- Allows you deploy the code automatically to EC2, on premises systems and lambda functions 
- Auto scales with your infrastructure 
- Integrates with various CI/CD tools, Jenkins, GitHub, Atlassian and AWS code pipeline 
- Also config management tools like Ansible, Puppet and Chef 
- Two deployment approaches, 
  - In-place 
    - Rolling update 
    - Deploys new version in batches 
    - Each batch of instances will be out of service during the deployment 
    - Your environment capacity will be reduced by the number of instances in a batch during the deployment
    - If the instances are behind the load balancer, configure load balancer to stop the traffic to the instances where the deployment is going on
    - It can only be used in EC2 and on-premise systems
    - Not supported for AWS lambda 
    - If the update fails, you need to perform additional rolling update to revert the changes 
  - Blue/Green 
    - Immutable 
    - Blue – active deployment
    - Green – New Release 
    - Deploys the newer version to fresh group of instances in their own auto scaling group 
    - When the new instances passed the health checks, they are moved to existing auto scaling group (elastic load balancer) nand the old instances are terminated 
- Deployment Group – Set of EC2 instances, lambda functions to which new version is to be deployed 
- Deployment – process 
- Deployment configuration – rules,  success / failure conditions 
- AppSpec File – defines the deployment actions you want AWS code deploy to execute. Must be placed root directory of your revision 
- Revision – everything needed to deploy
- Application 
- Lambda deployment – AppSpec file
  - AppSpec file – used to define the parameter will be used for codedeploy deployment – appspec.yml
  - For lambda deployment, AppSpec file – yaml or json 
  - Version – reserved for future release, currently only 0.0 is allowed 
  - Resources – name and properties of the lambda function to deploy 
    - Type
    - Properties 
- Name
- Alias
- CurrentVersion
- TargetVersion
  - Hooks – specifies lambda function to run at set points in the deployment lifecycle to validate the deployment 
    - BeforeAllowTraffic – used to specify the tasks or functions you want to run before allowing the traffic to your deployed lambda functions
    - AfterAllowTraffic – vice versa 
- EC2 / OnPremises deployment – AppSpec file
  - Supports only yaml	
  - Version – reserved for future release, currently only 0.0 is allowed
  - Os – windows or linux
  - Files – location of application files need to be copied and where to be copied, 
    - source 
    -  destination
  - Hooks – specifies scripts to run at set points in the deployment lifecycle to validate the deployment
    - BeforeInstall
- Location
- timeout
    - AfterInstall 
    - AppicationStart
    - ValidateService 
- Runas
- Hooks order of a code deploy – In place deployment 
  - 1 
    - BeforeBlockTraffic
    - BlockTraffic
    - AfterBlockTraffic
  - 2
    - ApplictionStop
    - DownloadBundle
    - BeforeInstall
    - Install
    - AfterInstall
    - ApplicationStart
    - ValidateService 
  - 3
    - BeforeAllowTraffic
    - Allowtraffic
    - AfterAllowTraffic

# Code Pipeline 
- Fully managed CI CD service 
- Pipeline workflow
- Can be integrated with code commit, code deploy etc 
- Workflow can be automatically triggered for every push
- Eg : user adds the code to S3  cloud watch trigger the code pipe line – code pipe line trigger code deploy  code deploy get the code from S3 and deploy to EC2
- Code pipeline can source the code from S3 or Code Commit or Git Hub – source provider
- Build provider – no build, Jenkins, AWS code build or Solano CI
- Deployment provider – no deployment, ECS, cloudformation, code deploy or elastic beanstalk 

#VPC Lab

- CreateVPC
- CIDR - 10.0.0.0/16 - biggest range
- it will create route table, NACL, Security group
- no subnet, no IGW
- Create Subnet 10.0.1.0-us-east-1a[AZ]
- CIDR block - 10.0.1.0/24 - will give 256 IP addresses 
- another Create Subnet 10.0.2.0-us-east-1b[AZ] - CIDR 10.0.2.0/24
- When you create subnet, y default they are private 
- Create IGW
- attch to VPC
- Route Table - Main route table wil be created with VPC
- Routes - By default Subnet's in VPC can talk to each other.  default rule in route table - local route.
- Subnet association - by default no subnet's will be associated 
- Main route table will not have access to internet by default 
- Create another  route table - for internet  access 
- Add route, along with local route, Destination : 0.0.0.0/0 and ::/0 and Target - IGW
- Now associate Subnet - this will be Public subnet now 
- By default auto assign public IP is NO for subnets. Even it has public route to internet.  Enable to Yes 
- Security groups exists only within the VPC. When you create EC2 in public subnet, you can select the security group with in the VPC 
- For Private EC2, create a security group with source as custom, 10.0.1.0/24 also add All ICMP protocol, 0-65535 port 
- To SSH to private EC2 from public EC2 you need to move private key pem file to your public EC2.  in real world use Bastion server. (NAT instance) 
- Nat instance - EC2 - Community AMI's 
- Choose public subnet 
- Security Group, SSH and HTTP, HTTPS 
- disable source / destination check 
- No need to login to NAT instance 
- add the route in route table which is assigned to private subnet which is default subnet here
- Destination : 0.0.0.0/0 and Terget is NAT instance
- if NAT instance is down, your private EC2 will not have internet access
- NAT Gateway
- choose public subnet 
- create Elastic IP address 
- Go to route table - default route table 
- add rule -  Destination : 0.0.0.0/0 and Terget is NAT gatway 
- NAT Gateways dont sit behind the security and its Highly availiable 
- NACL 
- open ephemeral port, Custom TCP rule,  1024 - 65535 - important to access from externally 
- you can block specfic IP 
- what is my IP
- add a deny rule 10.34.45.65/32  as source 
- Rules are validated in numerical order
- VPC endpoint
- With Nat GateWay, if you do AWS S3 ls from private EC2, it will list your buckets.  it has access to internet through NAT Getway 
- remove Nat GateWay route from private (default) route table 
- now AWS S3 ls will not work 
- Create NAT endpoint to make it work 
- Role for EC2 with S3 full access 
- assign this role to private Ec2 
- Create VPC endpoint
- Gateway 
- Interface 
- S3 Gateway
- select VPC 
- choose private subnet 
- 


# VPC
- Virtual private Cloud
- when you create VPC, it creates, Route table, security group and nacl
- By default 5 VPC’s can be created per region. For more need to contact amazon 
- Public subnet 
- Private subnet 
- When we create custom subnet, by default they can talk to each other across AZ
- VPC Subnet can have multiple route tables
- 1 subnet = 1 AZ
- Internet Gate way -  only one internet gateway per vpc 
- Virtual private gateway – VPN 
- Route Table 
- Security Group – it exists only with in the VPC, its attached with VPC
- ACL – Access control List 
- NACL, SNACL – Subnet Network ACL
- Security Groups – Stateful (if you add inbound rule, automatically outbound is added)
- NACL – StateLess (need to add both inbound and outbound rule)
- Default VPC
  - All subnets in default vpc have route to internet 
  - Each EC2 will have private / public ip address.  Custom VPC – EC2 have only private ip
- Can have multiple VPC’s in one account 
- http://cidr.xyz/
- VPC peering 
  - Allows you to connect one VPC to other via direct network route using private ip address
  - Can peer VPC’s in other aws accounts also within single region 
  - now VPC peering can be done in multiple region (updated)
  - Peering is star configuration, 1 central vpc peers with 4 others 
  - No transitive peering – all 4 vpc peers thorough centeral vpc.  4 vpc will not not peer directly 
  - you cant connect if the if vpc's are peered and they are in same CIDR IP range (or)
  - you cant create VPC peering between VPC's whcih has matching or overlapping CIDR blocks
- AWS reserves 5 ip addresses in a subnet. First 4 and last one in each subnet CIDR block
  - For subnet with CIDR block 10.0.0.0/24 following 5 are reserved
  - 10.0.0.0 – Network Address
  - 10.0.0.1 – VPC router
  - 10.0.0.2 - DNS
  - 10.0.0.3 – for future 
  - 10.0.0.255 – network broadcast 
- For public subnet – enable auto assign public ip address to yes
- IGW – internet gateway can be attached with only one VPC – always
- Route Table – attach IGW with IPv4 and IPv6 for internet access – for public subnet
- IPv4 – 0.0.0.0/0
- IPv6 - ::0/0
- For private subnet, attach NAT instance or NAT GateWay In route table
- NAT – Network Address Translation 
- Baston server – jump server
- NAT instance 
  - EC2 instance search NAT
  - by default EC2 will check source and destination. Disable for NAT instance
  - you have to manage 
  - use script to manage failovers
  - attached with security group 
  - must be in public subnet
- NAT Gateway – Ipv4
  - In VPC, create NAT GateWay and attach public subnet
  - Highly available
  - Create in each AZ 
  - AWS managed  
  - Security group not required 
  - Scale automatically 
  - More secure 
  - use ports from 1024 to 65535
- Egress only internet Gateways – ipv6
- NACL 
  - Subnet can be associated with one NACL always. But NACL can have multiple subnets 
  - If a subnet is associated new another NACL 2, existing association with NACL 1 will be automatically removed
  - NACL can be attached with one VPC	
  - Default NACL will be created with VPC – by default it  allows all traffic 
  - Custom  NACL (when you create) – by default it deny all traffic 
  - Each subnet in VPC must be associated with NACL.  If not subnet will be associated with default NACL
  - Rule 100 – ipv4
  - Rule 101 – ipv6 
  - Amazon recommends to start rule from 100
  - StateLess (need to add both inbound and outbound rule)
  - Rules are validated in numerical order.  If rule 99 is deny and rule 100 is allows, as per 99 traffic will be denied 
  - Can add a rule to deny traffic from particular IP range. Can’t do with security groups 
- VPC flow logs 
  - Capture information about the IP traffic going to and from – VPC 
  - Stored in cloud watch logs 
  - Can be created at 3 levels 
    - VPC 
    - SubNet
    - Network interface level
  - You cannot enable flow log for VPC’s that are peered with your VPC’s unless peer VPC is in your account
  - You cannot tag a flow log
  - After created the flow log, cannot change the configuration. (cannot change the role) 
  - Not all IP traffic is monitored. ( below will not be monitored)
    - Traffic generated by your instances when they contact amazon DNS server. If you use your own DNS server, it will be monitored 
    - Traffic generated by windows instance for amazon windows license activation
    - Traffic to and from 169.254.169.256 meta data
    - DHCP traffic
- VPC endpoint – to access AWS resources via virtual private gateway (IGW not required)
  - If you want to access S3 from private EC2, you can access through NAT GateWay which is in public subnet. NAT Gateway communitcate with S3 over internet
  - VPC endpoint helps you communicate with S3 internally through internal Gateway. No internet
  - Interface
  - Gateway
- If you want to multiple apps (different ip’s) on a single EC2
  - Launch a vpc instance with 2 network groups 
  - Assign elastic IP 
  - Assign separate security groups
- NATGatway cannot send traffic over VPC endpoints, VPN connections, AWS direct connect or VPC peering. 
- Internet Gatways are two way traffic. NAT Gatway is not
- For any route table local route cannot be edited or deleted 
- if both NAT Gateway and VPC endpoint is associated with same route table, VPC endpoint always takes the precedence. 
- VPC endpoints doesnt support cross region S3 requests 
- if route table has both NAT gateway and VPC endpoint route, if you want to access S3 from private Ec2, NAT gateway always takes the precedence. it will not communicate via internet which NAT Geteway.  But if the S3 in the different region then communication will be over NAT Gateway. since VPC endpoints doesnt support cross region S3 requests
- in VPC endpoint, we can add a policy to restrict access to certain S3 bucket and certain actions. by default policy allows all actions. 
- In NACL if SSH allowed in inbound and SSH denied in outbound and ephemeral allowed in outbound - SSH will work 
- when you SSH to EC2, inbound port is 22 and outbound port in ephemeral
- Nat Gatway should be associated with public subnet which has IGW 
- Nat Gatway cannot be created without elastic IP 
- NACL - should allow outbound traffic to all ports or ephemeral ports or specific protocol to allow traffic 
- VPC gateway endpoints not supported outside VPC
- By default security groups allows all outbound traffic. but you can change the ruke to allow traffic on specific protocols 
- In VPC peering, you cant use NAT GateWay created in one VPC in another VPC. using NAT Gateway in another VPC becomes transitive routing and its not allowed in AWS
- with custom VPC, dy default DNS hostnames are disabled. This can be enabled from VPC actions
- once the VPC and subnets are created, CIDR range cannot be edited 
- if all the IP address in CIDR range is use, need to use more 
  - Create another VPC and peer with old VPC - complex approach 
  - Add secondary CIDR range for VPC and create subnet with the new range 
- Custom route table can be changed to as main route table
- VPC peering beteen VPC A and VPC B, VPC A should have route table route to VPC B as destination and VPC B should have route table route to VPC A as destination
- For VPC peering - route will contain target as pcx-xxxxxx
- For direct connect and VPN - route will contain target as pcx-xxxxxx as vgw-xxxxxxx
- For secondary CIDR range - route will contain target as ipv4 address (20.0.0.0/32)
- x.x.x.x/16 - 65, 536 IP addresses 
- x.x.x.x/24 - 256 IP addresses 
- x.x.x.x/32 - 1 IP address
- To connect on primeses to AWS VPC via AWS VPN, you need
  - Hardware compatible VPN device 
  - Virtual private Gateway 
  - Direct connect is not for VPN
  
# CloudFormation

-  Template – template 
   - Description Declaration
   - Format Version Declaration 
   - Parameters
   - Resources 
   - Mappings 
   - Outputs
- Stack – provisioned 
- Json and yaml 
- Mandatory fields – aws resource 
- Options fields 
  -  input parameters (eg : tags), limit is 60 
  - Output parameters (eg:public ip), limit is 60 
  - Version – latest template version is 2010-09-09. If you don’t specify, aws will assume the latest version
- Fn:GetAtt – to output data 
- Fn::join – appends set of value into single value separated by delimiter, id delimiter is empty, just concat
- Fn::FindInMap – returns the value corresponding to keys in a two level map which is declared in mappings section
- Fn:Select – returns single object from a list of objects by index
- By default, “automatic rollback on error” is enabled 
- You are charged for errors 
- Cloud formation is free. You need to pay only for resources 
- Waitcondition  - stacks can wait for apps to be provisioned
- Route53, IAM role is supported
- You can create A records and aliases  
- Templates – No limit 
- Stacks per account – 200
- Stack can be increased beyond 200 by contacting AWS
- ListStackResources – API to list all the resources in a stack
- ListStacks – list all the stacks including deleted (90 days)
- DescribeStacks – lists only running Stacks 
- For deleted Stacks, ListStackResources will returns upto 90 days from the day deleted
- Provides python helper scripts – to install software, start service in EC2. You can call the helper script directly from your template. This will be executed in Ec2 as part of stack creation process 
- Can be used to bootstrap Chef and Puppet
- Data can be saved before deleting the stack by defining delete policy 

# Route 53
- Global service
- IPv4 – 32 bit 
- IPv6 – 128 bit, 340 undecillion addresses 
- supports MX records
- default limit is 50 domain names. but can be increased by contacting AWS
- Domain Registrars
  - Names are registered with InterNIC - a service of ICANN. They enforce the uniqueness.
  - Route53 isn't free, but domain registrars include things like GoDaddy.com etc.
- NS records – Name Server records
- A record – Address record – used by computer to translate the name of the domain to the IP address. 
- ELB always use DNSName – no ip
- TTL – Time to live – your computer always cashes the ipv4 address of domain name. by default TTL is 2 days.  So everytime you hit DNS, it will search in local cache, and then does dns look up and update the cache. If you planning DNS migration, drop the ttl to 5 mins, so that your computer refresh the cache for every 5 mins
  - Length that the DNS is cached on either the Resolving Server or on your PC
- SOA Records
  - supplies name of the server
  - admin of the zone
  - current version of the data file
  - number of seconds a server should wait before retrying a failed zone
  - Default number of seconds for TTL on resource records
- NS Records
  - Name Server Records
  - used by Top Level Domains to direct traffic to the Content DNS servers which contains the authoritative DNS records.
- A Records
  - Address record - used to translate from a domain name to the IP address. A records are always IPv4. IPv6 is AAA.
- CName – Canonical Name – used to resolve one domain name to another. You can use mobile.aacloud.com to m.aacloud.com  so users can use both and its points to same dns 
- Alias records 
  - Alias is free
  - Similar to CName 
  - Diff is you cant use Cname for naked domain names 
  - Naked domain name – zone apex record, no www, http://acloud.guru 
  - CName is chargeable 
  - You can't have a CNAME for acloud.guru. It must be either an A record or an Alias.
  - The naked domain name MUST always be an A record, not a C name. eg dennis.com.
  - The Alias will map this A record to an ELB.
- Always choose alias records over CName
- Routing Policy 
  - Simple
  - Weighted – 
    - let you to split the traffic, 
    - 95% traffic to region 1 and 5 % traffic to region 2 or 2 different ELB’s 
    - Need to provide weightage  1- 255
    - 
  - Latency – 
    - let you send traffic to region which has less latency (region near to user) t. 
    - Eg 1 elb in india and one in London. If users access from india, latency routing take him to india elb since London elb will have more latency
    - Need to select region
  - Failover 
    - Let you to create active passive setup
    - If active region health check fails traffic to passive region
    - Need to add health check of your target (ELB)
  - Geolocation
    - Let you to send traffic based on the geolocation of the user 
    - Eg: EU customers will be sent to London and US customers will be sent to Mumbai 
    - You need to select location.  Continent or Country or state or default (everywhere else) 
  - GeoProximity 
    - route traffic based on the location of your resouce and can shift traffic from resources in one location to resources in anothetr 
  - MultiValue answer 
    - when you want Route53 to respond to DNS queries with upto eight healty records selected in random 
- Supports SSL termination – all regions 
- New instances can be added on the fly. No need to stop ELB
- Global 
- 3 main functions 
  - Register domain names
  - route internet traffic to the resources for your domain 
  - check health of your resources 	(can send notifications if the resource is unavailiable) 
- To access S3 static website through Route 53 -> A IPv4 address with Alias = Yes
- To route traffic to ELB through Route 53 -> A IPv4 address with Alias = Yes
- To route traffic to RDS through Route 53 -> CNAME - Canonical with Alias = Yes
- Can route traffic to 
  - CloudFront 
  - Ec2
  - Elastic BeanStalk 
  - ELB
  - RDS
  - S3 
  - Amazon work mail 
- If Route53 couldnt reach your resouce and the browser says "Server Not Found" 
  - you didnt create a record for the doamin or sub domain name 
  - you created the record but specified the wrong value  (like wrong IP) 
  - resurce that you are routing is not avaliable 
- Health checks
  - monitor endpoint 
  - monitor Cloudwatch alarm 
  - monitor other health cheks
  
  


# AutoScaling 
- Free of cost 
- Can autoscale
  - EC2 autoscaling groups
  - Aurora DB clusters 
  - DynamoDB tables
  - DynamoDB global secondary index
  - ECS
  - Spot fleet requests
- Ways
  - Ec2 auto scaling 
  - Application auto scaling api
- EC2 Auto Scaling can also detect when an instance is unhealthy, terminate it, and launch an instance to replace it.
- Components required to setup effectively 
  - Launch configuration
  - ELB
  - Auto Scaling 
- Launch configuration cannot be edited once its created
- if you want to update launch configuration, you can use existing launch configuration as base, create new one, and update the new launch configuration in a auto scaling group. 
- Scaling default metict types
  - Average CPU Utilization
  - Network In
  - Network Out 
  - Application ELB request count per target
- Scalling based on Memory (RAM) - custom metric 
- Health check grace period - even if the EC2 is unhealthy, auto scaling will not act until the health check grace period expires 
- Termination Policy 
  - Default - useful if you have more then one scaling policy for the group 
  - Oldest Instance - useful if you are upgrading the instances in the auto scaling group. This will be helpful to get rid of old instance types
  - Newest instance - useful if you want to test your new launch configuration 
  - Oldest launch configuration - useful if you have updated the launch configuration 
  - ClosestToNextInstanceHour - terminates the instances that are closest to next billing hour. helps to manage EC2 usage costs
- Scaling policy 
  - Simple scaling 
  - Step Scaling 
  - Target tracking scaling 
- Default Termination poilcy 
  - If multiple AZ, choose AZ which has more instances
  - select the instance which has oldest launch configuration
  - if more then 1 instace with oldest launch configuration, then select the instance which is close to next billing hour 
  - if more then one then select random 


# Placement Group
- Placement group nane is unique per AWS account
- AWS supports, hemogenous instances with in placement group. Same family, storage, size
- You cant merge placement groups
- you cant move existing instance into placement group
- you can create AMI from existing instance, launch new instance from AMI into placement group
- Two types
  - Clustered placement group
    - grouping of instances within single AZ
	- recommended for applications that require low network latency or high network throughput or both 
	- Big data, you dont want to spread
	- in exam, by default clustered 
	- only certain instances can be launched.  you cant launch t2 micro
	- Instance families are compute optimized, GPU, memory optimized, storage optimized
	- cant span mutiple AZ
  - Spread placement group
    - grouping of instances that are each placed on distinct underlying hardware
	- recommended for applications that have small number of critical instances that should be kept seperate from each other. 
	- can span mutiple AZ

# Shared Responsibility 
 - AWS 
   - Responsible for security 'of' the cloud
   - Decommissioning Storage devices  
   -	Securing physical access to AWS resources 
   -	Virtualization infrastructure 
 - Customer 
   - Responsible for security 'in' the cloud
   - Security group & ACL settings
   - Patch management on EC2 instances
   - Life cycle management if IAM credentials
   - Encryption of EBS volumes 
   - OS, network, firewall configuration 
   - Customer Data 
   - Server side encryption
   
   
# CloudWatch
- Basic monitoring - 5 mins - default
- Detailed monitoring - 1 min
- Dashboards
- Widget
  - Text
  - Line 
  - Stacked Area
  - Number - current cpu utilization
- Default metric for EC2 
  - CPU related
  - Disk related
  - Network related
  - Status check related
  - No RAM related - custom metric 
- Alarm
  - create topic, notification list and confirm subscription 
- Events
  - event triggers lambda function to update DNS with public ip when EC2 becomes running state from stop
- Logs
  - Install agent, monitor and access 
- Metrics

# OpsWorks 
- Orchestration service that uses chef
- chef consists of 'receips' to maintain a consistent state 
- cook book 

# Security 

- Design Principle 
  - Apply security at all layers
  - Enable traceability 
  - Automate responses to security events
  - Foucus on securing your system - Responsibility
  - Automate security best practice
- Definition
  - consists of 4 areas 
  - Data Protection
    - Organise and classify your data (public, only employees, only MD)
	- least privilege access system (peoples are only able to access what they need)
	- encrypt everything possible (transit and rest)
	- ELB, EBS, S3 and RDS
  - Privilege management
    - Only authorized and authenticated users are able to access your resources 
	- Access control list 
	- Role based access control 
	- Password management
	- IAM, MFA
  - Infrastructure protection
    - How you protect your VPC, security group, NACL, private subnet etc
  - Detective controls 
    - Detect or identity securtiy breach 
	- CloudTrail, CloudWatch, AWS config
	
# Reliability 

- Ability of a system recover from service or infrastructure outage and autoscale
- Design Principle 
  - Test recovery procedures 
  - Automatically recover from failures 
  - Scale horizontally to increase aggregate system availability
  - Stop guessing capacity   
- Definition
  - consists of 3 areas
  - Foundations - IAM, VPC
  - Change Management - Cloud Trail 
  - Failure Management  - CloudFormation
  
# Performance Efficiency 
- Design Principle 
  - Democratize advanced techonologies 
  - Go global in mins
  - Use serverless architecture 
  - Experiment more often
- Definition
  - Consists of 4 areas
  - Compute - Autoscaling 
  - Storage - EBS, S3, Glacier
  - Database - RDS, DynamoDB, RedShift
  - Space - time trade off - CloudFront, Elasticache, Direct connect, RDS read replicas 
  
# Cost Optimization 
- Design Principle 
  - Transparently attribute expenditure 
  - Use managed services to reduce cost of ownership 
  - Trade capital expense for operatiing expense 
  - Benefit from economies scale 
  - Stop spending money on data centers and operations
- Definition
  - Consists of 4 areas
  - Matched supply and demand  - Autoscaling 
  - Cost effective resources - EC2 (reserved), trusted advicer
  - Expenditure awareness - cloud watch alarms, SNS
  - Optimizing overtime - AWS blog, trusted advicer
  
# Operational Excellence 
- Design Principle 
  - Perform operations with code
  - Align operations processess to business objectives 
  - Make regular, small, incremental changes 
  - Test for responses to unexpected events 
  - Learn from operational events and failures 
  - Keep operations procedures current 
- Definition
  - Consists of 3 areas
  - Preparation
    - Runbook
	- Playbook 
	- Tagging the resources 
	- CloudFormation, Auto Scaling, AWS config, Service catalog, SQS
  - Operation - Code Commit, Code Deploy, Code Pipeline, AWS SDK's, Cloud Trail 
  - Response  - CloudWatch, alarms, SNS 
  
# BigData
- To consume bigdata - Kinesis 
- For business inteligence - RedShift 
- For BigData processing - Elastic Map Reduce 

# Consolidated Billing 
- AWS Organization 
  - account management service enables you consolidate multiple aws accounts 
  - Centrally manage policies across mutiple aws accounts
  - Control access to AWS services 
    - you can create Service Control policies (SCP). Eg. you can deny your HR group to access Kinesis or DynamoDB
	- Even IAM in the account allows it, SCP will override it
  - Automate AWS account creation
  - Consolidate billing accross multiple AWS accounts
  - Two Features
    - Consolidated Billing 
    - All Features 
- Single payment method for all the AWS accounts 
- Paying account only for billing purpose only. do not deploy servcices in paying account 
- Linked accounts - 20 is the limit 
- Can do billing alerts consolidated and individual 
- consolidated billing on the linked accounts 
- Consolidated billing allows you to get volume discounts on all your accounts. Eg.  S3 storage more you use less you pay. so when you consolidate storaged used will save your moneny. 
- Unused reserved Ec2 instances are applied across the accounts. this will save your cost.
- CloudTrail 
  - per AWS account and is enabled per region 
  - Can consolidate logs using s3 bucket
    - Turn on cloudtrail on the paying account 
	- create bucket policy to enable CORS
	- Turn on cloud trail in linked accounts and use the bucket in the paying account 
	
# Cross Account Access 
- Can switch users without login

# Tags 
- Key pair value 
- Meta Data 
- Case sensitive 

# Resource Groups 
- group your resources using tags 
- Can contain Region, Name, Health checks etc
- For EC2 - public and private IP addresses 
- For ELB - port configurations 
- for RDS - DB engine 
- 2 types
  - Classic resource groups
    - Global 
  - AWS system manager
    - Per region based 
  
# Direct Connect 
- Dedicated network connection from your premises to AWS	
- Reduce costs when using large volumes of traffic 
- Increase reliability 
- Increase bandwidth
- for immediate need go for VPN since it can done in few mins (also if you want to encrypt your traffic)
- VPN goes over internet where as Direct Connect is Intranet
- Direct Connect is not a site-to-site VPN

# Workspaces

- VDI 
- cloud based replacement for a traditional desktop 
- Windows 7 experiance provided by windows server 2008 R2
- by default users can personalise their workspace (wallpaper, icons, shortcuts).  This can be locked by administrater by adding policy 
- By default you will be given local admin access, so you can install your own applications
- Persistant 
- All the data in D:\ is backed up every 12 hours 
- you dont need AWS accout to login to workspaces

# ECS
- Amazon EC2 Container Service 
- Container management service that makes it easy to run, stop and manage docker containers on a cluster of EC2 instances. 
- Docker Components
  - Docker Image - Containers are created from a read only template called Image which has instructions to create container 
  - Docker Container 
  - Layers / Union file system 
  - DockerFile 
  - Docker Daemon / Engine 
  - Docker Client
  - Docker Registries -  Docker Hub or ECR
- Container and Images 
- Task definition 
  - to run docker containers in ECS
  - test files in json format 
  - can have 
    - docker image name, 
	- cpu, memory to use 
	- networking mode to use
	- ports 
	- IAM role
	- data volumes
	- environment variables
	- init task 
- ECS Clusters 
  - logical grouping of container instances
  - with first ECS service, default cluster will be created. 
  - Can create multiple clusters 
  - Can contain multiple different container instance types
  - region specific 
  - Container instances can be part of 1 container at a time 
  - can use IAM policies to restrict/ allows users to aceess specific clusters 
- ECS Scheduling 
  - Service Scheduler 
  - Custom Scheduler - you can create your own scheduler or use third party like blox 
- ECS Container Agent 
  - This agent allows container instances to connect your cluster. 
  - supported only on EC2 instances that supports ECS specification 
  - Linux based
  - works with Amazon linux, Ubuntu, Red Hat, CentOs etc 
  - Will not work with Windows 
- ECS Security 
  - IAM roles- to restrict/allow aceess 
  - Security group - at the instance level. not at the tasks or container level 
- Customers will have full control over ECS. with root access can install third party apps   
- Container instance need external network access to communicate with ECS service endpoint. so if your container instances doest have public IP, they you muse use NAT Gatway for external network access (internet) 
- For ECS agents to communicate with ECS cluster
  - IAM role used to run ECS instance should have ecs:poll action in its policy 
  - security groups should allow traffic to ECS service endpoint
- ECS container has no password to use for SSH access, use key pair to login  to your instance securely. you will specify name of key pair when you launch your container service, then provide the private when you login using SSH. 
- ECS launch types
  - Fargate launch type 
  - EC2 launch type 
- Service definition 
  - Defines which task definition to use with your service
  - How many instantiations of that task to run
  - which load balancers associated with the tasks
  - cluster on which run your service 
  - IAM role that allows ECS to call your load balancer 
- To set ECS container agent configuration during ECS instance launch - Set configuration in user data parameter  of ECS instance

	
  
# ECR
- Amazon EC2 Container Registry 
- like DockerHub
- Docker image repository

# Acronym 

- IOPS – Input Output per Second
- SSD – Slot State Drive 
- AMI – Amazon Machine Instance
- HVM – Hardware Virtual Machine (X – Para virtualization)
- NFSv4 – Network File System V4 
- OTLP – Online Transaction Processing 
- OLAP – Online Analytics Processing 
- SAML – Security Assertion Markup language 
- CORS – Cross Origin Resource Sharing 
- RTMP – Real Time Messaging Protocol 
- HSM – Hardware Security Module ??
- API – application programming interface 
- rps - requests per second
- DAX - Dynamo DB Accelerator 
- NACL – Network Access control List 
- CIDR - Classless Inter-Domain Routing
- NAT – Network Address Translation 

# Limits
- EC2
  - 5 elastic ip address
  - uptime SLA for EC2 and EBS - 99.95
  - 20 EC2 instances per region (depends on the family).  New accounts may start with lower limit.  Can be increased by contacting AWS
- S3 
  - 100 buckets per account – can increase by contacting AWS
  - No limit 
  - 1 object – 0 bytes t0 5 TB
  - Single put max size is 5 GB
  - Amazon recommends Multipart upload for more then 100 MB
  - S3 standared - 99.99% Availiabilty, 99.999999999 Durability
  - S3 IA- 99.99% Availiabilty, 99.999999999 Durability
  - S3 onezone IA - 99.5% Availiabilty, 99.999999999 Durability
  - S3 RRS - 99.99% Availiabilty, 99.99 Durability
- DynamoDB
  - 256 tables per region – can increase by contacting AWS
  - 5 local secondary index – cannot increase secondary index (both local & global) 
  - 5 global secondary index (so total is 10) 
  - Max limit of item collection is 10 GB
  - Smallest amount of capacity unit can be purchased is 100 (both reads and writes)
  - Max size of item in dynamoDB = 400 kb
  - Number if attributes item can have = no limit, but total size including attribute names and values should not exclude 400 KB
  - Result set from a scan per call is limited to 1 MB, use LastEvaluateKey to reterive more results
  - Capacity unit calculation, unless its mentioned - Strongly consistent 
  - can support maximum of 3000 read capacity units and 1000 write capacity units 
  - Max length of sort key value - 1024 bytes
  - Max length of sort key value - 2048 bytes
  
- SWF
  - Max 100 SWF domains
  - Max 10000 workflow and activity types (in total)
  - SWF workflow can live upto 1 year
  - Maximum open activity tasks - 1000
- SNS
  - Topic name 
    - Should be unique within aws account 
    - Limited to 256 characters 
    - Alphanumeric, -, _ are allowed 
  - Subscription requests are valid for = 3 days for confirmation
  - 100,000 topic per account 
  - 10 million subscription per topic – contact aws for more for both
- SQS
  - No limit 
  - 1 million request per month – free tier  
  - Then 0.50$ for every million requests
  - Message size, 1 kb to Max 240 KB
  - Retention – 1 min to 14 days 
  - Default retention – 4 days 
  - Visibility time out 30 seconds to 12 hours 
  - Long poling max – 0 to 20 seconds 
- CloudFormation
  - Templates – No limit 
  - Stacks per account – 200 – can increase by contacting AWS
  - 60 parameters and 60 outputs in a template
- ELB 
  - No cost 
  - 200 subnets per vpc – call aws for more
- Route53
  - Default limit is 50 domain names. but can be increased by contacting AWS
  
# Tips
- Stateless Services - RDS, DynamoDB, Elasticache
- Stateful services - ELB 
- High Availability services - DynamoDB, S3, SQS (data automatically replicated in multiple AZ)
- RDS - customer has the setup high availability - Multi AZ and EC2 by auto scalling 
  
  
VPC - 2, 5, 6, 10, 11
S3 - 17