# AWS CSA Pro

# Architecture Pillars and Principles

- Business Continuity and Disaster Recovery
  - Resiliency and Fault Tolerance
  - Redundancy and High Availability 
- Cost
- Performance 
- Security 
- Monitoring 
- Scalability and Elasticity
- Ease of Deployment 

# Building Fault Tolerant Apps on AWS - WhitePaper

- Fault Tolerance 
  - Ability of System remain operational even if some components are build to fail. 
  
- AMI 
  - AMI with patches, application everything required to be operational
  - Launch the prebuild AMI in case if any failures to be fault Tolerant 
  - To minimize the down time, always keep the spare instance running. take over in case of failure - cost 
    - Cost
	- Can be done effectively using elastic ip (Floating IP)
	
- EBS 
  - Block level storage 
  - For DB, file system etc 
  - Snapshots can be stored in S3 
  - must stop I/O and flush in memory data to disk before taking snapshot 
  - EBS volumes stores data redundantly and durable 
  - Snapshots can be copied to other region to restore 

- AutoScaling 
  - With CloudWatch you can up or down EC2 capacity 
  - Automatically detect failures and launch replacement instances 
  - We can launch fresh instances for regular interval to handle memory leaks etc (Setting expiry date on your instances) 

- Elastic Load Balancer 
  - Distributes the incoming traffic
  - Detect the unhealthy EC2 instances and divert the traffic only to healthy instances 
  - Bound to Region 
  - Distributes the traffic between AZ within the region 
  - For Regions use Route53
  
- Region and AZ
  - To achieve greater fault tolerance, distribute your application geographically 
  - So one AZ or one Region is down, operation will not be impacted (from other AZ or other Region)
  - EC2 SLA commitment is 99.95% availability for each EC2 region. 
  - Auto scaling works across multiple AZ within region.   
  
- S3
  - Highly durable, Fault tolerant object storage 
  - S3 stores objects redundantly on multiple devices across multiple data centers 

- RDS 
  - Automated backups 
    - point in time recovery 
	- data backed up from transaction logs 
	- if the automated backup completed by 3 PM and DB crashed at 5 PM, can recover the data till 4.55 PM (5 mins)
	- Enabled by default 
  - Manual Backups 
    - Full DB backups manually 
	- for backup 
  - Multi-AZ
    - For DR 
	- Data synchronously replicated in a different AZ
	- Standby DB 
	- in case if the primary is down, stand by is promoted as primary 
  - Read Replica 

# Using AWS for Disaster Recovery - WhitePaper  
- Disaster Recovery
  - All about preparing and recovering from disaster 
  - AWS recommendation is keep your both primary and DR env with AWS platform 
    - equal contract   
	- same underlying AWS technologies 
	- Same tools and API's 
  - Counter statement is also valid  (AWS and AZURE)
    - Not to lock with same vender 
	
- RTO 
  - Recovery Time Objective
  - After Disaster, The time it takes to restore a business process to its service level defined by the Operational Level Agreement (OLA)
  - For Example, if Disaster occurs at 12 PM and RTO is 8 hours then business process should be restored by 8 PM  
  
- RPO 
  - Recovery Point Objective 
  - The acceptable amount of Data loss measured in time 
  - For Example, if Disaster occurs at 12 PM and RTO is 1 hour, then System to should recover all the data that was with the system upto 11 AM
  - its ok to lose the data between 11 AM to 12 PM as per RTO
  
- EC2

- S3
  - AWS service for backup and DR
  - Highly Durable (11 nines)

- Glacier 
  - AWS service for backup and DR
  - Backup (archival storage)  
  - Low cost storage 
  - Retrieval time is several hours 
  - Same Durable as S3

- EBS 
  - AWS service for backup and DR
  - Point in time snapshots of Volumes 
  
- Storage Gateway
  - Data stored in On Perm and backed up to AWS 
  - Replicates data from your own data center (on premise) to AWS. You install it as a host on your data center.
  - File Gateway 
    - NFS 
	- users can upload or retrieve the object data into OnPerm Server via NFS
	- Object data backed up to S3
	- Data can be accessed via S3 console since its object data 
  - Volume Gateway 
    - iSCSI protocol 
	- block based 
	- Disk Volumes 
	- Stores into S3 in the form of EBS snapshots
	- its block based. so cant store / access from S3 directly
	- Stored Volumes
	  - Storage mode 
	  - Entire data is stored in on site (on prime) and SG then backups this data up asynchronously to Amazon S3 (as EBS snapshot).
    - Cached Volumes
	  - Cached mode 
	  - Only your most frequently accessed data is stored in on prime and Your entire data set is stored in S3 (as EBS snapshot).
  - Tape Gateway 
    - iSCSI 
    - VTL
	- Virtual Tape Library 
	- Used for backup
	- Backing up your Tape Library to AWS S3 - VTL
    - if its moved to Glacier then its called - Virtual Tape Shelf - VTS
  - DR use case 
    - in case volume gateway on on-perm is crashed.  it can be restored by EBS Snapshot in S3 --> EBS Volume --> Attach to EC2 instance 
	
- Snowball
  - To transfer your data from OnPerm to AWS and vice versa
  - Physically like USB drive 
  - transfer Tera Byte data 
  - if the data transfer takes more then 1 week then use Snowball
  
- AWS import / Export Disk 
  - you can use your own external disks 
  
- AWS VM import / Export
  - Import your own Virtual Machine Images from your existing environment into Amazon EC2 instance 
  - Upload your VM Image into S3 and use VM import tool to import as EC2 into AWS 
  - Ec2 instance which was imported using VM import tool can be exported as Virtual Machine image for external use 
  - but AWS own EC2 instances cant be exported as VM image 
  - no charge for VM import / Export tool beyond and standard usage of Ec2 and S3
  



	
	
  
	
	