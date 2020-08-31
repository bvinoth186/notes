
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

SSL/TLS - Basics
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
