
# 2020 AWS Certified Solutions Architect - SAP-C01

# Identity & Federation 

## IAM 
   - Users: long term credentials
   - Groups
   - Roles: short-term credentials, uses STS
     - EC2 Instance Roles: uses the EC2 metadata service. One role at a time per instance
     - Service Roles: API Gateway, CodeDeploy, etc…
     - Cross Account roles
   - Policies
     - AWS Managed
     - Customer Managed
     - Inline Policies
   - Resource Based Policies (S3 bucket, SQS queue, etc…) 

## IAM Policies Deep Dive   
   - Anatomy of a policy: JSON doc with Effect, Action, Resource, Conditions, Policy Variables
   - Explicit DENY has precedence over ALLOW   
   - Best practice: use least privilege for maximum security   
     - Access Advisor: See permissions granted and when last accessed
     - Access Analyzer: Analyze resources that are shared with external entity
   - Navigate Examples at:
     - https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_examples.html

## IAM AWS Managed Policies
   - AdministratorAccess
     - Effect: Allow,
     - Action: *,
     - Resource: *
   - PowerUserAccess
     - Effect: Allow,
     - NotAction: 
       - iam:*,
       - organizations:*,
       - account:*
     - Resource: *
     - …
     - Effect: Allow,
     - Action: 
       - iam:CreateServiceLinkedRole,
       - iam:DeleteServiceLinkedRole,
       - iam:ListRoles,
       - organizations:DescribeOrganization”,
       - account:ListRegions
     - Resource: *
     - Note how ”NotAction” is used instead of Deny
     
## IAM Policies Conditions
    - "Condition" : { "{condition-operator}" : { "{condition-key}" : "{condition-value}" }}
    - Operators:
      - String (StringEquals, StringNotEquals, StringLike…)
        - "Condition": {"StringEquals": {"aws:PrincipalTag/job-category": "iamuser-admin"}}
        - "Condition": {"StringLike": {"s3:prefix": [ "", "home/", "home/${aws:username}/" ]}}
      - Numeric (NumericEquals, NumericNotEquals, NumericLessThan…)
      - Date (DateEquals, DateNotEquals, DateLessThan…)
      - Boolean (Bool):
        - “Condition": {"Bool": {"aws:SecureTransport": "true"}}
        - "Condition": {"Bool": {"aws:MultiFactorAuthPresent": "true"}}
      - (Not)IpAddress:
        - "Condition": {"IpAddress": {"aws:SourceIp": "203.0.113.0/24"}}
      - ArnEquals, ArnLike
      - Null: "Condition":{"Null":{"aws:TokenIssueTime":"true"}} 

## IAM Policies Variables and Tags
    - Example: ${aws:username}
      - "Resource": ["arn:aws:s3:::mybucket/${aws:username}/*"]
    - AWS Specific:
      - aws:CurrentTime, aws:TokenIssueTime, aws:principaltype, aws:SecureTransport, aws:SourceIp, aws:userid, ec2:SourceInstanceARN
    - Service Specific:
      - s3:prefix, s3:max-keys, s3:x-amz-acl, sns:Endpoint, sns:Protocol…
    - Tag Based:
      - iam:ResourceTag/key-name, aws:PrincipalTag/key-name…

## IAM Roles vs Resource Based Policies
   - Attach a policy to a resource (example: S3 bucket policy) versus attaching of a using a role as a proxy
   - When you assume a role (user, application or service), you give up your original permissions and take the permissions assigned to the role
   - When using a resource based policy, the principal doesn’t have to give up any permissions
   - Example: User in account A needs to scan a DynamoDB table in Account A and dump it in an S3 bucket in Account B.
   - Supported by: Amazon S3 buckets, SNS topics, SQS queues

## Using STS to Assume a Role
   - Define an IAM Role within your account or cross-account
   - Define which principals can access this IAM Role
   - Use AWS STS (Security Token Service) to retrieve credentials and impersonate the IAM Role you have access to (AssumeRole API)
   - Temporary credentials can be valid between 15 minutes to 1 hour
   
## Assuming a Role with STS
   - Provide access for an IAM user in one AWS account that you own to access resources in another account that you own
   - Provide access to IAM users in AWS accounts owned by third parties
   - Provide access for services offered by AWS to AWS resources
   - Provide access for externally authenticated users (identity federation)
   - Ability to revoke active sessions and credentials for a role (by adding a policy using a time statement – AWSRevokeOlderSessions)
   - When you assume a role (user, application or service), you give up your original permissions and take the permissions assigned to the role

## Providing Access to an IAM User in Your or Another AWS Account That You Own
   - You can grant your IAM users permission to switch to roles within your AWS account or to roles defined in other AWS accounts that you own.
   - Benefits:
     - You must explicitly grant your users permission to assume the role.
     - Your users must actively switch to the role using the AWS Management Console or assume the role using the AWS CLI or AWS API
     - You can add multi-factor authentication (MFA) protection to the role so that only users who sign in with an MFA device can assume the role
     - Least privilege + auditing using CloudTrail

## Cross account access with STS
   - 1. Admin creates role that grants Development account read/write access to productionapp bucket
   - 2. Admin grants members of the group Developers permission to assume the UpdateApp Role
   - 3. Users requests Access to role 
   - 4. STS returns Role credentials
   - 5. User can access the S3 bucket by using the role credentials

## Providing Access to AWS Accounts Owned by Third Parties
   - Zone of trust = accounts, organizations that you own
   - Outside Zone of Trust = 3rd parties
   - Use IAM Access Analyzer to find out which resources are exposed
   - For granting access to a 3rd party:
     - The 3rd party AWS account ID
     - An External ID (secret between you and the 3rd party)
       - To uniquely associate with the role between you and 3rd party
       - Must be provided when defining the trust and when assuming the role
       - Must be chosen by the 3rd party
     - Define permissions in the IAM policy

STS Important APIs
   - AssumeRole: access a role within your account or cross-account
   - AssumeRoleWithSAML: return credentials for users logged with SAML
   - AssumeRoleWithWebIdentity: return creds for users logged with an IdP
     - Example providers include Amazon Cognito, Login with Amazon, Facebook, Google, or any OpenID Connect-compatible identity provider
     - AWS recommends using Cognito instead
   - GetSessionToken: for MFA, from a user or AWS account root user
   - GetFederationToken: obtain temporary creds for a federated user, usually a proxy app that will give the creds to a distributed app inside a corporate network

## Identity Federation in AWS
   - Federation lets users outside of AWS to assume temporary role for accessing AWS resources.
   - These users assume identity provided access role.
   - Federations can have many flavors:
     - SAML 2.0
     - Custom Identity Broker
     - Web Identity Federation with Amazon Cognito
     - Web Identity Federation without Amazon Cognito
     - Single Sign On
     - Non-SAML with AWS Microsoft AD
   - Using federation, you don’t need to create IAM users

## SAML 2.0 Federation
   - To integrate Active Directory / ADFS with AWS (or any SAML 2.0)
   - Provides access to AWS Console or CLI (through temporary creds)
   - No need to create an IAM user for each of your employees
   - https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_saml.html 
   - https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_enable-console-saml.html

## SAML 2.0 Federation – Active Directory FS
   - Same process as with any SAML 2.0 compatible IdP
   - https://aws.amazon.com/blogs/security/aws-federated-authentication-with-active-directory-federation-services-ad-fs/

## SAML 2.0 Federation
   - Needs to setup a trust between AWS IAM and SAML (both ways)
   - SAML 2.0 enables web-based, cross domain SSO
   - Uses the STS API: AssumeRoleWithSAML
   - Note federation through SAML is the “old way” of doing things
   - Amazon Single Sign On (SSO) Federation is the new managed and simpler way
   - Read more here: https://aws.amazon.com/blogs/security/enabling-federation-toaws-using-windows-active-directory-adfs-and-saml-2-0/

## Custom Identity Broker Application
   - Use only if identity provider is not compatible with SAML 2.0
   - The identity broker must determine the appropriate IAM policy
   - Uses the STS API: AssumeRole or GetFederationToken
   - https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_common-scenarios_federated-users.html

## Web Identity Federation – AssumeRoleWithWebIdentity
   - Not recommended by AWS 
   – use Cognito instead (allows for anonymous users, data synchronization, MFA)
   - https://docs.amazonaws.cn/en_us/amazondynamodb/latest/developerguide/WIF.html

## Web Identity Federation – AWS Cognito
   - Preferred way for Web Identity Federation
     - Create IAM Roles using Cognito with the least privilege needed
     - Build trust between the OIDC IdP and AWS
   - Cognito benefits:
     - Support for anonymous users
     - Support for MFA
     - Data synchronization
   - Cognito replaces a Token Vending Machine (TVM)
   - https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_oidc_cognito.html

## Web Identity Federation – IAM Policy
   - After being authenticated with Web Identity Federation, you can identify the user with an IAM policy variable.
   - Examples:    
     - cognito- identity.amazonaws.com:sub    
	 - www.amazon.com:user_id    
	 - graph.facebook.com:id    
	 - accounts.google.com:sub

## What is Microsoft Active Directory (AD)?
   - Found on any Windows Server with AD Domain Services
   - Database of objects: User Accounts, Computers, Printers, File Shares, Security Groups
   - Centralized security management, create account, assign permissions
   - Objects are organized in trees
   - A group of trees is a forest 

## What is ADFS (AD Federation Services)?
   - ADFS: provide single sign-on across applications
   - SAML across 3rd party: AWS Console, Dropbox, Office365, etc…
   - https://aws.amazon.com/blogs/security/how-to-establish-federated-access-to-your-awsresources-by-using-active-directory-user-attributes/

## AWS Directory Services
   - AWS Managed Microsoft AD
     - Create your own AD in AWS, manage users locally, supports MFA
     - Establish “trust” connections with your on- premise AD
   - AD Connector
     - Directory Gateway (proxy) to redirect to on- premise AD
     - Users are managed on the on-premise AD
   - Simple AD
     - AD-compatible managed directory on AWS
     - Cannot be joined with on-premise 

## AWS Managed Microsoft AD
   - Managed Service: Microsoft AD in your AWS VPC
   - EC2 Windows Instances:
     - EC2 Windows instances can join the domain and run traditional AD applications (sharepoint, etc)
     - Seamlessly Domain Join Amazon EC2 Instances from Multiple Accounts & VPCs
   - Integrations:
     - RDS for SQL Server, AWS Workspaces, Quicksight…
     - AWS SSO to provide access to 3rd party applications
   - Standalone repository in AWS or joined to on- premise AD
   - Multi AZ deployment of AD in 2 AZ, # of DC (Domain Controllers) can be increased for scaling
   - Automated backups
   
## AWS Microsoft Managed AD - Integrations
   - RDS for SQL Server
   - Amazon WorkSpaces
   - Amazon Quicksight
   - Amazon Connect
   - Amazon WorkDocs
   - AWS Single-Sign On
   
## Connect to on-premise AD
   - Ability to connect your on-premise Active Directory to AWS Managed Microsoft AD
   - Must establish a Direct Connect (DX) or VPN connection
   - Can setup three kinds of forest trust:
     - One-way trust: AWS => On-Premise
     - One-way trust: On-Premise => AWS
     - Two-way forest trust: AWS <=> On-Premise
   - Forest trust is different than synchronization (replication is not supported)

## Solution Architecture: Active Directory Replication
   - You may want to create a replica of your AD on EC2 in the cloud to minimize latency of in case DX or VPN goes down
   - Establish trust between the AWS Managed Microsoft AD and EC2
   
## AWS Directory Services - AD Connector
   - AD Connector is a directory gateway to redirect directory requests to your on-premises Microsoft Active Directory
   - No caching capability
   - Manage users solely on-premise, no possibility of setting up a trust
   - VPN or Direct Connect
   - Doesn’t work with SQL Server, doesn’t do seamless joining, can’t share directory 
   - https://aws.amazon.com/blogs/security/how-to-connect-your-on-premises-active-directory-toaws-using-ad-connector/

## AWS Directory Services - Simple AD
   - Simple AD is an inexpensive Active Directory–compatible service with the common directory features.
   - Supports joining EC2 instances, manage users and groups
   - Does not support MFA, RDS SQL server, AWS SSO
   - Small: 500 users, large: 5000 users
   - Powered by Samba 4, compatible with Microsoft AD
   - lower cost, low scale, basic AD compatible, or LDAP compatibility
   - No trust relationship

## AWS Organizations
   - Master accounts must invite Child Accounts
   - Master accounts can create Child Accounts
   - Master can access child accounts using:
     - CloudFormation StackSets to create IAM roles in target accounts
     - Assume the roles using the STS Cross Account capability
   - Strategy to create a dedicated account for logging or security
   - API is available to automate AWS account creation
   - Integration with AWS Single Sign-On (SSO)

## AWS Organizations - Features
   - Consolidated billing features:
     - Consolidated Billing across all accounts - single payment method
     - Pricing benefits from aggregated usage (volume discount for EC2, S3…)
   - All Features (Default):
     - Includes consolidated billing features
     - You can use SCP
     - Invited accounts must approve enabling all features
     - Ability to apply an SCP to prevent member accounts from leaving the org
     - Can’t switch back to Consolidated Billing Features only

## Multi Account Strategies
   - Create accounts per department, 
   - per cost center, per dev / test / prod, 
   - based on regulatory restrictions (using SCP), 
   - for better resource isolation (ex: VPC), 
   - to have separate per-account service limits, isolated account for logging,
   - Multi Account vs One Account Multi VPC
   - Use tagging standards for billing purposes
   - Enable CloudTrail on all accounts, send logs to central S3 account
   - Send CloudWatch Logs to central logging account
   - Establish Cross Account Roles for Admin purposes

## Organizational Units (OU) - Examples
   - Business Unit 
   - Environmental Lifecycle 
   - Project-based
   - https://aws.amazon.com/answers/account-management/awsmulti-account-billing-strategy/

## Service Control Policies (SCP)
   - Whitelist or blacklist IAM actions
   - Applied at the Root, OU or Account level
   - SCP is applied to all the Users and Roles of the Account, including Root
   - The SCP does not affect service-linked roles
     - Service-linked roles enable other AWS services to integrate with AWS Organizations and can't be restricted by SCPs.
   - SCP must have an explicit Allow (does not allow anything by default)
   - Use cases:
     - Restrict access to certain services (for example: can’t use EMR)
     - Enforce PCI compliance by explicitly disabling services

## SCP Examples
   - Blacklist and Whitelist strategies
   - More examples: https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_example-scps.html

## IAM Policy Evaluation Logic
   - https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_evaluation-logic.html

## AWS Organizations – Reserved Instances
   - For billing purposes, the consolidated billing feature of AWS Organizations treats all the accounts in the organization as one account.
   - This means that all accounts in the organization can receive the hourly cost benefit of Reserved Instances that are purchased by any other account.
   - The payer account (master account) of an organization can turn off Reserved Instance (RI) discount and Savings Plans discount sharing for any accounts in that organization, including the payer account
   - This means that RIs and Savings Plans discounts aren't shared between any accounts that have sharing turned off.
   - To share an RI or Savings Plans discount with an account, both accounts must have sharing turned on.

## AWS Resource Access Manager (RAM)
   - Share AWS resources that you own with other AWS accounts
   - Share with any account or within your Organization
   - Avoid resource duplication!
   - VPC Subnets:
     - allow to have all the resources launched in the same subnets
     - must be from the same AWS Organizations.
     - Cannot share security groups and default VPC
     - Participants can manage their own resources in there
     - Participants can't view, modify, delete resources that belong to other participants or the owner
   - AWS Transit Gateway
   - Route53 Resolver Rules
   - License Manager Configurations

## AWS Single Sign-On (SSO)
   - Centrally manage Single Sign-On to access multiple accounts and 3rd-party business applications.
   - Integrated with AWS Organizations
   - Supports SAML 2.0 markup
   - Integration with on-premise Active Directory
   - Centralized permission management
   - Centralized auditing with CloudTrail
   - https://aws.amazon.com/blogs/security/introducing-aws-single-sign-on/

## AWS Single Sign-On (SSO) – Setup with AD
   - Options for integration
     - 1. Standalone AWS Managed Microsoft AD
     - 2. AD Connector to on-premise AD
     - 3. AWS Managed Microsoft AD with two-way forest trust with on-premise AD

## SSO – vs AssumeRoleWithSAML
   - diagram 
   
## Summary of Identity & Federation
   - Users and Accounts all in AWS
   - AWS Organizations
   - Federation with SAML
   - Federation without SAML with a custom IdP (GetFederationToken)
   - Federation with SSO for multiple accounts with AWS Organizations
   - Web Identity Federation (not recommended)
   - Cognito for most web and mobile applications (has anonymous mode, MFA)
   - Active Directory on AWS:
     - Microsoft AD: standalone or setup trust AD with on-premise, has MFA, seamless join, RDS integration
     - AD Connector: proxy requests to on-premise
     - Simple AD: standalone & cheap AD-compatible with no MFA, no advanced capabilities
   - Single Sign On to connect to multiple AWS Accounts (Organization) and SAML apps
   
# Security Section

## AWS CloudTrail
   - Provides governance, compliance and audit for your AWS Account
   - CloudTrail is enabled by default!
   - Get an history of events / API calls made within your AWS Account by:
     - Console
     - SDK
     - CLI
     - AWS Services
   - Can put logs from CloudTrail into CloudWatch Logs
   - If a resource is deleted in AWS, look into CloudTrail first!
   - CloudTrail console shows the past 90 days of activity
   - The default UI only shows “Create”, “Modify” or “Delete” events
   - CloudTrail Trail:
     - Get a detailed list of all the events you choose
     - Can include events happening at the object level in S3
     - Ability to store these events in S3 for further analysis
     - Can be region specific or be global & include global events (IAM, etc)
   - S3 Enhancements:
     - Enable Versioning
     - MFA Delete Protection
     - S3 Lifecycle Policy (S3 IA, Glacier…)
     - S3 Object Lock
     - SSE-S3 or SSE-KMS encryption
     - Feature to perform CloudTrail Log File Integrity validation (SHA-256 for hashing and signing)

## CloudTrail - Solution Architecture:
   - Multi Account, Multi Region Logging Observations:
     - The S3 bucket policy is necessary for cross-account delivery
     - If Account A wants to access its CloudTrail files:
       - Option 1: create a cross-account role and assume the role
       - Option 2: edit the bucket policy
   - Alert for API calls
     - Log filter metrics can be used to detect a high level of API happening
     - Ex: Count occurrences of EC2 TerminateInstances API
     - Ex: Count of API calls per user
     - Ex: Detect high level of Denied API calls

## CloudTrail: How to react to events the fastest?
   - Overall, CloudTrail may take up to 15 minutes to deliver events
   - CloudWatch Events:
     - Can be triggered for any API call in CloudTrail
     - The fastest, most reactive way
   - CloudTrail Delivery in CloudWatch Logs:
     - Events are streamed
     - Can perform a metric filter to analyze occurrences and detect anomalies
   - CloudTrail Delivery in S3:
     - Events are delivered every 5 minutes
     - Possibility of analyzing logs integrity, deliver cross account, long-term storage

## AWS KMS (Key Management Service)
   - Anytime you hear “encryption” for an AWS service, it’s most likely KMS
   - Easy way to control access to your data, AWS manages keys for us
   - Fully integrated with IAM for authorization
   - Seamlessly integrated into:
     - Amazon EBS: encrypt volumes
     - Amazon S3: Server side encryption of objects
     - Amazon Redshift: encryption of data
     - Amazon RDS: encryption of data
     - Amazon SSM: Parameter store
     - Etc…
   - But you can also use the CLI / SDK

## AWS KMS 101
   - The value in KMS is that the CMK used to encrypt data can never be retrieved by the user, and the CMK can be rotated for extra security
   - Never ever store your secrets in plaintext, especially in your code!
   - Encrypted secrets can be stored in the code / environment variables
   - KMS can only help in encrypting up to 4KB of data per call
   - If data > 4 KB, use Envelope Encryption
   - To give access to KMS to someone:
     - Make sure the Key Policy allows the user
     - Make sure the IAM Policy allows the API calls
   - Track API calls made to KMS in CloudTrail

## Types of KMS Keys
   - Customer Manager CMK:
     - Create, manage and use, can enable or disable
     - Possibility of rotation policy (new key generated every year, old key preserved)
     - Can add a key policy (resource policy)
     - Leverage for envelope encryption
   - AWS managed CMK:
     - Used by AWS service (aws/s3, aws/ebs, aws/redshift)
     - Managed by AWS

## AWS Parameter Store
   - Secure storage for configuration and secrets
   - Optional Seamless Encryption using KMS
   - Serverless, scalable, durable, easy SDK, free
   - Version tracking of configurations / secrets
   - Configuration management using path & IAM
   - Notifications with CloudWatch Events
   - Integration with CloudFormation
   - Can retrieve secrets from Secrets Manager using the SSM Parameter Store API

## AWS Parameter Store Hierarchy
   - /my-department/
     - my-app/
       - dev/
         - db-url
         - db-password
       - prod/
         -  db-url
         - db-password
     - other-app/
   - /other-department/
   - /aws/reference/secretsmanager/secret_ID_in_Secrets_Manager
   - /aws/service/ami-amazon-linux-latest/amzn2-ami-hvm-x86_64-gp2

## AWS Secrets Manager
   - Newer service, meant for storing secrets
   - Capability to force rotation of secrets every X days
   - Automate generation of secrets on rotation (uses Lambda)
   - Integration with Amazon RDS (MySQL, PostgreSQL, Aurora)
   - Secrets are encrypted using KMS
   - Mostly meant for RDS integration

## RDS - Security
   - KMS encryption at rest for underlying EBS volumes / snapshots
   - Transparent Data Encryption (TDE) for Oracle and SQL Server
   - SSL encryption to RDS is possible for all DB (in-flight)
   - IAM authentication for MySQL and PostgreSQL
   - Authorization still happens within RDS (not in IAM)
   - Can copy an un-encrypted RDS snapshot into an encrypted one
   - CloudTrail cannot be used to track queries made within RDS

## SSL/TLS - Basics
   - SSL refers to Secure Sockets Layer, used to encrypt connections
   - TLS refers to Transport Layer Security, which is a newer version
   - Nowadays, TLS certificates are mainly used, but people still refer as SSL
   - Public SSL certificates are issued by Certificate Authorities (CA)
   - Comodo, Symantec, GoDaddy, GlobalSign, Digicert, Letsencrypt, etc…
   - SSL certificates have an expiration date (you set) and must be renewed

## SSL Encryption – How it works
   - Asymmetric Encryption is expensive (SSL)
   - Symmetric encryption is cheaper
   - Asymmetric handshake is used to exchange a per- client random symmetric key
   - Possibility of client sending an SSL certificate as well (two-way certificate) 
   - 1. Client sends hello, cipher suits & random
   - 2. Server Response with server random & SSL certificate (Public Key)
   - 3. Client verifies SSL certificate 
   - 4. Master key (symmetric) generated and sent encrypted using the Public Key 
   - 5. Server verifies Client SSL cert (optional)
   - 6. Master key is decrypted using Private Key
   - 7. Secure Symmetric Communication in Place

## SSL – Server Name Indication (SNI)
   - SNI solves the problem of loading multiple SSL certificates onto one web server (to serve multiple websites)
   - It’s a “newer” protocol, and requires the client to indicate the hostname of the target server in the initial SSL handshake
   - The server will then find the correct certificate, or return the default one
   - Note:
     - Only works for ALB & NLB (newer generation), CloudFront
     - Does not work for CLB (older gen) Client ALB

## SSL – Man in the Middle Attack How to prevent
   - 1. Don’t use public-facing HTTP, use HTTPS (meaning, use SSL/TLS certicates)
   - 2. Use a DNS that has DNSSEC
     - To end send a client to a pirate server, a DNS response needs to be “forged” by a server which intercepts them
     - It is possible to protect your domain name by configuring DNSSEC
     - Amazon Route 53 supports DNSSEC for domain registration. However, Route 53 does
not support DNSSEC for DNS service, regardless of whether the domain is registered
with Route 53. If you want to configure DNSSEC for a domain that is registered with
Route 53, you must use another DNS service provider.
     - You could run a custom DNS server on Amazon EC2 for example (Bind is the most
popular, dnsmasq, KnotDNS, PowerDNS).    

## AWS Certificate Manager (ACM)
   - To host public SSL certificates in AWS, you can:
     - Buy your own and upload them using the CLI
     - Have ACM provision and renew public SSL
certificates for you (free of cost)
   - ACM loads SSL certificates on the following
integrations:
     - Load Balancers (including the ones created by EB)
     - CloudFront distributions
     - APIs on API Gateways
   - SSL certificates is overall a pain to manually
manage, so ACM is great to leverage in your
AWS infrastructure!

## ACM – Good to know
   - Possibility of creating public certificates
     - Must verify public DNS
     - Must be issued by a trusted public certificate authority (CA)
   - Possibility of creating private certificates
     - For your internal applications
     - You create your own private CA
     - Your applications must trust your private CA
   - Certificate renewal:
     - Automatically done if generated provisioned by ACM
     - Any manually uploaded certificates must be renewed manually and re-uploaded
   - ACM is a regional service
     - To use with a global application (multiple ALB for example), you need to issue an SSL certificate
in each region where you application is deployed.
     - You cannot copy certs across regions

## CloudHSM
   - KMS => AWS manages the software for encryption
   - CloudHSM => AWS provisions encryption hardware
   - Dedicated Hardware (HSM = Hardware Security Module)
   - You manage your own encryption keys entirely (not AWS)
   - HSM device is tamper resistant, FIPS 140-2 Level 3 compliance
   - Supports both symmetric and asymmetric encryption (SSL/TLS keys)
   - No free tier available
   - Must use the CloudHSM Client Software
   - Redshift supports CloudHSM for database encryption and key management
   - Good option to use with SSE-C encryption
   - IAM permissions:
     - CRUD an HSM Cluster
   - CloudHSM Software:
     - Manage the Keys
     - Manage the Users

## CloudHSM – High Availability
   - CloudHSM clusters are spread across Multi AZ (HA)
   - Great for availability and durability

## CloudHSM vs KMS
Feature AWS KMS AWS CloudHSM
   - KMS 
     - Tenancy - Uses multi-tenant key storage 
     - Keys - Keys owned and managed by AWS
     - Encryption - Supports only symmetric key encryption
     - Cryptographic Acceleration - None 
     - Key Storage and Management - Accessible from multiple regions, Centralized management from IAM
     - Free Tier Availability - Yes
   - CloudHSM 
     - Tenancy - Single tenant key storage, dedicated to one customer
     - Keys - Customer managed Keys
     - Encryption - Supports both symmetric and asymmetric encryption
     - Cryptographic Acceleration - 	 SSL/TLS Acceleration, Oracle TDE Acceleration
     - Key Storage and Management - Deployed and managed from a customer VPC. Accessible and can be shared across VPCs using VPC peering
     - Free Tier Availability - No	 

## Solution Architecture: SSL on ALB
   - ALB with SSL cert from ACM

## Solution Architecture: SSL on web server EC2 instances
   - NLB
   - SSM Parameter Store
   - Retrieve SSL private key at EC2 boot time (user data)
   - Install certs on EC2
   - IAM permissions
   - Performing SSL encryption / decryption can use CPU resources 

## Solution Architecture: CloudHSM – SSL Offloading
   - You can offload SSL to 
CloudHSM (SSL
Acceleration)
   - Supported by NGINX &
Apache Web servers
   - Extra security: the SSL
private key never leaves the
HSM device
   - Must setup a cryptographic
user (CU) on the
CloudHSM device

## S3 Encryption for Objects
   - There are 4 methods of encrypting objects in S3
   - SSE-S3: encrypts S3 objects using keys handled & managed by AWS
   - SSE-KMS: leverage AWS Key Management Service to manage encryption
keys
   - SSE-C: when you want to manage your own encryption keys
   - Client Side Encryption
   - Glacier: all data is AES-256 encrypted, key under AWS control

## Encryption in transit (SSL)
   - AWS S3 exposes:
     - HTTP endpoint: non encrypted
     - HTTPS endpoint: encryption in flight
   - You’re free to use the endpoint you want, but HTTPS is recommended
   - HTTPS is mandatory for SSE-C
   - Encryption in flight is also called SSL / TLS 

## Events in S3 Buckets
   - S3 Access Logs:
     - Detailed records for the requests that are made to a bucket
     - Might take hours to deliver
     - Might be incomplete (best effort)
   - S3 Events Notifications:
     - Receive notifications when certain events happen in your bucket
     - E.g.: new objects created, object removal, restore objects, replication events
     - Destinations: SNS, SQS queue, Lambda
     - Typically delivered in seconds but can take minutes, notification for every object if versioning is
enabled, else risk of one notification for two same object write done simultaneously
   - Trusted Advisor:
     - Check the bucket permission (is the bucket public?)
   - CloudWatch Events:
     - Need to enable CloudTrail object level logging on S3 first
     - Target can be Lambda, SQS, SNS, etc… 

## S3 Security
   - User based
     - IAM policies - which API calls should be allowed for a specific user from IAM
console
   - Resource Based
     - Bucket Policies - bucket wide rules from the S3 console - allows cross account
     - Object Access Control List (ACL) – finer grain
     - Bucket Access Control List (ACL) – less common

## S3 Bucket Policies
   - Use S3 bucket for policy to:
     - Grant public access to the bucket
     - Force objects to be encrypted at upload
     - Grant access to another account (Cross Account)
   - Optional Conditions on:
     - Public IP or Elastic IP (not on Private IP)
     - Source VPC or Source VPC Endpoint – only works with VPC Endpoints
     - CloudFront Origin Identity
     - MFA
   - Examples here: https://docs.aws.amazon.com/AmazonS3/latest/dev/examplebucket-policies.html

## S3 pre-signed URLs
   - Can generate pre-signed URLs using SDK or CLI
     - For downloads (easy, can use the CLI)
     - For uploads (harder, must use the SDK)
   - Valid for a default of 3600 seconds, can change timeout with --expires-in
[TIME_BY_SECONDS] argument
   - Users given a pre-signed URL inherit the permissions of the person who generated the URL for GET / PUT
   - Examples :
     - Allow only logged-in users to download a premium video on your S3 bucket
     - Allow an ever changing list of users to download files by generating URLs dynamically
     - Allow temporarily a user to upload a file to a precise location in our bucket

## VPC Endpoint Gateway for S3 VPC Endpoint
   - IGW 
     - public
     - Bucket policy by AWS:SourceIP (public IP)
	 - ElasticIP
   - VPC Endpoint Gatweay
     - private
	 - Bucket policy by
       - AWS:SourceVpce (one or few endpoints)
       - AWS:SourceVpc (encompass all possible VPC endpoints)
	   
## S3 Object Lock & Glacier Vault Lock
   - S3 Object Lock
     - Adopt a WORM (Write Once Read
Many) model
     - Block an object version deletion for a
specified amount of time
   - Glacier Vault Lock
     - Adopt a WORM (Write Once Read
Many) model
     - Lock the policy for future edits (can no
longer be changed)
     - Helpful for compliance and data retention
Object

## Network Security    
   - Security Groups    
     - Attached to ENI (Elastic Network Interfaces)
     – EC2, RDS, Lambda in VPC, etc
     - Are stateful (any traffic in is allowed to go out, any traffic
out can go back in)
     - Can reference by CIDR and security group id    - Supports security group references for VPC peering    - Default: inbound denied, outbound all allowed
   - NACL (Network ACL):    
     - Attached at the subnet level    
	 - Are stateless (inbound and outbound rules apply for all traffic)    
	 - Can only reference a CIDR range (no hostname)    
	 - Default: allow all inbound, allow all outbound    
	 - New NACL: denies all inbound, denies all outbound    
   - Host Firewall    
   - Software based, highly customizable NACL

## Type of Attacks on your infrastructure
   - Distributed Denial of Service (DDoS):
     - When your service is unavailable because it’s receiving too many requests
     - SYN Flood (Layer 4): send too many TCP connection requests
     - UDP Reflection (Layer 4): get other servers to send many big UDP requests
     - DNS flood attack: overwhelm the DNS so legitimate users can’t find the site
     - Slow Loris attack: a lot of HTTP connections are opened and maintained
   - Application level attacks:
     - more complex, more specific (HTTP level)
     - Cache bursting strategies: overload the backend database by invalidating cache

## DDoS Protection on AWS
   - AWS Shield Standard: protects against DDoS attack for your website and applications, for all customers at no additional costs
   - AWS Shield Advanced: 24/7 premium DDoS protection
   - AWS WAF: Filter specific requests based on rules
   - CloudFront and Route 53:
     - Availability protection using global edge network
     - Combined with AWS Shield, provides DDoS attack mitigation at the edge
   - Be ready to scale – leverage AWS Auto Scaling
   - Separate static resources (S3 / CloudFront) from dynamic ones (EC2 / ALB)
   - Read the whitepaper for details:
https://d1.awsstatic.com/whitepapers/Security/DDoS_White_Paper.pdf

## Sample Reference Architecture
   - https://aws.amazon.com/answers/networking/aws-ddos-attack-mitigation/

## AWS Shield
   - AWS Shield Standard:
     - Free service that is activated for every AWS customer
     - Provides protection from attacks such as SYN/UDP Floods, Reflection attacks
and other layer 3/layer 4 attacks
   - AWS Shield Advanced:
     - Optional DDoS mitigation service ($3,000 per month per organization)
     - Protect against more sophisticated attack on Amazon EC2, Elastic Load
Balancing (ELB), Amazon CloudFront, AWS Global Accelerator, and Route 53
     - 24/7 access to AWS DDoS response team (DRP)
     - Protect against higher fees during usage spikes due to DDoS

## AWS WAF – Web Application Firewall
   - Protects your web applications from common web exploits (Layer 7)
   - Deploy on Application Load Balancer (localized rules)
   - Deploy on API Gateway (rules running at the regional or edge level)
   - Deploy on CloudFront (rules globally on edge locations)
     - Used to front other solutions: CLB, EC2 instances, custom origins, S3 websites)
   - WAF is not for DDoS protection
   - Define Web ACL (Web Access Control List):
     - Rules can include: IP addresses, HTTP headers, HTTP body, or URI strings
     - Protects from common attack - SQL injection and Cross-Site Scripting (XSS)
     - Size constraints, Geo match
     - Rate-based rules (to count occurrences of events)

## AWS Firewall Manager
   - Manage rules in all accounts of an AWS Organization
   - Common set of security rules
   - WAF rules (Application Load Balancer, API Gateways, CloudFront)
   - AWS Shield Advanced (ALB, CLB, Elastic IP, CloudFront)
   - Security Groups for EC2 and ENI resources in VPC

## Blocking an IP address - EC2 Instance
   - Public IP
   - Optional Firewall Software in EC2
   - Security group
   - NACL

## Blocking an IP address – with an ALB
   - EC2 Instance
   - Private IP
   - ALB Security group
   - EC2 Security group
   - NACL
   - Application Load Balancer Connection Termination

## Blocking an IP address – with an NLB
   - NACL
   - Network Load Balancer Traffic goes through
   - No Security Group for NLB

## Blocking an IP address – ALB + WAF
   - attach WAF at ALB
   - IP address filtering

## Blocking an IP address – ALB, CloudFront WAF
   - attach WAF at CloudFront
   - IP address filtering
   - CloudFront Geo Restriction
   - CloudFront Public IPs
   - NACL not helpful

## AWS Inspector
   - Only for EC2 instances (started from an AMI)
   - Analyze the running OS against known vulnerabilities
   - Analyze against unintended network accessibility
   - AWS Inspector Agent must be installed on OS in EC2 instances
   - Define template (rules package, duration, attributes, SNS topics)
   - No own custom rules possible – only use AWS managed rules
   - After the assessment, you get a report with a list of vulnerabilities

## AWS Config
   - Helps with auditing and recording compliance of your AWS resources
   - Helps record configurations and changes over time
   - AWS Config Rules does not prevent actions from happening (no deny)
   - Questions that can be solved by AWS Config:
     - Is there unrestricted SSH access to my security groups?
     - Do my buckets have any public access?
     - How has my ALB configuration changed over time?
   - You can receive alerts (SNS notifications) for any changes
   - AWS Config is a per-region service
   - Can be aggregated across regions and accounts
   - AWS Config Resource    
     - View compliance of a resource over time    
	 - View configuration of a resource over time    
	 - View CloudTrail API calls if enabled

## AWS Config Rules
   - Can use AWS managed config rules (over 75)
   - Can make custom config rules (must be defined in AWS Lambda)
     - Evaluate if each EBS disk is of type gp2
     - Evaluate if each EC2 instance is t2.micro
   - Rules can be evaluated / triggered:
     - For each config change
     - And / or: at regular time intervals
     - Can trigger CloudWatch Events if the rule is non-compliant (and chain with Lambda)
   - Rules can have auto remediations:
     - If a resource is not compliant, you can trigger an auto remediation
     - Define the remediation through SSM Automations
     - Ex: remediate security group rules, stop instances with non-approved tags

## AWS Managed Logs
   - Load Balancer Access Logs (ALB, NLB, CLB) => to S3
     - Access logs for your Load Balancers
   - CloudTrail Logs => to S3 and CloudWatch Logs
     - Logs for API calls made within your account
   - VPC Flow Logs => to S3 and CloudWatch Logs
     - Information about IP traffic going to and from network interfaces in yourVPC
   - Route 53 Access Logs => to CloudWatch Logs
     - Log information about the queries that Route 53 receives
   - S3 Access Logs => to S3
     - Server access logging provides detailed records for the requests that are made to a bucket
   - CloudFront Access Logs => to S3
     - Detailed information about every user request that CloudFront receives
   - AWS Config => to S3

## GuardDuty
   - Intelligent Threat discovery to Protect AWS Account
   - Uses Machine Learning algorithms, anomaly detection, 3rd party data
   - One click to enable (30 days trial), no need to install software
   - Input data includes:
     - CloudTrail Logs: unusual API calls, unauthorized deployments
     - VPC Flow Logs: unusual internal traffic, unusual IP address
     - DNS Logs: compromised EC2 instances sending encoded data within DNS queries
   - Can setup CloudWatch Event rules to be notified in case of findings
   - CloudWatch Events rules can target AWS Lambda or SNS
   
# Compute and Load Balancing

## EC2 Instance Types 
   - R: applications that needs a lot of RAM – in-memory caches
   - C: applications that needs good CPU – compute / databases
   - M: applications that are balanced (think “medium”) – general / web app
   - I: applications that need good local I/O (instance storage) – databases
   - G: applications that need a GPU – video rendering / machine learning
   - T2 / T3: burstable instances (up to a capacity)
   - T2 / T3 - unlimited: unlimited burst
   - Real-world tip: use https://www.ec2instances.info

## EC2 - Placement Groups
   - Control the EC2 Instance placement strategy using placement groups
   - Group Strategies:
     - Cluster—clusters instances into a low-latency group in a single Availability Zone
     - Spread—spreads instances across underlying hardware (max 7 instances per group per
AZ) – critical applications
     - Partition—spreads instances across many different partitions (which rely on different sets
of racks) within an AZ. Scales to 100s of EC2 instances per group (Hadoop, Cassandra,
Kafka)
   - You can move an instance into or out of a placement group
     - Your first need to stop it
     - You then need to use the CLI (modify-instance-placement)
     - You can then start your instance
     
     
 ## Tips 
 - For EC2 instances, always use a Type A Record without an Alias. For ELB, Cloudfront and S3, always use a Type A Record with an Alias and finally, for RDS, always use the CNAME Record with no Alias.

- After you’ve created your VPC, you further expand your network by adding associating one to utmost 4 secondary CIDR blocks to your VPC.

- Data Pipeline is for batch jobs

- Lambda can be triggered from SQS
- In Multi-AZ RDS,  standby instance cannot be accessed for read

- HTTPS between viewers and CloudFront
    – You can use a certificate that was issued by a trusted certificate authority (CA) such as Comodo, DigiCert, Symantec or other third-party providers.
    – You can use a certificate provided by AWS Certificate Manager (ACM)
HTTPS between CloudFront and a custom origin
    – If the origin is not an ELB load balancer, such as Amazon EC2, the certificate must be issued by a trusted CA such as Comodo, DigiCert, Symantec or other third-party providers.
    – If your origin is an ELB load balancer, you can also use a certificate provided by ACM.
- Although the on-premises data center is using a tape gateway, you can still set up a solution to use a file gateway in order to properly process the videos using Amazon Rekognition. Keep in mind that the tape gateway in AWS Storage Gateway service is primarily used as an archive solution.  That’s glacier
- AWS Organizations, SCPs DO NOT affect any service-linked role. Service-linked roles enable other AWS services to integrate with AWS Organizations and can't be restricted by SCPs
- You can use the same SSL certificate from ACM in more than one AWS Region but it depends on whether you’re using Elastic Load Balancing or Amazon CloudFront. To use a certificate with Elastic Load Balancing for the same site (the same fully qualified domain name, or FQDN, or set of FQDNs) in a different Region, you must request a new certificate for each Region in which you plan to use it. To use an ACM certificate with Amazon CloudFront, you must request the certificate in the US East (N. Virginia) region.
- Amazon EC2 now allows peering relationships to be established between Virtual Private Clouds (VPCs) across different AWS regions. Inter-Region VPC Peering allows VPC resources like EC2 instances, RDS databases, and Lambda functions running in different AWS regions to communicate with each other using private IP addresses, without requiring gateways, VPN connections or separate network appliances.
- With AWS Certificate Manager, you can generate public or private SSL/TLS certificates that you can use to secure your site. Public SSL/TLS certificates provisioned through AWS Certificate Manager are free. You pay only for the AWS resources that you create to run your application. For private certificates, the ACM Private Certificate Authority (CA) is priced along two dimensions: (1) You pay a monthly fee for the operation of each private CA until you delete it and (2) you pay for the private certificates you issue each month.
- Public certificates generated from ACM can be used on Amazon CloudFront, Elastic Load Balancing, or Amazon API Gateway but not directly on EC2 instances, unlike private certificates.
- Gateway-Cached volumes can support volumes of 1,024TB in size, whereas Gateway-stored volume supports volumes of 512 TB size.
- Service Control Policies (SCP) vs IAM Policies:
https://tutorialsdojo.com/aws-cheat-sheet-service-control-policies-scp-vs-iam-policies/
- Service Control Policies (SCP) vs IAM Policies  References: https://aws.amazon.com/premiumsupport/knowledge-center/iam-policy-service-control-policy/
- SCPs are available only when you enable all features in your organization.
- network device that supports Border Gateway Protocol (BGP) and BGP MD5 authentication is needed to establish a Direct Connect link from your data center to your VPC
- Global accelerator
- Transit Gateway
- only one virtual private gateway (VGW) can be attached to a VPC at a time
- APIGateway error codes
- Redshift - Automated snapshots are enabled by default when you create a cluster. cross-region snapshot copy is not by default
- certificate can be stored in AWS Certificate Manager (ACM) or in IAM
- SNS can be used to fan out notifications to end users using mobile push, SMS, and email.


## Placement Groups Cluster
   - Pros: Great network (10 Gbps bandwidth between instances)
   - Cons: If the rack fails, all instances fails at the same time
   - Note: choose than instance type that has Enhanced Networking
   - Use case:
   - Big Data job that needs to complete fast
   - Application that needs extremely low latency and high network throughput

## Placement Groups Spread
   - Pros:
     - Can span across Availability
Zones (AZ)
     - Reduced risk is simultaneous
failure
     - EC2 Instances are on different
physical hardware
   - Cons:
     - Limited to 7 instances per AZ
per placement group
   - Use case:
     - Application that needs to
maximize high availability
     - Critical Applications where
each instance must be isolated
from failure from each other

## Placements Groups Partition
   - Up to 7 partitions per AZ
   - Up to 100s of EC2 instances
   - The instances in a partition do
not share racks with the instances
in the other partitions
   - A partition failure can affect many
EC2 but won’t affect other
partitions
   - EC2 instances get access to the
partition information as metadata
   - Use cases: HDFS, HBase,
Cassandra, Kafka

## EC2 Instance Launch Types
   - On Demand Instances: short workload, predictable pricing, reliable
   - Spot Instances: short workloads, for cheap, can lose instances (not reliable)
   - Reserved: (MINIMUM 1 year)
     - Reserved Instances: long workloads
     - Convertible Reserved Instances: long workloads with flexible instances
     - Scheduled Reserved Instances: example – every Thursday between 3 and 6 pm
   - Dedicated Instances: no other customers will share your hardware
   - Dedicated Hosts: book an entire physical server, control instance placement
     - Great for software licenses that operate at the core, or CPU socket level
     - Can define host affinity so that instance reboots are kept on the same host

## EC2 included metrics    
   - CPU: CPU Utilization + Credit Usage / Balance    
   - Network: Network In / Out    
   - Status Check:    
     - Instance status = check the EC2 VM    
     - System status = check the underlying hardware    
   - Disk: Read / Write for Ops / Bytes (only for instance store)    
   - RAM is NOT included in the AWS EC2 metrics

## EC2 Instance Recovery
   - CloudWatch Alarm - StatusCheckFailed_System
   - Status Check:
     - Instance status = check the EC2 VM
     - System status = check the underlying hardware
   - Recovery: Same Private, Public, Elastic IP, metadata, placement group

## Auto Scaling – Scaling Policies
   - Simple / Step Scaling: increase or decrease instances based on two CW
alarms
   - Target Tracking: select a metric and a target value, ASG will smartly
adjust
     - Keep average CPU at 40%
     - Keep request count per target at 1000
   - To scale based on RAM, you must use a Custom CloudWatch Metric

## Auto Scaling – Good to know
   - Spot Fleet support (mix on Spot and On-Demand instances)
   - To upgrade an AMI, must update the launch configuration / template
     - You must terminate instances manually
     - CloudFormation can help with that step (we’ll see it later)
   - Scheduled scaling actions:
     - Modify the ASG settings (min / max / desired) at pre-defined time
     - Helpful when patterns are known in advance
   - Lifecycle Hooks:
     - Perform actions before an instance is in service, or before it is terminated
     - Examples: cleanup, log extraction, special health checks

Auto Scaling – Scaling Processes
   - Launch: Add a new EC2 to the group, increasing the capacity
   - Terminate: Removes an EC2 instance from the group, decreasing its capacity.
   - HealthCheck: Checks the health of the instances
   - ReplaceUnhealthy:Terminate unhealthy instances and re-create them
   - AZRebalance: Balancer the number of EC2 instances across AZ
   - AlarmNotification: Accept notification from CloudWatch
   - ScheduledActions: Performs scheduled actions that you create.
   - AddToLoadBalancer: Adds instances to the load balancer or target group
   - We can suspend these processes!

## Auto Scaling – Health Checks
   - Health checks available:
     - EC2 Status Checks
     - ELB Health Checks (HTTP)
   - ASG will launch a new
instance after terminating
an unhealthy one
   - Make sure the health check
is simple and checks the
correct thing
   - GOOD HEALTH CHECK /health-server  
   - BAD HEALTH CHECK  /number-customers DB call

## EC2 Spot Instances
   - Can get a discount of up to 90% compared to On-demand
   - Define max spot price and get the instance while current spot price < max
     - The hourly spot price varies based on offer and capacity
     - If the current spot price > your max price you can choose to stop or terminate your
instance with a 2 minutes grace period.
   - Other strategy: Spot Block
     - “block” spot instance during a specified time frame (1 to 6 hours) without interruptions
     - In rare situations, the instance may be reclaimed
   - Used for batch jobs, data analysis, or workloads that are resilient to failures.
   - Not great for critical jobs or databases

## Spot Fleets
   - Collection (Fleet) of Spot Instances and optionally on-demand instances
   - Set a maximum price you’re willing to pay per Spot Instances or all
   - Can have a mix of instance types (M5.large, M5.xlarge, C5.2xlarge, etc..)
   - Supports: EC2 standalone, Auto Scaling Groups (launch template), ECS
(underlying ASG), AWS Batch (Managed Compute Environment)
   - Soft limits:
     - Target capacity per Spot Fleet or EC2 fleet: 10,000
     - Target capacity across all Spot Fleet and EC2 Fleet in a region: 100,000

## AWS ECS – Elastic Container Service
   - ECS is a container orchestration service
   - ECS helps you run Docker containers on EC2 machines
   - ECS is complicated, and made of:
     - “ECS Core”: Running ECS on user-provisioned EC2 instances
     - Fargate: Running ECS tasks on AWS-provisioned compute (serverless)
     - EKS: Running ECS on AWS-powered Kubernetes (running on EC2)
     - ECR: Docker Container Registry hosted by AWS
   - ECS & Docker are very popular for microservices

## What’s Docker?
   - Docker is a “container technology”
   - Run a containerized application on any machine with Docker installed
   - Containers allows our application to work the same way anywhere
   - Containers are isolated from each other
   - Control how much memory / CPU is allocated to your container
   - Ability to restrict network rules
   - More efficient than Virtual machines
   - Scale containers up and down very quickly (seconds)

## AWS ECS – Use cases
   - Run microservices
     - Ability to run multiple docker containers on the same machine
     - Easy service discovery features to enhance communication
     - Direct integration with Application Load Balancers
     - Auto scaling capability
   - Run batch processing / scheduled tasks
     - Schedule ECS containers to run on On-demand / Reserved / Spot instances
   - Migrate applications to the cloud
     - Dockerize legacy applications running on premise
     - Move Docker containers to run on ECS

## AWS ECS – Concepts
   - ECS cluster: set of EC2
instances
   - ECS service: applications
definitions running on ECS
cluster
   - ECS tasks + definition:
containers running to create
the application
   - ECS IAM roles: roles assigned
to tasks to interact with
AWS

## AWS ECS – ALB integration
   - Application Load Balancer (ALB)
has a direct integration feature
with ECS called “port mapping”
   - This allows you to run multiple
instances of the same application
on the same EC2 machine
   - Use cases:
     - Increased resiliency even if running
on one EC2 instance
     - Maximize utilization of CPU / cores
     - Ability to perform rolling upgrades
without impacting application uptime

## Fargate
   - When launching an ECS Cluster, we have to create our EC2 instances
   - If we need to scale, we need to add EC2 instances
   - So we manage infrastructure…
   - With Fargate, it’s all Serverless!
   - We don’t provision EC2 instances
   - We just create task definitions, and AWS will run our containers for us
   - To scale, just increase the task number. Simple! No more EC2 J

## ECS – Security & Networking
   - IAM security
     - EC2 Instance Role must have basic ECS permissions
     - ECS Task level should have an IAM Task Role (maximum security)
   - Secrets and Configuration injection into parameters, environment variables:
     - Integration with SSM Parameter Store & Secrets Manager
   - Tasks networking:
     - none: no network connectivity, no port mappings
     - bridge: uses Docker’s virtual container-based network
     - host: bypass Docker’s network, uses the underlying host network interface
     - awsvpc:
       - Every tasks launched on the instance gets its own ENI and a private IP address
       - Simplified networking, enhanced security, security groups, monitoring, VPC flow logs
       - Default mode for Fargate 

## ECS – Service Auto Scaling
   - CPU and RAM is tracked in CloudWatch at the ECS service level
   - Target Tracking: target a specific average CloudWatch metric
   - Step Scaling: scale based on CloudWatch alarms
   - Scheduled Scaling: based on predictable changes
   - ECS Service Scaling (task level) ≠ EC2 Auto Scaling (instance level)
   - Fargate Auto Scaling is much easier to setup (because serverless)

## ECS – Spot Instances
   - ECS Classic:
     - Can have the underlying EC2 instances as Spot Instances (managed by an ASG)
     - Instances may go into draining mode to remove running tasks
     - Good for cost savings, but will impact reliability
   - Fargate: Spot Instances are available as of Dec 2019:
     - Specify minimum of tasks for on-demand baseline workload
     - Add tasks running on Fargate Spot for cost-savings (can be reclaimed by AWS)
     - Regardless of On-demand or Spot, Fargate scales well based on load

## AWS Lambda Integrations
   - API Gateway 
   - Kinesis 
   - DynamoDB 
   - AWS S3 –
   - CloudWatch Events 
   - CloudWatch Logs 
   - AWS SNS 
   - AWS Cognito
   - AWS IoT
   - SQS
   - Example: Serverless Thumbnail creation
   - Example: Serverless CRON Job

## AWS Lambda Language Support (runtimes)
   - AWS supported: Node.js (JavaScript), Python, Ruby, Java (Java 8
compatible), Golang, C# (.NET Core), C# / Powershell
   - Ability to write / use a custom runtime (community supported):
     - Ex: C++, Rust, etc…
   - If Docker, you should use ECS, Fargate or Batch, not Lambda


Lambda – Limits to know
   - RAM: 128 MB to 3G
   - CPU:
     - is linked to RAM (cannot be set manually)
     - 2 vCPU are allocated after 1.5G of RAM
   - Timeout: up to 15 minutes
   - /tmp storage: 512 MB (can’t process BIG files)
   - Deployment package limit: 250 MB including layers
   - Concurrency execution: 1000 – soft limit that can be increased

## Lambda – Latencies Considerations
   - Lambda Latency:
     - Cold Lambda Invocation: ~100ms
     - Warm Lambda Invocation: ~ms
     - New feature of “provisioned concurrency”
(Dec 2019) to reduce # of cold starts
   - API Gateway invocation: 100 ms
   - CloudFront invocation: 100 ms
   - If you chain with other services (API
Gateway, CloudFront, ALB, Lambda, SQS,
Step Functions…), add their latencies as
well
   - X-Ray can help visualize the end-to-end
latency

## Lambda - Security
   - IAM Roles for Lambda to grant
access to other AWS services
   - Resource-based Policies for
Lambda (similar to S3 bucket
policies):    
     - Allow other accounts to invoke or
manage Lambda
     - Allow other services to invoke or
manage Lambda
(define through the CLI)
write

## Lambda in a VPC
   - to allow lambda to communicate with resources in private network 
   - Note: Lambda - CloudWatch Logs works even without endpoint or NAT Gateway

## AWS Lambda Logging, Monitoring and Tracing
   - CloudWatch:
     - AWS Lambda execution logs are stored in AWS CloudWatch Logs
     - AWS Lambda metrics are displayed in AWS CloudWatch Metrics (successful
invocations, error rates, latency, timeouts, etc…)
     - Make sure your AWS Lambda function has an execution role with an IAM policy
that authorizes writes to CloudWatch Logs
   - X-Ray:
     - It’s possible to trace Lambda with X-Ray
     - Enable in Lambda configuration (runs the X-Ray daemon for you)
     - Use AWS SDK in Code
     - Ensure Lambda Function has correct IAM Execution Role

## Lambda – Synchronous Invocations
   - Synchronous: CLI, SDK, API Gateway
     - Results is returned right away
     - Error handling must happen client side (retries, exponential backoff, etc…)

## Lambda – Asynchronous Invocation
   - S3, SNS, CloudWatch Events…
   - Lambda attempts to retry on
errors (3 tries total)
   - Make sure the processing is
idempotent (in case of retries)
   - Can define a DLQ (dead-letter
queue) – SNS or SQS – for
failed processing

## Lambda – Event Source Mapping
   - Kinesis Data Streams, SQS, SQS FIFO
queue, DynamoDB Streams
   - Common denominator: records need
to be polled from the source
   - All records are respect ordering
properties except for SQS standard
   - If your function returns an error, the
entire batch is reprocessed until
success
     - Kinesis, DynamoDB Stream: stop shard
processing
     - SQS FIFO: stop, unless a SQS DLQ has
been defined
     - Need to make sure your Lambda
function is idempotent

Lambda – Destinations
   - Nov 2019: Can configure to send result to a
destination
   - Asynchronous invocations - can define destinations for
successful and failed event:
     - Amazon SQS
     - Amazon SNS
     - AWS Lambda
     - Amazon EventBridge bus
   - Note: AWS recommends you use destinations instead of
DLQ now (but both can be used at the same time)
   - Event Source mapping: for discarded event batches    
     - Amazon SQS
     - Amazon SNS
   - Note: you can send events to a DLQ directly from SQS
https://docs.aws.amazon.com/lambda/latest/dg/invocation-eventsourcemapping.html
https://docs.aws.amazon.com/lambda/latest/dg/invocation-async.html

## AWS Lambda Versions    
   - When you work on a Lambda function,
we work on $LATEST
   - When we’re ready to publish a Lambda
function, we create a version
   - Versions are immutable    
   - Versions have increasing version numbers    
   - Versions get their own ARN (Amazon
Resource Name)
   - Version = code + configuration (nothing
can be changed - immutable)
   - Each version of the lambda function can
be accessed

## AWS Lambda Aliases    
   - Aliases are ”pointers” to Lambda
function versions
   - We can define a “dev”, ”test”,
“prod” aliases and have them point
at different lambda versions
   - Aliases are mutable    - Aliases enable Blue / Green
deployment by assigning weights to
lambda functions
   - Aliases enable stable configuration
of our event triggers / destinations
   - Aliases have their own ARNs

## AWS Lambda Aliases with API Gateway

## Lambda & CodeDeploy
   - CodeDeploy can help you automate traffic shift for Lambda aliases
   - Feature is integrated within the SAM
framework
   - Linear: grow traffic every N minutes until 100%
     - Linear10PercentEvery3Minutes
     - Linear10PercentEvery10Minutes
   - Canary: try X percent then 100%
     - Canary10Percent5Minutes
     - Canary10Percent30Minutes
   - AllAtOnce: immediate
   - Can create Pre & Post Traffic hooks to
check the health of the Lambda function

## Types of load balancer on AWS
   - AWS has 3 kinds of managed Load Balancers
   - Classic Load Balancer (v1 - old generation) – 2009
     - HTTP, HTTPS, TCP
   - Application Load Balancer (v2 - new generation) – 2016
     - HTTP, HTTPS, WebSocket
   - Network Load Balancer (v2 - new generation) – 2017
     - TCP, TLS (secure TCP) & UDP
   - Overall, it is recommended to use the newer / v2 generation load balancers as they
provide more features
   - You can setup internal (private) or external (public) ELBs

## Classic Load Balancers (v1)
   - Health Checks can be HTTP (L7) or TCP (L4) based
   - Supports only one SSL certificate
     - The SSL certificate can have many SAN (Subject Alternate Name), but the SSL
certificate must be changed anytime a SAN is added / edited / removed
     - Better to use ALB with SNI (Server Name Indication) if possible
     - Can use multiple CLB if you want distinct SSL certificates
   - TCP => TCP passes all the traffic to the EC2 instance
     - Only way to use 2-way SSL authentication 

## Application Load Balancer (v2)
   - Application load balancers is Layer 7 (HTTP)
   - Load balancing to multiple HTTP applications across machines
(target groups)
   - Load balancing to multiple applications on the same machine
(ex: containers)
   - Support for HTTP/2 and WebSocket
   - Support redirects (from HTTP to HTTPS for example)
   - Routing tables to different target groups:
     - Routing based on path in URL (example.com/users & example.com/posts)
     - Routing based on hostname in URL (one.example.com & other.example.com)
     - Routing based on Query String, Headers
(example.com/users?id=123&order=false)
   - ALB are a great fit for micro services & container-based application
(example: Docker & Amazon ECS)
   - Has a port mapping feature to redirect to a dynamic port in ECS
   - In comparison, we’d need multiple Classic Load Balancer per application
   - Target Groups:
     - EC2 instances (can be managed by an ASG) – HTTP
     - ECS tasks (managed by ECS itself) – HTTP
     - Lambda functions – HTTP request is translated into a JSON event
     - IP Addresses – must be private IPs (ex: instances in peered VPC, on-premise)
     - ALB can route to multiple target groups
     - Health checks are at the target group level
   - SSL certificates:
     - Supports multiple listeners
     - Supports SNI - Server Name Indication

## Network Load Balancer (v2)    
   - Network load balancers (Layer 4) allow to do:    
   - Forward TCP traffic to your instances (UDP support
– Jun 2019)
   - Handle millions of request per seconds    
   - NLB has one static IP per AZ, and supports assigning Elastic IP
(helpful for whitelisting specific IP)
   - Less latency ~100 ms (vs 400 ms for ALB)    
   - Support for TLS    
   - Support for WebSockets
   - Network Load Balancers are mostly used:    
     - for extreme performance, TCP or UDP traffic    
	 - with AWS Private Link to expose a service internally
   - Target Groups:
     - EC2 instances (can be managed by an ASG) –TCP
     - ECS tasks (managed by ECS itself) –TCP
     - IP addresses – Private IP only, even outside your VPC
   - Proxy Protocol:
     - Send additional connection information such as the source and destination
     - The load balancer prepends a proxy protocol header to the TCP data
     - Helpful when you have the “IP addresses” target group type
    - You can retrieve the source IP address of the originating client

## Cross Zone Load Balancing
   - With Cross Zone Load
Balancing: each load
balancer instance
distributes evenly
across all registered
instances in all AZ
   - Otherwise, each load
balancer node
distributes requests
evenly across the
registered instances in
its Availability Zone
only.
   - Classic Load Balancer    
     - Disabled by default    
	 - No charges for inter AZ data if enabled    
   - Application Load Balancer    
     - Always on (can’t be disabled)    
	 - No charges for inter AZ data    
   - Network Load Balancer    
	 - Disabled by default    
	 - You pay charges ($) for inter AZ data if enabled

## Load Balancer Stickiness    
   - It is possible to implement stickiness so that
the same client is always redirected to the
same instance behind a load balancer
   - This works for Classic Load Balancers &
Application Load Balancers
   - The “cookie” used for stickiness has an
expiration date you control
   - Use case: make sure the user doesn’t lose his
session data
   - Enabling stickiness may bring imbalance to the
load over the backend EC2 instances
   - Alternative is to cache session data in
ElastiCache, DynamoDB for example

## API Gateway – Overview
   - Helps expose Lambda, HTTP & AWS Services as an API
   - API versioning, authorization, traffic management (API keys, throttles),
huge scale, serverless, req/resp transformations, OpenAPI spec, CORS
   - Limits to know:
     - 29 seconds timeout
     - 10 MB max payload size
	 
## API Gateway – Deployment Stages
   - API changes are deployed to “Stages” (as many as you want)
   - Use the naming you like for stages (dev, test, prod)
   - Stages can be rolled back as a history of deployments is kept

## API Gateway – Integrations
   - HTTP
     - Expose HTTP endpoints in the backend
     - Example: internal HTTP API on premise, Application Load Balancer…
     - Why? Add rate limiting, caching, user authentications, API keys, etc…
   - Lambda Function
     - Invoke Lambda function
     - Easy way to expose REST API backed by AWS Lambda
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
     - Could manually combine with CloudFront (more control over the caching
strategies and the distribution)
   - Private:
     - Can only be accessed from your VPC using an interface VPC endpoint (ENI)
     - Use a resource policy to define access

## Caching API responses
   - Caching reduces the number of calls made to the
backend
   - Default TTL (time to live) is 300 seconds (min: 0s, max: 3600s)
   - Caches are defined per stage
   - Possible to override cache settings per method
   - Clients can invalidate the cache with header:
Cache-Control: max-age=0 (with proper IAM authorization)
   - Able to flush the entire cache (invalidate it)
immediately
   - Cache encryption option
   - Cache capacity between 0.5GB to 237GB
   
## API Gateway - Errors
   - 4xx means Client errors
     - 400: Bad Request
     - 403: Access Denied, WAF filtered
     - 429: Quota exceeded, Throttle
   - 5xx means Server errors
     - 502: Bad Gateway Exception, usually for an incompatible output returned from a
Lambda proxy integration backend and occasionally for out-of-order invocations due to
heavy loads.
     - 503: Service Unavailable Exception
     - 504: Integration Failure – ex Endpoint Request Timed-out Exception API Gateway requests time out after 29 second maximum

API Gateway – Security
   - Load SSL certificates and use Route53 to define a CNAME
   - Resource Policy (~S3 Bucket Policy):
     - control who can access the API
     - Users from AWS accounts, IP or CIDR blocks, VPC or VPC Endpoints
   - IAM Execution Roles for API Gateway at the API level
     - To invoke a Lambda Function, an AWS service…
   - CORS (Cross-origin resource sharing):
     - Browser based security
     - Control which domains can call your API

## API Gateway – Authentication
   - IAM based access
     - Good for providing access within your own
infrastructure
     - Pass IAM credentials in headers through Sig V4
   - Lambda Authorizer (formerly Custom
Authorizer)
     - Use Lambda to verify a custom OAuth / SAML /
3rd party authentication
   - Cognito User Pools
     - Client authenticates with Cognito
     - Client passes the token to API Gateway
     - API Gateway knows out-of-the-box how to verify to token

## API Gateway – Logging, Monitoring, Tracing
   - CloudWatch Logs:
     - Enable CloudWatch logging at the Stage level (with Log Level – ERROR, INFO)
     - Can log full requests / responses data
     - Can send API Gateway Access Logs (customizable)
     - Can send logs directly into Kinesis Data Firehose (as an alternative to CW logs)
   - CloudWatch Metrics:
     - Metrics are by stage, possibility to enable detailed metrics
     - IntegrationLatency, Latency, CacheHitCount, CacheMissCount
   - X-Ray:
     - Enable tracing to get extra information about requests in API Gateway
     - X-Ray API Gateway + AWS Lambda gives you the full picture

## Route 53 – Records
   - Route53 is a Managed DNS (Domain Name System)    
   - A: hostname to IPv4    
   - AAAA: hostname to IPv6    
   - CNAME: hostname to hostname    
   - Alias: hostname to AWS resource    
     - Use for: CLB, ALB, NLB, CloudFront, S3 bucket, Elastic Beanstalk    
     - Can be used for root apex record (mydomain.com)    
   - Other record types are not needed for the exam

## DNS Records TTL (Time to Live)
   - High TTL: (e.g. 24hr)
     - Less traffic on DNS
     - Possibly outdated
records
   - Low TTL: (e.g 60 s)
     - More traffic on DNS
     - Records are outdated
for less time
     - Easy to change records
   - TTL is mandatory for
each DNS record

## Simple Routing Policy    
   - Maps a hostname to a single
resource
   - You can’t attach health
checks to simple routing
policy
   - If multiple values are
returned, a random one is
chosen by the client

## Weighted Routing Policy    
   - Control the % of the requests
that go to specific endpoint
   - Helpful to test 1% of traffic on
new app version for example
   - Helpful to split traffic between
two regions
– Load Balancing
   - Can be associated with
Health Checks
   - Note: The weights don’t need
to sum up to 100

## Failover Routing Policy Active - Passive 

## Latency Routing Policy    
   - Redirect to the server that
has the least latency close to
us
   - Super helpful when latency
of users is a priority
   - Latency is evaluated in terms
of user to designated AWS
Region
   - Germany users may be
directed to the US (if that’s
the lowest latency)
   - Has a failover capability if you
enable health checks

## Geo Location Routing Policy
   - Different from Latency based!
   - This is routing based on user
location
   - Here we specify: traffic from the
UK should go to this specific IP
   - Should create a “default” policy
(in case there’s no match on
location)

## Route 53 - Complex / Nested Records

## Multi Value Routing Policy
   - Use when routing traffic to multiple resources
   - Want to associate a Route 53 health checks with records
   - Up to 8 healthy records are returned for each Multi Value query
   - Multi Value is not a substitute for having an ELB


Route 53 – Good to know
   - Private DNS:
     - Can use Route 53 for internal private DNS
     - Must enable the VPC settings enableDnsHostNames and enableDnsSupport
   - DNSSEC (protect against Man In the Middle attack):
     - Amazon Route 53 supports DNSSEC for domain registration
     - Route 53 does not support DNSSEC for DNS service
     - To configure DNSSEC for a Route 53 domain, use another DNS provider or
custom DNS server on Amazon EC2 (Bind, dnsmasq, KnotDNS, PowerDNS)
   - 3rd party registrar:
     - You can buy the domain out of AWS and use Route 53 as your DNS provider
     - Update the NS records on the 3rd party registrar

## Health Checks with Route 53
   - Health Check => automated DNS failovers:
     - 1. Health checks that monitor an endpoint
(application, server, other AWS resource)
     - 2. Health checks that monitor other health checks
(calculated health checks)
     - 3. Health checks that monitor CloudWatch alarms
(full control !!) – e.g. throttles of DynamoDB, alarms on RDS, custom metrics, etc
   - Health Checks are integrated with CW metrics
   
## Route 53 Health Checks – good to know
   - Health Checks can be setup to pass / fail
based on
text in the first 5120 bytes of the response
   - Health Checks pass only with the 2xx and
3xx status response
   - Calculated health checks
     - Create separate individual health checks
     - Specify how many of the health checks need to
pass to make the parent pass
   - Health Checks can trigger CW Alarms

## Health Checks – Private Hosted Zones
   - Route 53 health checkers are outside
the VPC
   - They can’t access private endpoints (private VPC or on-premise resource)
   - Options:
     - To check a resource within a VPC, you
must assign a public IP address
     - You can configure the health checker to
check the health of an external resource
the instance relies on, for example a
database server
     - You can create a CloudWatch metric
and associate an alarm. You then create
a health check that checks the alarm
itself

## Route 53 Solution Architecture Sharing a Private Zone across VPC
   - Having a central private
“Shared Services” DNS can
ease management
   - Oher accounts may want to
access the central private DNS
records
     - 1. Connectivity between VPC
must be established (VPC
peering)
     - 2. Must programmatically (CLI)
associate the VPC with the
central hosted zone
   - One association must be
created for each new account

## Solution Architecture Comparisons    
   - EC2 on its own with Elastic IP    
   - EC2 with Route53    
   - ALB + ASG    
   - ALB + ECS on EC2    
   - ALB + ECS on Fargate    
   - ALB + Lambda    
   - API Gateway + Lambda    
   - API Gateway + AWS Service    
   - API Gateway + HTTP backend (ex: ALB)

## EC2 with Elastic IP
   - Quick failover
   - The client should not
see the change
happen
   - Helpful if the client
needs to resolve by
static Public IP
address
   - Does not scale
   - Cheap

## Stateless web app - scaling horizontally
   - “DNS-based load
balancing”
   - Ability to use multiple
instances
   - Route53 TTL implies
client may get outdated
information
   - Clients must have logic to
deal with hostname
resolution failures
   - Adding an instance may
not receive full traffic
right away due to DNS
TTL

## ALB + ASG
   - Scales well, classic architecture    
   - New instances are in service right away.    
   - Users are not sent to instances that are out-of-service
   - Time to scale is slow (EC2 instance
startup + bootstrap) – AMI can help
   - ALB is elastic but can’t handle sudden,
huge peak of demand (pre -warm)
   - Could lose a few requests if instances
are overloaded
   - CloudWatch used for scaling    
   - Cross-Zone balancing for even traffic distribution    
   - Target utilization should be between 40% and 70%

## ALB + ECS on EC2 (backed by ASG)
   - Same properties as ALB +
ASG
   - Application is run on
Docker
   - ASG + ECS allows to have
dynamic port mappings
   - Tough to orchestrate ECS
service auto-scaling + ASG
auto-scaling

## ALB + ECS on Fargate
   - Application is run on
Docker
   - Service Auto Scaling is easy
   - Time to be in-service is
quick (no need to launch an
EC2 instance in advance)
   - Still limited by the ALB in
case of sudden peaks
   - “serverless” application tier
   - “managed” load balancer

## ALB + Lambda
   - Limited to Lambda’s runtimes    - Seamless scaling thanks to
Lambda
   - Simple way to expose
Lambda functions as HTTP/S
without all the features from
API Gateway
   - Can combine with WAF
(Web Application Firewall)
   - Good for hybrid
microservices
   - Example: use ECS for some
requests, use Lambda for
others

## API Gateway + Lambda
   - Pay per request, seamless scaling,
fully serverless
   - Soft limits: 10000/s API Gateway,
1000 concurrent Lambda
   - API Gateway features:
authentication, rate limiting,
caching, etc…
   - Lambda Cold Start time may
increase latency for some
requests
   - Fully integrated with X-Ray
   
## API Gateway + AWS Service (as a proxy)
   - Lower latency, cheaper
   - Not using Lambda concurrent
capacity, no custom code
   - Expose AWS APIs securely
through API Gateway
   - SQS, SNS, Step Functions…
   - Remember API Gateway has a
payload limit of 10 MB (can be
a problem for S3 proxy)

## API Gateway + HTTP backend (ex: ALB)
   - Use API Gateway features on
top of custom HTTP backend
(authentication, rate control,
API keys, caching…)
   - Can connect to…
   - On-premise service
   - Application Load Balancer
   - 3rd party HTTP service  
   
   
 ## Tips
 - For EC2 instances, always use a Type A Record without an Alias. For ELB, Cloudfront and S3, always use a Type A Record with an Alias and finally, for RDS, always use the CNAME Record with no Alias.

- After you’ve created your VPC, you further expand your network by adding associating one to utmost 4 secondary CIDR blocks to your VPC.

- Data Pipeline is for batch jobs

- Lambda can be triggered from SQS
- In Multi-AZ RDS,  standby instance cannot be accessed for read

- HTTPS between viewers and CloudFront
    – You can use a certificate that was issued by a trusted certificate authority (CA) such as Comodo, DigiCert, Symantec or other third-party providers.
    – You can use a certificate provided by AWS Certificate Manager (ACM)
HTTPS between CloudFront and a custom origin
    – If the origin is not an ELB load balancer, such as Amazon EC2, the certificate must be issued by a trusted CA such as Comodo, DigiCert, Symantec or other third-party providers.
    – If your origin is an ELB load balancer, you can also use a certificate provided by ACM.
- Although the on-premises data center is using a tape gateway, you can still set up a solution to use a file gateway in order to properly process the videos using Amazon Rekognition. Keep in mind that the tape gateway in AWS Storage Gateway service is primarily used as an archive solution.  That’s glacier
- AWS Organizations, SCPs DO NOT affect any service-linked role. Service-linked roles enable other AWS services to integrate with AWS Organizations and can't be restricted by SCPs
- You can use the same SSL certificate from ACM in more than one AWS Region but it depends on whether you’re using Elastic Load Balancing or Amazon CloudFront. To use a certificate with Elastic Load Balancing for the same site (the same fully qualified domain name, or FQDN, or set of FQDNs) in a different Region, you must request a new certificate for each Region in which you plan to use it. To use an ACM certificate with Amazon CloudFront, you must request the certificate in the US East (N. Virginia) region.
- Amazon EC2 now allows peering relationships to be established between Virtual Private Clouds (VPCs) across different AWS regions. Inter-Region VPC Peering allows VPC resources like EC2 instances, RDS databases, and Lambda functions running in different AWS regions to communicate with each other using private IP addresses, without requiring gateways, VPN connections or separate network appliances.
- With AWS Certificate Manager, you can generate public or private SSL/TLS certificates that you can use to secure your site. Public SSL/TLS certificates provisioned through AWS Certificate Manager are free. You pay only for the AWS resources that you create to run your application. For private certificates, the ACM Private Certificate Authority (CA) is priced along two dimensions: (1) You pay a monthly fee for the operation of each private CA until you delete it and (2) you pay for the private certificates you issue each month.
- Public certificates generated from ACM can be used on Amazon CloudFront, Elastic Load Balancing, or Amazon API Gateway but not directly on EC2 instances, unlike private certificates.
- Gateway-Cached volumes can support volumes of 1,024TB in size, whereas Gateway-stored volume supports volumes of 512 TB size.
- Service Control Policies (SCP) vs IAM Policies:
https://tutorialsdojo.com/aws-cheat-sheet-service-control-policies-scp-vs-iam-policies/
- Service Control Policies (SCP) vs IAM Policies  References: https://aws.amazon.com/premiumsupport/knowledge-center/iam-policy-service-control-policy/
- SCPs are available only when you enable all features in your organization.
- network device that supports Border Gateway Protocol (BGP) and BGP MD5 authentication is needed to establish a Direct Connect link from your data center to your VPC
- Global accelerator
- Transit Gateway
- only one virtual private gateway (VGW) can be attached to a VPC at a time
- APIGateway error codes
- Redshift - Automated snapshots are enabled by default when you create a cluster. cross-region snapshot copy is not by default
- certificate can be stored in AWS Certificate Manager (ACM) or in IAM
- SNS can be used to fan out notifications to end users using mobile push, SMS, and email.


