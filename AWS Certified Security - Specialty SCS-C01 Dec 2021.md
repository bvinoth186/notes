# AWS Certified Security - Specialty SCS-C01 Dec 2021 - Notes

  - Amazon Inspector assessment 
    - Network reachability  
    - CVE common vulnerability and exposure 
    - Center of Internet security benchmarks CIS
    - security best practice 

  - Once vpc flow logs are created it can't be edited. Delete and recreate for any modifications 

  - AWS KMS also gives you the ability to add an additional layer of authentication for your KMS API calls utilizing encryption context. The encryption context is a key-value pair of additional data that you want associated with AWS KMS-protected information. This is then incorporated into the additional authenticated data (AAD) of the authenticated encryption in AWS KMS-encrypted ciphertexts.

  - Encryption Context - Iam policies for CMk, AAD, audit trail and authorisation context 


  - WAF supports rate based rule to save ddos attacks 

  - Cognito identity pools also support custom developer identity pools other then fb google amazon and saml(user poo)

  - you can use Amazon Cognito to integrate the on-premises Active Directory, you have to use a User Pool instead of an Identity Pool. Remember that user pools are for authentication (identity verification) while Identity pools are for authorization (access control). You can only use identity pools to create unique identities for users and give them access to other AWS services.

  - Amazon Cognito User Pool supports “groups” which enables you to create and manage groups, add users to groups, and remove users from groups.  Iam role cam be assigned to that group 

  - Identity pools are primarily used for authorization (access control) only to specific AWS resources. You can use identity pools to create unique identities for users and give them access to other AWS services.



  - By default, an SCP named FullAWSAccess is attached to every root, OU, and account. This default SCP allows all actions and all services

  - As soon as you update default scp, services needs to explicitly enabled

  - AWS managed AD support 3 trust types
    - One-way:incoming – Users in the specified realm will not be able to access any resources in this domain.
    - One-way:outgoing – Users in this domain will not be able to access any resources in the specified realm.
    - Two-way (Bi-directional) – Users in this domain and users in the specified realm will be able to access resources in either domain or realm.

  - The direction of access is the opposite of the direction of trust

  - Both sides of the trust relationship must be created before authentication traffic can begin flowing through the trust. For an instance, if you create a one-way incoming trust in Domain A, then a one-way outgoing trust should also be created in Domain B.

  - can customize  monitoring scope by configuring GuardDuty to also use your own trusted IP lists and threat lists.

  - GuardDuty does not generate VPC Flow Log or CloudTrail findings for IP addresses on trusted IP lists. At any given time, you can have only one uploaded trusted IP list per AWS account per Region.

  - Threat lists consist of known malicious IP addresses. GuardDuty generates findings based on threat lists. At any given time, you can have up to six uploaded threat lists per AWS account per Region.

  - CloudWatch doesn’t have the capability to directly monitor and detect IAM access keys. Use IAM apis


  - can configure your Cloudtrail to send events to CloudWatch Logs. Create metric filter for api error codes and alarm to send Notification

  - Config rules
    - iam-root-access-key-check – Checks whether the root user access key is available
    - cloudtrail-enabled – Checks whether CloudTrail is enabled in your AWS account

  - Ssh key pair - Amazon EC2 stores the public key only, and you store the private key.  Because Amazon EC2 doesn’t keep a copy of your private key, there is no way to recover a private key if you lose it. 

  - At boot time, the public key content is placed on your Linux instance in an entry within ~/.ssh/authorized_keys

  - When you delete a key pair, you are only deleting the Amazon EC2 copy of the public key. Deleting a key pair doesn’t affect the private key on your computer or the public key on any instances that already launched using that key pair.

  - CloudTrail can directly send logs s3 bucket in another central account.  Enable cloudtrail permissions in bucket policy

  - Requester pays option must be disabled to cloud trail send logs to S3

  - By default, trails do not log data events.

  - Aws Comfig --> CW event --> Lambda

  - To assess access key history --> CloudTrail and CloudWatch. Iam credentials report will not provide the historical data

  - CloudTrail event history provides a viewable, searchable, and downloadable record of the past 90 days of CloudTrail events.

  - Cloud Trail stores all the event history for 90 days by default. No additional setup needed. To send trail logs to CW and S3 additional configuration needed 

  - To inspect IP packet data, use proxy software in outbound traffic or host based agent. Vpc flow logs and access logs will not help here

  - With AWS Firewall Manager, you set up your AWS WAF firewall rules, Shield Advanced protections, and Amazon VPC security groups just once. The service automatically applies the rules and protections across your accounts and resources, even as you add new resources

  - Classic Load Balancer does not support Server Name Indication (SNI). You have to use an Application Load Balancer instead or a CloudFront web distribution to use SNI.

  - Waf rate based control
    - 1. Ip range 
	- 2. User Agent


  - Either IAm policy or bucket policy or both works as long as no explicit deny. iam policy allows then bucket policy not required and vice versa


  - CloudFront and Elastic Load Balancing are two AWS services that support Perfect Forward Secrecy. SSL/TLS

  - Guard Duty master account 
    - Can archive findings in own and member accounts 
    - Can upload further trusted ip list and threat list in master account which will be inheritted to all member 
    - Can generate findings only in n own account 
    - CanNOT generare findings in member account 

  - Guard Duty Member account 
    - Can generate findings only in own account 
    - Can't archive 
    - Can't upload trusted ip lists and threat lists

  - When Amazon Cognito receives a SAML assertion, it needs to be able to map SAML attributes to 
user pool attributes. When configuring Amazon Cognito to receive SAML assertions from an identity provider, you 
need ensure that the identity provider is configured to have Amazon Cognito as a relying party. Amazon API 
Gateway will need to be able to understand the authorization being passed from Amazon Cognito, which is a 
configuration step.

  - Locking a vault takes two steps:

    - 1. Initiate the lock by attaching a vault lock policy to your vault, which sets the lock to an in-progress state and returns a lock ID. While in the in-progress state, you have 24 hours to validate your vault lock policy before the lock ID expires.

    - 2. Use the lock ID to complete the lock process. If the vault lock policy doesn’t work as expected, you can abort the lock and restart from the beginning. For information on how to use the S3 Glacier API to lock a vault, see Locking a Vault by Using the Amazon S3 Glacier API.

  - AWS KMS supports two resource-based access control mechanisms: key policies and grants 

  - grants used to allow access, but not deny it.

  - grants can be very specific, and are easy to create and revoke, they are often used to provide temporary permissions or more granular permissions.

  - To enable more granular permissions management, use grants instead of key policy

  - automatic key rotation not eligible in asymmetric CMKs, CMKs in custom key stores, and CMKs with imported key material.

  - You choose the key material origin when you create the CMK, and you cannot change 

  - When you import key material into a CMK, the CMK is permanently associated with that key material. You can reimport the same key material, but you cannot import different key material into that CMK

  - The standard asymmetric encryption algorithms that AWS KMS uses do not support an encryption context.  Applicable only for symmetric key

  - Envelope encryption is just the practice of encrypting plaintext data with a data key and then encrypting the data key under another key. Envelop encryption does not add additional encryption to your data. It merely protects (encrypts) the data key that you used to encrypt your data.

  - Assuming you still have a running EC2 with access to the Encrypted volume and it has an unencrypted volume attached, you migrate the data of that encrypted volume to the unencrypted volume. You can freely transfer data between them. EC2 carries out the encryption and decryption operations transparently.

  - This way, even if you lose the CMK used to encrypt the original EBS volume, you can still recover the data and copy to another volume without encryption

  - Envelope encryption is the practice of encrypting plaintext data with a data key, and then encrypting the data key under another key.

  - You can even encrypt the data encryption key under another encryption key, and encrypt that encryption key under another encryption key. But, eventually, one key must remain in plaintext so you can decrypt the keys and your data. This top-level plaintext key encryption key is known as the master key.

  - The AWS CLI (aws s3 commands), AWS SDKs, and many third-party programs automatically perform a multipart upload when the file is large. To perform a multipart upload with encryption using an AWS KMS key, the requester must have permission to the kms:Decrypt action on the key. This permission is required because Amazon S3 must decrypt and read data from the encrypted file parts before it completes the multipart upload.

  - Enabling Server-Side Encryption with Amazon S3-Managed Keys (SSE-S3) will not provide audit trail on when the encryption decryption action performed.  Use SSE KMs

  - RDS - If the master and read replica are in different AWS Regions, Set up a new CMK in the other region using AWS KMS. Create the encrypted read replica in another AWS Region by specifying the key identifier of the newly created CMK in the other Region.

  - Lambda@edge (cloud front integrated) and waf can security headers.   WAF can't replace security headers it, it can only insert it 


  - Setting up ACM private CA require root ca. this can be used to sign certificate signing request CSR for the new subordinate (CA) , then import into acm private CA, it's possible that container in your own platform can generate key pairs over OpenSSL. They can use they key pairs to make their own CSR and receive their own certificate

  - Aws support api provide operations to access trusted advisor checks

  - When you switch roles in the AWS Management Console, the console always uses your original credentials to authorize the switch. This applies whether you sign in as an IAM user, as a SAML-federated role, or as a web-identity federated role. For example, if you switch to RoleA, IAM uses your original user or federated role credentials to determine whether you are allowed to assume RoleA. If you then switch to RoleB while you are using RoleA, AWS still uses your original user or federated role credentials to authorize the switch, not the credentials for RoleA.

  - You cannot switch roles if you sign in as the AWS account root user. You can switch roles when you sign in as an IAM user.

  - Role chaining is possible only in cli and api. Not possible in console 

  - When you import key material into a KMS key, the KMS key is permanently associated with that key material. You can reimport the same key material, but you cannot import different key material into that KMS key.

  - When you encrypt data under a KMS key, the ciphertext cannot be decrypted with any other KMS key. This is true even when you import the same key material into a different KMS key. This is a security feature of KMS keys.

  - If key material is expired or deleted, You must reimport the same key material that was originally imported into the KMS key. You cannot import different key material into a KMS key. Also, AWS KMS cannot create key material for a KMS key that is created without key material.

  - Each time you import key material to a KMS key, you need to download and use a new wrapping key and import token for the KMS key. The wrapping procedure does not affect the content of the key material, so you can use different wrapping keys (and different import tokens) to import the same key material.

  - Wrapping key or public key is used to encrypt your key material.  While importing key material upload the encrypted key material along with import token 

  - Public key and import token that gets downloaded expire in 24 hours. So import your key material within 24 hours or download new key and token 

  - CMk uses imported key material origin wil be external 

  - Network Load Balancer integrates seamlessly with ECS and other AWS services, providing end-to-end TLS communication across services without offloading or terminating the certificates. This gives you the ability to use dynamic ports in ECS containers. It can also handle tens of millions of requests per second while maintaining high throughput at ultra-low latency for applications that require the TCP protocol.

  - When you use SSE-KMS to protect your data without an S3 Bucket Key, Amazon S3 uses an individual AWS KMS data key for every object.


  - To use a certificate in AWS Certificate Manager (ACM) to require HTTPS between viewers and CloudFront, make sure you request (or import) the certificate in the US East (N. Virginia) Region (us-east-1).

  - If you want to require HTTPS between CloudFront and your origin, and you’re using a load balancer in Elastic Load Balancing as your origin, you can request or import the certificate in any AWS Region.

  - Aws config cab check cloudyrail disabled and root user access key. Also can trigger lambda. 

  - trusted advisor cannot detect the creation of access keys on the root account only the presence of them in popular code repos


  - To encrypt data outside of AWS KMS:

    - 1. Use the GenerateDataKey operation to get a data key.
    - 2. Use the plaintext data key (in the Plaintext field of the response) to encrypt your data outside of AWS KMS. Then erase the plaintext data key from memory.
    - 3. **** Store the encrypted data key (in the CiphertextBlob field of the response) with the encrypted data. ****

  - To decrypt data outside of AWS KMS:

    - 1. **** Use the Decrypt operation to decrypt the encrypted data key. **** The operation returns a plaintext copy of the data key.
    - 2. Use the plaintext data key to decrypt data outside of AWS KMS, then erase the plaintext data key from memory.

  - To manually rotate kms key ( key materiel ), create new CMk and redirect existing key alias to new CMK. Manual Key rotation is different from key material expired or deleted.

  - Iam group can't be principle in iam policy. It's should be role or user

  - If you specify root user in KMS key policy, it enables any iam user / role in the account to access the key as long as they have attached with iam permissions that allows it.  This is called kms actions controlled by iam

  - Scp's are available only if all features are enabled

  - By default, when Security Hub is enabled, it deploys the CIS AWS Foundations standard in your account. CIS AWS Foundations are a set of security configuration best practices for hardening AWS accounts. In order for Security Hub to successfully run compliance checks against the rules included in the CIS AWS Foundations standard, AWS Config must be enabled in the same account where you enabled AWS Security Hub.

  - If you had previously created your own AWS Config rules before enabling AWS Security Hub, you will need to import them into AWS Security Hub

  - Inspector can do center for internet security CIS benchmarks only for EC2.  To run the compliance check in an account go for security hub with config

  - To assign an ACM certificate to a CloudFront distribution, you must request or import the certificate in the US East (N. Virginia) Region


  - The certificate must be a 2048-bit RSA certificate or smaller. Though ACM supports 1024 bit through 4096-bit RSA certificates, services such as CloudFront that are integrated with ACM support a maximum of 2048-bit RSA certificates.

  - Certificate, private key, certificate chain must be pem encoded. Not pkcs-12 encoded

  - AWS Artifact is your go-to, central resource for compliance-related information that matters to you. It provides on-demand access to AWS’ security and compliance reports and select online agreements. Reports available in AWS Artifact include our Service Organization Control (SOC) reports, Payment Card Industry (PCI) reports, and certifications from accreditation bodies across geographies and compliance verticals that validate the implementation and operating effectiveness of AWS security controls. Agreements available in AWS Artifact include the Business Associate Addendum (BAA) and the Nondisclosure Agreement (NDA).

  - You can enable logging to get detailed information about traffic that is analyzed by your web ACL. Logged information includes the time that AWS WAF received a web request from your AWS resource, detailed information about the request, and details about the rules that the request matched. You can send your logs to an Amazon CloudWatch Logs log group, an Amazon Simple Storage Service (Amazon S3) bucket, or an Amazon Kinesis Data Firehose.

  - When you import key material, you can specify an expiration date. When the key material expires, AWS KMS deletes the key material and the AWS KMS key becomes unusable. You can also delete key material on demand. Whether you wait for the key material to expire or you delete it manually, the effect is the same. AWS KMS deletes the key material, the KMS key's key state changes to pending import, and the KMS key is unusable. To use the KMS key again, you must reimport the same key material.

  - Deleting key material affects the KMS key immediately, but you can reverse the deletion of key material by reimporting the same key material into the KMS key. In contrast, deleting a KMS key is irreversible. If you schedule key deletion and the required waiting period expires, AWS KMS deletes the key material and all metadata associated with the KMS key.

  - You must disable source / destination checks if the instance runs services such as network address translation, routing or firewall

  - Both KMS and CloudHSM are FIPS compliant.

  - Using AWS Encryption SDK you can set max age and max number of messages for encryption


  - If you try to add, modify, or remove a log file prefix for an S3 bucket that receives logs from a trail, you might see the error: There is a problem with the bucket policy. A bucket policy with an incorrect prefix can prevent your trail from delivering logs to the bucket. To resolve this issue, use the Amazon S3 console to update the prefix in the bucket policy, and then use the CloudTrail console to specify the same prefix for the bucket in the trail.


  - Network ACL outbound rule must contain ephemeral ports.

  - Ip tables can easily block access to the instance metadata at the user level. The below Command blocks meta data to the users except for the root user: "iptables --append OUTPUT --proto tcp --destination 169.254.169.254 --match owner ! --uid-owner root --jump REJECT"

  - We can disable metadata, but should not, it can be used for many purpose of automation/administration. The easier and better is : restrict the access of user to metadata by blocking access to it from local machines (by firewall).

  - For encrypted ami sharing across accounts in asg, Create a customer-managed CMK in the centralized account. Allow other applicable accounts to use that key for cryptographical operations by applying proper cross-account permissions in the key policy. Create an IAM role in all applicable accounts and configure its access policy with permissions to create grants for the centrally managed CMK. Use this IAM role to create a grant for the centrally managed CMK with permissions to perform cryptographical operations and with the EC2 Auto Scaling service-linked role defined as the grantee principal.

  - Launch constraints allow an AWS Service Catalog end user to launch an AWS Service Catalog product without requiring elevated permissions to AWS resources.

  - EC2Rescue for Linux is an easy-to-use, open-source tool that can be run on an Amazon EC2 Linux instance to diagnose and troubleshoot common issues using its library of over 100 modules. A few generalized use cases for EC2Rescue for Linux include gathering syslog and package manager logs, collecting resource utilization data, and diagnosing/remediating known problematic kernel parameters and common OpenSSH issues.

  - EC2Rescue for Windows Server is an easy-to-use tool that you run on an Amazon EC2 Windows Server instance to diagnose and troubleshoot possible problems. It is valuable for collecting log files and troubleshooting issues and also proactively searching for possible areas of concern. It can even examine Amazon EBS root volumes from other instances and collect relevant logs for troubleshooting Windows Server instances using that volume.

  - The following elements aren't supported in SCPs:
    - Principal
    - NotPrincipal
    - NotResource

  - S3 object ownership is an S3 feature that enables bucket owner to automatically assume ownership of the objects that are uploaded to bucketsby other aws accounts

  - Ddos simulation test can only be performed with assistance of an APN partner.  Otherwise it will be considered as violation of aws customer agreement

  - S3 bucket key - When you configure your bucket to use an S3 Bucket Key for SSE-KMS, AWS KMS generates a bucket-level key that is used to create unique data keys for new objects that you add to the bucket. This S3 Bucket Key is used for a time-limited period within Amazon S3, reducing the need for Amazon S3 to make requests to AWS KMS to complete encryption operations. This reduces traffic from S3 to AWS KMS, allowing you to access AWS KMS-encrypted objects in S3 at a fraction of the previous cost.

  - When you configure an S3 Bucket Key, objects that are already in the bucket do not use the S3 Bucket Key. To configure an S3 Bucket Key for existing objects, you can use a COPY operation.

  - Only imported key material can be deleted immediately

  - Authenticate users using an Application Load Balancer
    - Authenticate users through an identity provider (IdP) that is OpenID Connect (OIDC) compliant.
    - Authenticate users through social IdPs, such as Amazon, Facebook, or Google, through the user pools supported by Amazon Cognito.
    - Authenticate users through corporate identities, using SAML, LDAP, or Microsoft AD, through the user pools supported by Amazon Cognito.

  - The aws:MultiFactorAuthPresent key is not present when an API or CLI command is called with long-term credentials, such as user access key pairs. Therefore we recommend that when you check for this key that you use the ...IfExists versions of the condition operators.

  - This combination of the Deny effect, Bool element, and false value denies requests that can be authenticated using MFA, but were not. This applies only to temporary credentials that support using MFA. This statement does not deny access to requests that are made using long-term credentials, or to requests that are authenticated using MFA

  - For cross region KMS, Use an AWS KMS custom key store backed by AWS CloudHSM clusters, and copy backups across Regions

  - You can not redirect to HTTP/HTTPS in Network LB as it does not have application layer. HTTP and HTTPS traffic can be routed to your environment over TCP. ALB can redirect.

  - Enabling rotation causes the secret to rotate once immediately when you save the secret. Before you enable rotation, be sure you update all of your applications using this secret credentials to retrieve the secret from Secrets Manager. The original credentials might not be usable after the initial rotation. Any applications you fail to update break as soon as the old credentials become invalid.

  - When you create an organization trail, a trail with the name that you give it will be created in every AWS account that belongs to your organization. Users with CloudTrail permissions in member accounts will be able to see this trail when they log into the AWS CloudTrail console from their AWS accounts, or when they run AWS CLI commands such as describe-trail. However, users in member accounts will not have sufficient permissions to delete the organization trail, turn logging on or off, change what types of events are logged, or otherwise alter the organization trail in any way.

  - Q: Why would I need to use a custom key store?
    - A: Since you control your AWS CloudHSM cluster, you have the option to manage the lifecycle of your CMKs independently of AWS KMS. There are four reasons why you might find a custom key store useful. Firstly, you might have keys that are explicitly required to be protected in a single tenant HSM or in an HSM over which you have direct control. Secondly, you might have keys that are required to be stored in an HSM that has been validated to FIPS 140-2 level 3 overall (the HSMs used in the standard AWS KMS key store are either validated or in the process of being validated to level 2 with level 3 in multiple categories). Thirdly, you might need the ability to immediately remove key material from AWS KMS and to prove you have done so by independent means. Finally, you might have a requirement to be able to audit all use of your keys independently of AWS KMS or AWS CloudTrail.


  - "When you create an Amazon GuardDuty filter, you choose specific filter criteria, name the filter and can enable the auto-archiving of findings that the filter matches. This allows you to further tune GuardDuty to your unique environment, without degrading the ability to identify threats. With auto-archive set, all findings are still generated by GuardDuty, so you have a complete and immutable history of all suspicious activity."

  - AWS Permitted Services for security assessments or penetration tests against their AWS infrastructure without prior approval for 8 services, listed as “Permitted Services.”
    - Amazon EC2 instances, NAT Gateways, and Elastic Load Balancers
    - Amazon RDS
    - Amazon CloudFront
    - Amazon Aurora
    - Amazon API Gateways
    - AWS Lambda and Lambda Edge functions
    - Amazon Lightsail resources
    - Amazon Elastic Beanstalk environments

  - edge-to-edge routing in a cloud, For example, you have a corporate network connected to one Amazon VPC (A). That VPC is also connected to another VPC (B). Routing from your corporate network through VPC A to reach VPC B is an example of edge to edge routing (and it's not allowed).

  - custom proprietary protocols = Classic LB + TCP listner. Alb and clb supports end to end data in transit till ec2 with out offloading

  - Even though Kinesis Data Analytics , Kinesis Data Firehose and Kinesis Data Streams enforce encryption at rest and transit. Only in Kinesis Data Analytics, we cannot disable encryption at rest or transit.

  - Cloudwatch doesn't stream logs to S3; it supports exporting them to S3 with an up to 12 hour expected delay:  for realtime stream CW logs to kinesis


  - Nacl - Ensure that you place the deny rules earlier in the table than the allow rules that open the wide range of ephemeral ports. 100 200

  - you can use Firewall manager to centrally manage security groups


  - If accounts are already members in the same organization, you don't have to send invitation. 

  - To manage multiple accounts in Amazon GuardDuty, you must choose a single AWS account to be the administrator account for GuardDuty. You can then associate other AWS accounts with the administrator account as member accounts. There are two ways to associate accounts with a GuardDuty administrator account: either through an AWS Organizations organization that both accounts are members of, or by sending an invitation through GuardDuty


  - CloudFront Use signed URLs in the following cases:
    - You want to restrict access to individual files, for example, an installation download for your application.
    - Your users are using a client (for example, a custom HTTP client) that doesn't support cookies.

  - Use signed cookies in CloudFront for the following cases:
    - You want to provide access to multiple restricted files, for example, all of the files for a video in HLS format or all of the files in the subscribers' area of website. 
    - You don't want to change your current URLs.


  - Redshift DB users have their own ARN in this format: arn:aws:redshift:region:account-id:dbuser:cluster-name/user-name


  - You cannot reference the security group of a peer VPC that's in a different Region. Instead, use the CIDR block of the peer VPC.

  - HSM is one of the service which has Tamper-evident control & its Secured by VPC ,


  - you can use Amazon Cognito to integrate the on-premises Active Directory, you have to use a User Pool instead of an Identity Pool. Remember that user pools are for authentication (identity verification) while Identity pools are for authorization (access control). You can only use identity pools to create unique identities for users and give them access to other AWS services.

  - Cognito Sync just a client library that enables cross-device syncing of application-related user data.

  - there is no direct way to stream API event data from CloudTrail to Kinesis. You should use CloudWatch Alarms instead.

  - Trusted iplist and threat list - Guard Duty

  - Inspector automatically assesses applications for exposure, vulnerabilities, and deviations from best practices.  CVE, CIS, network reachability and best practices

  - CORS not required for trail to send logs to S3

  - For existing instances, you can turn off access to your instance metadata by disabling the HTTP endpoint of the instance metadata service, regardless of which version of the instance metadata service you are using. You can reverse this change at any time by enabling the HTTP endpoint. Use the modify-instance-metadata-options CLI command and set the http-endpoint parameter to disabled.


  - The DynamoDB Encryption Client supports client-side encryption, where you encrypt your table data before you send it to DynamoDB. However, DynamoDB provides a server-side encryption at rest feature that transparently encrypts your table when it is persisted to disk and decrypts it when you access the table.

  - For web applications, you can use the Application Load Balancer to route traffic based on content and accept only well-formed web requests. Application Load Balancer blocks many common DDoS attacks, such as SYN floods or UDP reflection attacks, protecting your application from the attack. Application Load Balancer automatically scales to absorb the additional traffic when these types of attacks are detected. Scaling activities due to infrastructure layer attacks are transparent for AWS customers and do not affect your bill.

  - For TCP-based applications, you can use Network Load Balancer to route traffic to targets (for example, Amazon EC2 instances) at ultra-low latency. One key consideration with Network Load Balancer is that any traffic that reaches the load balancer on a valid listener will be routed to your targets, not absorbed. You can use Shield Advanced to configure DDoS protection for Elastic IP addresses. When an Elastic IP address is assigned per Availability Zone to the Network Load Balancer, Shield Advanced will apply the relevant DDoS protections for the Network Load Balancer traffic.

  - Aws services doesn't provide IPS / IDS protection.  Use third party solution from marketplace

  - Aws network firewall to perform deep packet inspection

  - For end to end encryption, need to terminate ssl/tls on the ec2 instance. It's possible only with NLB and CLG.  ALB operstes on http and https

  - With glacier vault lock, for data retention, once locked, the policy can no longer be changed

