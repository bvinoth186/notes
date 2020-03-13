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
    - Not to lock with same vendor 
	
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
	- EBS snapshots can be restored as either EBS volume and attached to EC2 or ICASI transfer via another storage gateway 
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
  
- Route 53
  - HA, Cost effective 
  - Health check 
  - ability to fail over multiple endpoints
  
- Elastic IP address 
  - static ip's  
  - public ip address are dynamic. (restarts of instance would give you different ip)
  - Elastic IP address, mask instance or AZ failure by programmatically remapping your public IP addresses to instances in your account in a region
  
- Elastic Load Balancer 
  - Region based
  - can distribute the traffic within AZ or across AZ. (for across Region - Route 53)
  
-----------------------  
- DR Approaches 
  - Backup and Restore (Slow, Cheap)
  - Pilot lite
  - Warm Standby 
  - Multi site (Fast, Pricey)
  
- Backup and Restore
  - S3
    - Over Internet
	- Direct Connect 
	- AWS Import / Export 
	- Snapshots of EBS
	- RDS DB (EBS Snapshots)
	- Redshift (snapshots)
  - Glacier
  - Storage Gateway 

- Pilot Lite 
  - Minimal version of core components always running in DR and continuously up-to date. 
  - for Data - Data mirroring from primary to pilot lite 
  - During DR situation, rapidly provision the full scale production environment around the critical core. 
  - For Restore 
    - Pre configured AMI's 
	- Elastic IP address 
	- ELB
	- EBS snapshots using backup and restore method
	- Resize the Database to handle traffic and data (with RDS you can increase on the fly but decreasing not possible)
  - quicker recovery then Backup and Restore 
  - Costlier then backup and restore 
  - Regularly test and keep the data up-to date
  - automate the provisioning of AWS services 
  
- Warm Standby 
  - Extends the pilot version
  - Scaled down version of full functional primary environment always running 
  - Data replication up-to date 
  - can run with minimum fleet of EC2 with smallest sizes possible since there is no traffic 
  - Can be used for non prod work like pre prod, testing, internal use until DR 
  - Create and maintain AMI's 
  - Patch and update software and keep in sync with primary 
  - Recovery 
    - Rescale 
	- load balancer split the traffic to the new instances 
	- Route 53 to route the traffic
	- can resize to larger EC2 instances 
	- scale up RDS DB

- Multi Site
  - Active Active
  - Route 53 weight age policy 

- Replication 
  - distance - larger distance would have high latency 
  - bandwidth 
  - data rate required by application (should be less then the bandwidth)
  - replication technology (should be in parallel)
  - Synchronous replication
    - data is automically updated in multiple locations
	- depends on network performance and availability
  - Asynchronous replication
    - eventual consistency

- Self Healing 
  - SQS to decouple
  - Cloud watch and Auto scaling terminates unhealthy instances
  - Auto scaling replaces terminated instances 
  - S3 / Glacier performs systematic integrity checks (automatic self healing)
 
# Organizational complexity and Cost control  

- AWS Billing and Cost Management
  - service used to pay bill, monitor usage and budget usage costs 
  - payer account 
  - consolidated billing 
  - aggregated usage across the accounts(cost saving)
  - month begin usage reset to zero (aws charges monthly)
  - aws support is per individual account 
  - aws limits are per individual account (not master account or consolidated)
  - one detailed bill for multiple accounts
  - consolidated billing is free
  
- AWS Cost Explorer 
  - free service 
  - cost data as graph 
  - filter by AZ, region, tag, aws resource etc
  - filter by account (consolidated billing)
  - forecast based on historical data 

- AWS Budgets 
  - to plan service usage, costs, instance reservations
  - predict the usage 
  - how much of the budget used 
  - SNS notification 
    - usage goes over budget amount 
	- estimated cost exceeds budget 
  - info updated 3 times a day 
  - Types
    - Cost Budget - plan how much desired to spend on service
	- Usage budget - 
	- RI Utilization budget
	- RI coverage budget 
	
- Consolidated billing
  - master account or payer account -- responsible for paying 
  - one or more member account 
  - by default member cant add /  edit / read other account budgets
  - but can be enabled via IAM policy 
  - AWS recommendation not to run any services in master account.  S3 alone created to store the bills
  - volume discounts (charged by aggregated usage)
  - tagging the resources across multiple accounts are complex 
  - usage of Reserved instance can be shared across accounts. sharing must be turned on both sides
  - This can be turned off also 
  
- Cost Monitoring 
  - Cloud watch can monitor and send notifications
  - Billing alerts with threshold 
  - upto 20000 budgets can be created by individual account or master account 
  - first 2 budgets are free
  - notification 
    - SNS topic
	- email 
	- or both

- Tags 

- Cost Allocation Tags
  - To track AWS costs details
  - must be activated to appear in cost explorer 
  - only master account or individual account would have the access 
  
- Resource Groups 
  - group of AWS resources sharing one or more tags 
  
- EC2 Purchasing options
  - OnDemand
  - Reserved
  - Scheduled - Specified schedule for one year term 
  - Spot
  - Dedicated Host 
  - Dedicated Instances
  - Capacity reservations 
  - Do not commit RI purchase before sufficiently bench marking your application in production 
  - RI utilization report to ensure we are making most of our reserved capacity 
  - Spot instances can be launched independently with auto scaling or EMR (task nodes)
  - Redshift doesn't use  SPOT

- AWS Organization 
  - Consolidating multiple accounts into single org
  - Free service 
  - Global like IAM 
  - Can be reached through endpoint in US East region 
  
  
- AWS Organization Components
  - Root account 
    - Parent container for all accounts and OU's in the org 
	- policy applied at root level flows through all accounts and OU's
	- Root created automatically when we create AWS organization 
	- Only one root in an 
	- Can have one or more individual accounts 
	- Can have one or more OU's 
  - OU (organizational unit)
    - Container of other OU's and account 
	- OU can have only one parent
	- An account can be member of only one OU 
	- policy applied at OU level flows through other accounts and OU's under this OU
    - Can have one or more individual accounts 
	- Can have one or more OU's 
	- Can nest OUs within other OU's at the up to the depth of 5
  - Individual Account 
    - Master Account 
	  - Payer account 
	  - Responsible for payment 
	  - Account that exists before creating AWS organization (Account that creates AWS Organization)
	  - From the master account 
	    - Add / Remove / Manage accounts in / from the organization
		- Can manage invitations to join to AWS Organization
		- Can apply Organizational policies (NOT IAM) at Root, OU and accounts level
	- All other accounts called member account 
	- An account can be member of only one account 
	
- AWS Organization Features 
  - if the account is created it will be automatically part of AWS organization
  - Can invite any other accounts to join in AWS Organization
  - Ability to apply policies selectively or all accounts in organization 
  - Accounts can be grouped into in OU (Finance OU)
  - Each OU can have separate policy 
  - Organization permissions can overrule / override account permissions 
  - Master account administrator have higher authority over the administrators of individual account 
  - Master account administrator can restrict which aws service or api actions that user in individual account can access to.  (which is authorized by individual account administrator)
  - User or IAM role can access only what is allowed by both AWS organization and Individual account policies 
  - if any service is denied by any one, this cannot be accessed 
  
- AWS Organization Data replication
  - Highly available service 
  - AWS Organization data is replicated to multiple data centers within the region 
  - Eventual consistency 
  - if any configuration or policies changes, might not available immediately if the configuration is retrieved from other data center. bit it will consistent eventually 

- AWS Organization Feature sets
  - operates on two modes 
  - 1. Consolidated Billing
    - default 
	- only consolidated feature is enabled 
	- it cant access other advanced features.
  - 2. All Features 
    - Full advanced features 
	- Can be enabled at the time or creation or can be enabled later time 
	- To enable All features from Consolidated billing mode, invite should be sent from master to all the member accounts.  member account have to approve the changes first 
  - in All features mode, Master account can 
    - has full control over what member accounts can do 
	- Can apply Service Control Policies (SCP's) to restrict all IAM users ((including root) and IAM roles in the account can do or access 
	- Can prevent member accounts from leaving the organization
	
- Service Control Policy (SCP)
  - SCP defines the services and actions the IAM users and IAM roles can do in the accounts to which SCP is applied to 
  - SCP doesn't grant permissions, they are only filtering permissions
  - it defines maximum permissions that affected accounts can have 
  - SCP can be applied to (like policy) 
    - Root level 
	- OU level 
	- Account level 
  - SCP has not effect on the master account 
  
- White-listing 
  - policy specifies list of actions or access  that are allowed 
  - all others actions are denied 
  - By default, Root, AU and accounts have default policy applied by AWS Organization - called FullAWSAccess
  - in other words, By default, everything allowed by IAM is whitelisted 
  - to change you need replace the default policy 
  - if the policy applied at root level, it will flow through all OUs and accounts under it and so on 
  - you cannot add what is denied by non default policy at root 
  
- Black-Listing 
  - Default for AWS organization
  - policy specifies list of actions or access  that are denied 
  - all others actions are allowed
  - To establish this 
    - Leave FullAWSAccess policy at the root level 
	- and attach more policies that define what is denied 
  - Like IAM, an explicit deny overrides any allow 
  
- AWS Organization integration with other services 
  - CloudTrail 
    - For central logging for all accounts logs in Organization
  - CloudWatch Events
  - AWS Config 
  - Artifact 
  - AWS Firewall Manager 
    - Centrally manage all firewall configurations
  - AWS Directory Service 
  - AWS Licensing Manager 
  - AWS RAM  
    - For sharing resources across the accounts (Sharing subnet, Transit gateway)  
  - AWS Service Catalog 
  - AWS Single SignOn

- AWS IAM Service Linked Role 
  - For integration with AWS services in Organization
  - AWS Organization created service linked role for AWS service in each member account of the AWS Organization, when that AWS service is authorized to access AWS Organization
  
- AWS Organization Best practices 
  - use AWS Cloud Trail to monitor the log activities in a Master account 
  - Do not add resources in master account (except S3 and Cloud-trail for storing bill and logs) 
  - Use the least privileged principle 
  - Assign SCP's at OU level instead of account level (easy scaling) 
  - Test you SCP's before rolling out 
  - avoid assigning the SCP's at root level unless necessary 
  - ue either white-listing or blacklisting but not both 
  - Establish clear strategy on when to create account 
  
# Deployment and Operations management

 - Cloudformation
   - Infra as as Code
   - Code that provision and configures the resources 
   - Result is called Stack 
   - Json or YAML 
   - Control and track changes 
   - create code or use existing template 
   - store locally or in S3
   - Cloudformation engine to create stack based on your template 
   - Calls Api calls to provision the resources
   - Can perform actions only that you have permissions to do
   - Cloudformation created S3 bucket in each region for you to upload the template file 
   - bucket is accessible to anyone who is with S3 permissions
   - if the bucket created by Cloudformation already presents, the template added to that bucket 
   - Can use your own bucket. Specify the bucket url in a stack 
   - Cloudformation would notify once the stack is created from the bucket
   - if Cloudformation fails, it rollbacks all the changes by deleting the resources 
   - To update stack resources, can modify the template 
   - no need to recreate the new template 
   - To update a stack 
     - Create a change set by submitting the modified template with different configurations or input values if any 
	 - Cloudformation compares the modified template with old template and generate the change set
     - can review the changes and confirm 
	 - Can execute the changes to update the stack
	 - Updates can cause interruptions
   - Cloudformation Designer 
     - Graphic tool for creating, viewing, modifying the template
     - diagram your template resource 
     - drag and drop 
     - integrated with Json/Yaml editor
   - Template components 
     - Format Version
	 - Description 
	 - Meta Data
	   - Additional information of the template 
	 - Parameters 
	   - Values to pass to your template at runtime 
	   - can refer parameters from resources and output section 
     - Mappings
       - mapping of keys and associated values 
       - like lookup table 
     - Conditions
       - based on the condition create resource, or create dev or prod infra
     - Transform 
       - For serverless applications
       - specifies version of the AWS Serverless Application Model to use (SAM)
	 - Resources 
	   - MANDATORY component
	   - specifies the stack resource 
	   - like EC2 and its configuration 
	 - Outputs 
	   - describes the values that are returned whenever you view your stack properties 
   - Intrinsic Functions 
     - built in functions to manage your stacks 
	 - Can be used in
	   - Resources 
	   - Outputs 
	   - Meta data
	   - to conditionally crete stack resources
	 - GetAtt
	 - FindInMap 
	 - Ref
	 - GetAZs
	 - ImportValue 
   - Resource Attributes 
     - Creation policy 
	   - When you want to delay your resource creation until another resource is created 
	   - invoked only when Cloudformation creates the associated resources 
	   - currently supported resources are 
	     - AWS::AutoScaling::AutoScalingGroup
		 - AWS::EC2::Instance 
		 - AWS::Cloudformation::WaitCondition
	 - Deletion Policy 
	   - When you want to retain or create snapshots before stack is being deleted 
	   - If no deletion policy is associated, Cloudformation deletes the resource by default 
	   - Attributes
	     - Delete 
		   - By Default deletes the resources 
		   - AWS::RDS::DBCluster resources, default policy is snapshot 
		   - for S3 buckets, you should delete all the objects in the bucket before deleting the bucket 
		 - Retain
		   - to retain
		   - Can be used for any resource 
		 - Snapshot
		   - EBS
		   - Elasticache 
		   - RDS 
		   - Redshift 
	   - Can use both Retain and Snapshot (costly)
	 - Depends On
	   - Specifies the resource creation is depends on another resource 
	 - Meta Data 
	   - If you want to add more meta data to your resources 
	 - Update Policy 
	   - defines how the resource update should be carried out 
   - Nested Stacks 
     - Stacks that creates others stacks 
     - To reuse common template patterns 
     - To create nested stacks, use AWS::CloudFormation::Stack to call/reference other templates
   - Cross Stack References 
     - to export shared resources 
	 - when you want organize your AWS resources based on life cycle and ownership 
	 - instead of including all your resources in a singe stack 
	 - Can create AWS resources in a separate stacks 
	 - Can refer the required resource outputs from other stacks 
	 - using Fn::ImportValue function
	 - For example, you have network stack with VPC, security group, subnet and web application stack 
   - Cloudformation Security Best Practices
     - Only allow specific templates and stack policies, so you can control the source of stack templates and stack policy documents
	 - Stack Policy - JSON documents describes what you can do during the update operation
	 - Restrict what resource types can an IAM principle can create 
   - CloudFormation with CloudTrail and SNS 
     - Cloudformation integrates with Cloud Trail
	 - if Logging is enabled, Cloud Trail logs all the AWS services API calls that were done by CloudFormation
	 
- OpsWorks 
  - Provides simple and flexible way to create and manage stacks and applications 
  - Distributes the applications to application servers 
  - monitor the stacks, performance, mange security and permissions 
  - Stack is the core AWS OpsWorks component 
  - For example, web application require, application servers, db servers, load balancers etc. this group of instances is called Stack 
  - OpsWorks allows SSH and RDP access to Stack instances
  - Resources can be managed  only in the region in which they are created 
  - Resources that are created in one regional endpoint are not available nor can they be cloned to another regional endpoint 
  - Chef 
    - chef provides automated configuration that enables consistent configurations at scale 
    - Cookbook 
      - package file that contains configuration information, including instructions called recipes 
    - Recipes 
      - set of one or more instructions
	  - written in ruby syntax language 
	  - specifies the resources to use and order in which those resources are applied 
	  - Opsworks runs the recipes automatically at the appropriate time 
	  - can also be run manually at anytime 
    - Resource 
      - in Chef terms, A resource, as used in chef, is statement of configuration policy 
	  - Thus is different in AWS OpsWorks terms
- OpsWorks TBD

- Elastic BeanStalk
  - Quickly deploy and manage applications into AWS cloud
  - IAAS
  - need not know or worry about AWS services and infrastructure 
  - Automatically controls capacity provisioning, load balancing, scaling and application health monitoring
  - less control 
  - for more control go for OpsWorks 
  - for more and more control go for CloudFormation
  - Java, .NET, PHP, Node.js, Ruby, Python, GO
  - Web Containers (Tomcat, Passenger, Puma), Docker containers
  - Difference from OpsWorks 
    - OpsWorks is to manage your infrastructure
	- EBS can automate, monitor and do the healthcheck 
	- EBS can rotate your application logs and publishes to S3
  - EBS Data Persistence 
    - EBS runs on EC2 instances that have no persistent local storage 
	- if you terminate the EBS you will lose all the data. when you start the EBS again EC2 will start with the default data 
	- its your responsibility to backup the data - S3, DyanamoDB, RDS (Not EBS (elastic block storage))
	- EBS doesn't restrict your choice of data persistence and DB service options 
	- don't include RDS within EBS. though its allowed not recommended. RDS can be outside EBS and can be accessed via CNAME (DNS)
  - EBS Rebuild 
    - EBS became unusable if you modify or terminate one of the underlying aws resource which is in EBS (due to dependency)   
	- if that happens you can rebuild the EBS environment to restore to working state 
	- rebuild would terminate all the resources, replace with new resources with same configuration 
	- you can also rebuild the terminated environment within 6 weeks (42 days). EBS attempt to create new environment with same id, name if it available
  - EBS components
    - Application 
	  - container for the environment 
	  - includes source code, configurations, logs and other artifacts that you create while creating EBS 
	- Application Version 
	- Environment
	  - Version that is deployed in AWS
	- Environment Tier 
	  - Web server environment 
	    - Apache or Tomcat
	  - Worker environment
	    - pulls the tasks from AWS SQS
	- Environment configuration 
	  - Configuration of associated resource 
	  - when update the configuration, EBS automatically apply the changes or terminate the resources and replace with new configuration 
	- Configuration Template 
  - EBS Custom AMI's 
    - supports custom platforms and custom AMI's 
	- lets say application uses different programming language and web server which not comes in pre build AMI's you can use custom AMI's 
	- to create custom platform 
	  - build AMI from one of the supported operating system (Ubuntu, RHEL, or Amazon Linux)
	  - create your own EBS platform using packer (open source)
	- build custom AMI (golden image) if you image require lot of softwares and pre configurations and use the golden image to build EBS
  - EBS Docker container 
    - EBS supports the deployment of web applications from docker containers 
	- if Docker container runs EBS crashes, EBS restarts automatically 
	- Single container configuration 
	  - when you only need to run one container per instance 
	- Multi container configuration 
	  - deployment of multiple docker containers to an Amazon ECS cluster in EBS
	  - when you need to run multiple containers in each instance 
	- Pre Configured Docker platform configurations
	  - Java with Glassfish 
  - EBS and S3
    - EBS creates S3 bucket named elasticbeanstalk-region-account-id for each region 
	- EBS use this bucket to store objects required for your application 
	- EBS doesn't turn on the default encryption. so by default the objects are not encrypted 
	- you can configure EBS to retrieve log files and publish to S3 (different bucket), enable log rotation and publish logs to S3 automatically after they rotated (difference from OpsWorks)
  - EBS and EFS 
    - shared drive 
  - EBS and RDS 
    - including RDS within EBS is not recommended. Ok only for Dev and test environment.  you lose the data if the EBS is terminated 
	- decouple RDS from your EBS 
	- use separate security group out side of EBS to avoid dependency. (don't use security group in EC2 (in EBS)  or RDS) - best practices
  - EBS and DynamoDB 
  - EBS and CloudWatch
    - automatically uses cloud watch to monitor your environment and application  
	- Can configure EBS to automatically stream logs to cloudwatch (difference from OpsWorks)
	- logs can be exported to S3.  you should create bucket and configure 
  - EBS and CloudTrail 
    - CloudTrail captures all api calls for Events 
	
- CloudWatch 
  - monitoring 
  - collect and track metrics
  - is a metrics repository
  - can do in console, cli, api and SDK 
  - VPC resources can connect to cloudwatch by creating cloudwatch VPC interface endpoint 
  - CW Namespaces 
    - container for CW repository
	- metrics in different namespaces are isolated to each other (not aggregated into same statistics)
	- naming convention AWS/service 
	- eg for EC2 it uses AWS/EC2 namespace
    - can specify namespace name when you create a metric 
    - there is no default namespaces	
  - CW Metrics 
    - time ordered set of data points 
	- CPU usage of EC2 instance is one metric 
	- AWS services send metrics to CW
	- metrics exists only in region in which they are created 
  - CW Timestamps 
    - metrics data must be marked with a timestamp 
	- timestamp can be upto 2 weeks in the past and upto two hours in the future 
  - Metric retention 
    - metrics cannot be deleted but it automatically expires after 15 months 
	- data points with the period less than 60 seconds are available for 3 hours
	  - these data points are called high resolution custom metrics
	- 60 seconds - 15 days 
	- 300 seconds (5 min) - 63 days 
	- 3600 seconds (1 hour) - 15 months (455 days) 
	- expire in rolling basis 
	- shorter period data points are aggregated to next tier when they expire 
  - CW Alarm 
    - alarm can be created either to watch single CW metric or result of a math expression based on CW metrics 
	- can be based on CW logs metric filters also 
	- can be added to CW dashboard (they become RED once the threshold is reached or alarm state ) 
	- Limited to actions like
	  - EC2
	  - SNS
	  - AutoScaling
	- for others use CW events
	- 
	- States 
	  - Ok
	  - ALARM 
	  - Insufficient Data
	- 3 settings required for alarm 
	  - Period in seconds 
	  - Evaluation period 
	  - Data points to Alarm 
	- Actions
	  - Action when in Alarm state 
	  - EC2 action 
	    - Recover 
		- Start 
		- Reboot 
		- Terminate 
	  - CW doesn't test or validate the action results. Eg, it cant detect EC2, autoscaling, SNS errors 
    - CW Logs 
	  - to monitor, store, access logs from many AWS / non AWS sources 
	  - no code change required 
	  - view the logs 
	  - sort or search logs for error codes or patterns 
	  - filter
	  - archive securely 
	  - CW logs can monitor logs in EC2 instance in real time
	  - CloudTrail events can be logged to CW logs and can do Alarm
	  - CW logs can be stored in S3 
	    - can export logs from multiple log groups 
		- takes 12 hours to export log data
		- for real time analysis 
		  - use CW logsInsights 
		  - using subscriptions
		    - based on subscription filter 
		    - log group can be configured to stream the data to AW Elastic Search service in near real time  (costly)
			- Kinesis stream 
			- Kinesis Data Firehose
			- Lambda
	  - Log Data sources 
	    - EC2, ECS (requires cloudwatch agent)
		- CloudTrail, Route53 DNS queries 
		- Aurora, MySql, mariaDB
		- Lambda 
		- Logs from on perm services (requires cloud watch agent) 
		- VPC flow logs etc 
	  - Log Events 
	    - log 
		- two parts 
		  - timestamp
		  - raw log data 
	  - Log Streams
	    - sequence of log events which shares the same source
		- AWS deletes log streams which is being empty for 2 years.  or you can delete it manually 
	  - Log Groups 
	    - Group of log streams that shares same retention, monitoring, access control 
		- each log stream belong to one log group 
	  - Log retention 
	    - by default indefinite or never expire (costs charge)
        - retention can be adjusted at log group level (1 day to 10 years)
        - expired log events deleted automatically 
      - Metric Filters
        - can you filters to configure custom metrics 
        - assigned to log groups 
        - filter the word "success" in log events. for every match CW logs reports the data to the specified metric
        - CW logs sends metrics to CW for every 1 min
	  - Encryption in  Transit 
	    - uses end to end encryption in data transit 
		- manages server side encryption keys 
	  - Encryption at Rest 
	    - manages server side encryption keys
		- you can encrypt using AWS KMS customer master key (CMK)
		- encryption is enabled at log group level 
		- can be done either at the time log group creation or after it exists
    - CW Logs Insights
	  - can create custom / powerful queries and visualize the data in dashboards 
	  - enables interactive search and analysis of the log data in CW logs 
	  - includes purpose-built query language with simple and powerful commands 
	  - automatically discovers fields in logs
	  - single request can query upto 20 log groups
	  - can search older data after Nov 5th 2018
	- CW Agent 
	  - agent to collect logs and stream
	  - EC2 or on perm server 
	  - Old Agent 
	  - New Unified CW agent 
	    - supports both linux and windows servers 
		- additional metrics 
	- Cross account logs data sharing 
	  - Kinesis Stream
	- Central logging in multi account environment 
	- CW Events
	  - Scheduled (creating EBS snapshot periodically) 
	  - application level events
	  - cron 
	  - State change (EC2 terminated) 
	  - Target is AWS resource (all most all the the services) (EC2, lambda etc)
	  - JSON message 
	  - Sharing CW events across accounts
        - Event can be sent from one account to another account 
        - both accounts should be in same region 
        - receiving account will NOT be charged. sending account will be charged for CW events
    - CW Synthetics 
      - enables canaries creation 
      - monitor endpoints and API's in your environment from the outside in 
      - can check the availability of endpoints, load time, latency etc 
      - can create CW alarms based on created canaries 
	- CW ServiceLens
	  - To monitor the health of the application
	  - Enhances the observability of applications by integrating traces, metrics, logs and alarms into one place 
	  - ServiceLens integrates CW with X-ray

- AWS Config
  - it provides detailed view of the configuration of AWS resources in an AWS account 
    - Resources relation with each other 
    - Configuration and configuration history 
  - private connection between VPC and AWS config will be established. so the communication over the AWS infrastructure, not over the Internet. 	
  - Benefits 
    - get the snapshot of current configuration 
	- get historical configurations
	- notification when the resource created, edited and deleted 
	- view relationship between resources 
  - Configuration Item 
    - point in time view of AWS resource 
	- includes metadata, attributes, relationship, configuration, and events
	- AWS config creates the configuration item whenever it detects a change in resource type 
  - Configuration History 
    - collection of Configuration Item for a given time 
	- helps to find when the resource created, edited etc
	- configuration history file stored in S3 automatically 
	- retention period minimum of 30 days to 7 years 
  - Configuration Recorder 
  - Configuration Snapshot 
  - Configuration Stream
  - Resource Relationship
  - Delivering configuration items 
    - S3 bucket 
	  - AWS config tracks the changes in the configuration of your AWS resources and regularly sends the configuration file to S3 bucket which you specify 
	  - sends for every six hours for the six hours periodically
	  - if no change it will not be sent
	- SNS Topic 
      - AWS Config uses SNS topic you specify to send the notifications
      - for better results use SQS as the notification endpoint for the SNS topic 
    - Evaluating resources with Rules 
      - it has managed rules
      - custom rules can be created
      - for compliance 
      - if any resource violate the rules, AWS config mark the resource as non compliant 
	  - can run periodically or when there change in configuration 
	  - each rule is associated with lambda 
	- SNS Notifications
	- Multi Account Multi region Data Aggregation 
	  - aggregates configuration and compliances from multiple accounts and regions to single account 
	  - Source Account 
	    - Account from which config data will be collected 
		- can be a individual account or organization 
	  - Source Region 
	    - region from which configuration and compliance data need to be aggregated
      - Aggregator 
        - resource type in AWS Config
        - collects configuration and compliance data from multiple source accounts and regions
        -  		
	  
	
		
	   
		

	
	  
	
	  
	 
	 
  
	
	
- Practice	
  - ORACLE RAC (Real Application Clusters)  not supported in AWS 
  - AD connector (for ad to connect on perm)
  - AWS Shield standards - to protect layer 3 and layer 4 attacks like SYN floods and UDP reflection attacks   
  - AWS Shield advanced - includes notification 
  - Route 53 and ELB integration should use alias record.  Not non alias record, A record or CNAME  
  - Storage Gateway can store upto 512 TB 
  - Can attach multiple ENI (elastic network interface) so can assign multiple ip /EIP's
  - only one virtual private gateway (VGW) can be associated with VPC
  - A record - to IP 
  - CNAME - to another name. but one name can be associated 
  - Alias - to another name. but multiple name can be associated 
	  
  
  


  
  



	
	
  
	
	