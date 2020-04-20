# OCI Foundations

## Concepts

- IAAS
  - Cloud provider 
    - network
	- storage 
	- servers 
	- visualization
  - You 
    - OS 
	- Middleware 
	- application 
	- Data
	- Runtime

- PAAS 
  - you 
    - Data 
	- application 
	

- SAAS 

- HA 
  - Load balancer 
  
- DR 
  - RPO 
  - RTO 
  
- Fault Tolerance 

- Scalability 
  - horizontal 
  - vertical
  
- Elasticity 

- CAPeX vs OPeX

## OCI Architecture 

- Regions 
  - 21 regions live
  - 15 planned
  - has one or more AD 
  
- AD 
  - region has 3 AD 
  - new regions has only one AD 
    - one AD region 
	  - region pair - for HA and DR
 
- FD
  - logical data centers 
  - each AD has 3 FD's 
  - even if one FB's down other 2 FD's serves the requests to make AD HA 
  - placement of you compute / DB instances to FD can be chosen during launch time

- Compartment 
  - Logical collection of resources 
  - Tenancy / Root compartment
  - Network compartment 
  - Storage compartment 
  - isolation for access 
  - best practice 
  - each resource belongs to one compartment
  - resources can interact each other with resources in different compartments
  - resources in compartments can be added or deleted at any time 
  - resources can be moved from one compartment  to another 
  - resources from multiple regions can be in same compartment 
  - its global 
  - Can be Nested - upto Six levels 
  - can give access to group of users to compartments
  - bulling 
  
## IAM 

- users 
- groups 
- instance principles 
- Authentication 
  - username and password
  - API signing key 
  - Auth Tokens 
- Authorization 
  - polices 

- Policy 
  - everything is disabled by default
  - Allow <subject or group <group_name> to <verb> <resource_type> in <tenancy or compartment> where <condition> 
  - verb 
    -  inspect 
	   - list resources
	- read 
	  - inspect + user metadata 
	- use 
	  - read + update 
	  - cannot create and delete 
	- manage 
	  - all permissions 
  - resource-type
    - all-resources 
	- database-family 
	- instance-family 

  
## Compute Resources 

- Bare Metal
  - physical server 
  - No virtualization  
  - you have to manage OS and runtime
  - use case 
    - high performance workloads 
	- workloads that are not virtualized 
	- workloads require specific hypervisor
	- workloads require BYO licensing 
- Dedicated Virtual Hosts 
  - hosts dedicated to you 
  - you can run vm's on top
  - dedicated to you
  -  you have to manage OS and runtime
- Virtual Machines 
  - hyper-visor to virtualize 
  - VM's on top of bare metals 
  - not dedicated 
  - Bare metal can be shared by other tenants as well 
- Container Engine 
  - OKE 
  - OCIR - registry 
- Functions 
  - Oracle Functions
  - Serverless 
  - its based on FN project 
  - FN project is open source platform 
  - Can run anywhere 
  - any cloud platforms  
  - or on premises 

- Instance Basics 
  - sizes based on CPU, RAM, Bandwidth 
  - Intel and AMD
  - GPU 
  - HPC - high performance computing 

## Storage Services

- Block Volume 
  - remote storage 
  - persistent 
  - boot volume (type of block volume)
  - block volume 
  - DB 
  - NTFS 
  - Highly durable 
    - Block Volumes stores replica of data in 3 different FD's 
  - automated backups to obj storage 
  - can copy backups to another region 
  - encryption 
  - Volumes can be 50 GB to 32 TB in size 
  - 32 volumes can be attached to instance 
  - Tiers 
    - Basic
	  - big data 
	  - data-ware housing 
	  - 2 IOPS/GB
	  - 240 KB/s /GB throughput
	- Balanced 
	  - boot disk 
	  - 60 IOPS/GB
	  - 480 KB/s /GB throughput
	- Higher Performance 
	  - large transactional db 
	  - 75 IOPS/GB
	  - 600 KB/s /GB throughput
  
- Local NVMe 
  - NVMe disk 
  - Non volatile memory express 
  - Temporary 
  - non persistent 
  - survives reboot 
  - local attached 
  - use case 
    - No sql DB 
	- in memory DB 
	- high performance local storage 

- File Storage 
  - file structure 
  - NFS and SMB 
  - access over networks
  - supports all major OS and hypervisor 
  - NAS and SAN 
  - FSS - File storage service 
  - shared file system storage 
  - NFSv3 compatible
  - hierarchy structure 
  - HA 
  - replicates in 3 FD 
  - snapshot for backup 
  
- Object Storage 
  - bucket 
  - metadata 
  - unstructured 
  - no folder hierarchy  like file storage 
  - OSS
  - regional service 
  - not tied to compute instance 
  - HA 
  - Tiers 
    - Standard (hot) 
	  - Fast, immediate, Frequent access
	  - can be downgraded to archive 
	- Archive (Cold)
	  - infrequently accessed 
	  - 10x cheap then standard 
	  - 90 days minimum retention 
	  - 4 hours to restore 
	  - cant be upgraded to standard
- Archive Storage 

## Networking Services

- Virtual Cloud Network 
  - VCN 
  - Region specific 
  - Address range - 10.0.0.0/16
  - subnets 
  
- Gateway  
  - Internet Gateway 
  - NAT Gateway
    - Enables outbound connections to Internet 
	- Blocks inbound connections initiated from Internet
    - Updates Patches
    - for resources in private subnet 
  - Dynamic Routing Gateway
    - to connect to your on prem data center 
	- IPsec VPN (site to site)
	- FastConnect (private, dedicated)
  - Service gateway 
    - access OCI resource with in OCI network
	- without using Internet or NAT gateway
	
- Security 
  - SG
  - NACL 
  - can apply at vcn layer - NAC
  -  or subnet layer - SG
  
- VCN Peering 
  - Local VCN Peering 
    - Same region 
  - Remote VCN Peering 
    - between different regions
  - Peering is not transitive 
  - explicit peering required 
  
- Load Balancer 
  - primary and standby (failover in different FD)
  - Public Load Balancer 
  - Private Load Balancer 
    	
## DB Services 


- VM DB systems 
  - VM with DB 
  - fast provisioning 
  - Storage scaling with no impact
  - will not be billed in stop state 
  - customer managed 
  - updates - customer initiated
  - scaling - storage - yes 
  - scaling - cpu - no 
  - backup - customer initiated
  - storage - block
  - RAC - available - 2 nodes 
  - Dataguard - available
- BM DB Systems 
  - Bare Metal 
  - Dedicated 
  - Fast performance
  - Dynamic CPU (scale up and down)
  - billing charged even in stop state
  - customer managed 
  - updates - customer initiated
  - backup - customer initiated
  - scaling - storage - no 
  - scaling - cpu - yes  
  - storage - NVMe
  - RAC - Not available
  - Dataguard - available
- RAC 
  - 2 Node VM RAC
  - even if one node goes down, HA 
- Exadata DB systems 
  - fully managed exadata db
  - customer managed 
  - updates - customer initiated
  - backup - customer initiated
  - storage - Local Disk and NVMe
  - RAC - available
  - Dataguard - available (configure manually) 
- Autonomous 
  - self driving 
  - self securing 
  - self repairing 
  - auto healing 
  - auto backups 
  - auto patching (no downtime)
  - oracle managed 
  - two types
    - shared 
	  - you provision and manage only the autonomous DB 
	  - oracle manages the infra and deployment 
	  - supports both ATP and ADW
	  - updates - automatic
	  - scaling - storage - yes 
      - scaling - cpu - yes 
	  - backup - automatic
	  - storage - Local Disk and NVMe
	  - RAC - Not available
	  - Dataguard - not available
    - dedicated
	  - exclusive use of exadata hardware 
	  - supports both ATP and ADW
	  - updates - customer policy control 
	  - scaling - storage - yes 
      - scaling - cpu - yes 
	  - backup - automatic
	  - storage - Local Disk and NVMe
	  - RAC - Not available
	  - Dataguard - not available
  
  
- Autonomous -  

- DB Operations
  - BYOL
  - Patching 
  - ATP
    - Autonomous Transaction Processing 
	- OLTP 
  - ADW
    - Autonomous Data Warehouse 
	  - OLTP 

- DB Backup and Restore 
  - manual 
  - automatic
  - to Object storage via service gateway 

- DR 
  - data guard to replicate the data
  - between DB in two AD's
  - active data guard 
    - extends data guard 
	- for data protection and HA 
	- included by default in Extreme performance edition and Exadata service 
 - Two modes
   - switchover 
     - planned migration 
	 - no data loss 
   - failover 
     - unplanned   
     - minimal data loss
	 
- DB Systems HA and DR 
  - multi AD region 
    - when you create RAC1, by default in AD1
      - RAC primary DB1 is placed in FD1
	  - RAC primary DB2 is placed in FD2
    - you can have another RAC2 in AD2 and replicate data with Data Guard for HA 
  - for one AD region 
    - RAC1, primary DB1 and DB2 can be placed in FD1 and FD2 
	- RAC2, secondary DB1 and DB2 can be placed in FD2 ad FD3 
	- so even if any one FD gone down also no issues 
	
## Security 

- Shared Security Model 
  - Oracle manages 
    - security of the cloud 
	- network, storage, servers, virtualization
  - you manage 
    - security in the cloud
	- patching 
	- iam 
	- network security 
	- data compliance 
	
- Security Services 
  - IAM 
    - IAM
	  - Role based Access control 
	- MFA
	- Federation 
  - Data Protection 
    - Data encrypted at rest and transit 
    - Data Safe 
	  - protect sensitive data in  OCL db's 
	  Supports ATP (shared), ADW (shared), VM/BM DB 
	  - security  assessment 
	  - user assessment 
	  - data masking
	  - no security expertise required 
	  - no extra cost to use 
	- Data Vault 
	- Key Management 
	  - Bring your own keys
	  - HSM 
	  - FIPS certified
	  - Federal Information Processing Standards
  - OS and Workload management 
    - OS management service 
	  - install security updates 
	  - patches 
	  - without any downtime 
	  - security / compliance reporting 
	  - configured by default for oracle linux instances 
  - Infrastructure protection 
    - VCN NSG, SL 
	- WAF
	  - OWASP violations
	  - DDOS 

- Compliance Certifications
  - Global 
    - AICPA SOC
	  - SOC1:SOC2:SOC3
	  - Service and Organization Control 
	- ISO 
	  - 27001:27017:27018
	- CSA 
	  - Level 1
	  - CLoud Security Alliance 
	- Privacy Shield Framework 
  - Government
    - DOD DISA 
	- G Cloud 11 - UK 
	- Model Clauses - EU 
  - Industry 
    - HIPAA
	- PCI DSS
	- FISC - Japan 
	- NHS - UK 
	- FINMA - Switzerland 
  - Regional 
    - GDPR -EU 
	- BSI C5 - Germany 
	- TISAX - Germany 
	- PIPEDA - Canada 
	- My Number - Japan 
	
## Pricing Billing 

- Pricing Models 
  - PAYG 
    - Pay As you go 
	- no minimum service period 
	- metered hourly 
  - Monthly Flex
    - Universal Credits
	- minimum of 1000$ charge monthly and minimum 12 months contract 
	- 33% to 60% savings vs PAYG 
	- discounts based on size and deal 
	- monthly prepaid commitment 
  - BYOL 
    - Bring your Own License 
	
- Factors
  - resource size
  - resource type
  - data transfer 
    - Ingress is free 
	- Egress is not free
  - All oci regions are same pricing (unlike AWS)
	
- Block Volume pricing 
  - Storage cost 
  - performance cost 
  
- Data Transfer Cost 
  - no charge with in AD 
  - no charge with in Region 
  - for diff region or Internet 
    - ingress is free 
	- egress is not free 
  - no charge for DRG to on prem 
    - both ingress and egress are free 
	- aws charges 
	- vpn and fastconnect 
	
- Billing and Cost management 
  - Cost Tracking Tags
  - Budgets 
    - alerts 
	- evaluated for every 15 mins
  - Usage reports
    - csv file 
	- retained for one year 
	- 24 hours of usage data 
	
- Free Tier
  - 300$ free credits 
  - for 30 days 
  - which ever comes first 
  - upto 8 instances across all available services 
  - upto 5 TB storage 
  
- Always free 
  - 2 oracle autonomous DB 
  - 2 OCI compute VM's, Block, Object and Archive storage
  - Load balancer and data egress 
  - monitoring 
  - notifications
  

  
	
	

	
	
  
  
  
 
	
