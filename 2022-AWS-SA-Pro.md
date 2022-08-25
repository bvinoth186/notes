# 2022 AWS CSA Pro

1. To use a certificate with Elastic Load Balancing for the same site (the same fully qualified domain name, or FQDN, or set of FQDNs) in a different Region, you must request a new certificate for each Region in which you plan to use it.  

1. To use an ACM certificate with Amazon CloudFront, you must request the certificate in the US East (N. Virginia) region. ACM certificates in this region that are associated with a CloudFront distribution are distributed to all the geographic locations configured for that distribution. 

1. For EC2 instances, always use a Type A Record without an Alias. For ELB, Cloudfront and S3, always use a Type A Record with an Alias and finally, for RDS, always use the CNAME Record with no Alias 

1. Amazon CloudFront only accepts well-formed connections to prevent many common DDoS attacks like SYN floods and UDP reflection attacks from reaching your origin. 

1. Multi-account, multi-region data aggregation in AWS Config enables you to aggregate AWS Config data from multiple accounts and regions into a single account. 

1. If you plan to use Systems Manager to manage on-premises servers and virtual machines (VMs) in what is called a hybrid environment, you must create an IAM Service role for those resources to communicate with the Systems Manager service 

1. .SCPs DO NOT affect any service-linked role. Service-linked roles enable other AWS services to integrate with AWS Organizations and can't be restricted by SCPs. 

1. By using stored volumes, you can store your primary data locally, while asynchronously back up that data to AWS. Stored volumes provide your on-premises applications with low-latency access to the entire datasets.  

1.   Cached volume gateway provides you low-latency access to your frequently accessed data but not to the entire data. 

1. For some AWS services, you can grant cross-account access to your resources. To do this, you attach a policy directly to the resource that you want to share, instead of using a role as a proxy. The resource that you want to share must support resource-based policies. Unlike a user-based policy, a resource-based policy specifies who (in the form of a list of AWS account ID numbers) can access that resource. 

1. Cross-account access with a resource-based policy has some advantages over a role. With a resource that is accessed through a resource-based policy, the user still works in the trusted account and does not have to give up his or her user permissions in place of the role permissions. In other words, the user continues to have access to resources in the trusted account at the same time as he or she has access to the resource in the trusting account. This is useful for tasks such as copying information to or from the shared resource in the other account. 

1. When an invited account joins your organization, you do not automatically have full administrator control over the account, unlike created accounts. If you want the master account to have full administrative control over an invited member account, you must create the  OrganizationAccountAccessRole IAM role in the member account and grant permission to the master account to assume the role. 

1. The master account of an organization can turn off Reserved Instance (RI) sharing for member accounts in that organization. This means that Reserved Instances are not shared between that member account and other member accounts. You can change this preference multiple times. Each estimated bill is computed using the last set of preferences. However, take note that turning off Reserved Instance sharing can result in a higher monthly bill. 

1. By default, the member account doesn’t have the capability to turn off RI sharing on their account. 

1. Even though “All Features” is enabled by default, this will be overridden if you enable only the “Consolidated Billing” feature. This means that you cannot use the SCP to your member AWS accounts anymore. You need to enable “All features” on the AWS Organization to be able to create and apply SCP for each subsidiary. 

1. To avoid interruption to your Spot instances, you can actually set up a diversified spot fleet allocation strategy in which you are using a range of different EC2 instance types such as c3.2xlarge, m3.xlarge, r3.xlarge et cetera instead of just one type. This will effectively increase the chances of providing a more stable compute capacity to your application. Therefore, in the event that there is a Spot interruption due to the high demand for a specific instance type, say c3.2xlarge, your application could still scale using another instance type such as m3.xlarge or r3.xlarge. 

1.  As an Amazon EC2 Reserved Instances example, suppose that Bob and Susan each have an account in an organization. Susan has five Reserved Instances of the same type, and Bob has none. During one particular hour, Susan uses three instances and Bob uses six, for a total of nine instances on the organization’s consolidated bill. AWS bills five instances as Reserved Instances, and the remaining four instances as regular instances. 

1. Bob receives the cost benefit from Susan’s Reserved Instances only if he launches his instances in the same Availability Zone where Susan purchased her Reserved Instances. For example, if Susan specifies us-west-2a when she purchases her Reserved Instances, Bob must specify us-west-2a when he launches his instances to get the cost benefit on the organization’s consolidated bill. However, the actual locations of Availability Zones are independent from one account to another. For example, the us-west-2a Availability Zone for Bob’s account might be in a different location than the location for Susan’s account. 

1.  To maintain high availability, but reduce the costs, you can use a single AWS Direct Connect to create a dedicated private connection from your data center network to your Amazon VPC, and then combine this connection with an AWS managed VPN connection to create an IPsec-encrypted connection as a backup connection. If the Direct Connect connection fails, you still have a managed VPN to connect to your Amazon VPC, albeit with a slower connection. This will suffice until your Direct Connect connection is restored. 

1. Although CloudFront supports content uploads via POST, PUT, other HTTP Methods, there is a limited connection timeout to the origin (60 seconds). If uploads will take several minutes, the connection might get terminated. If you want to optimize performance when uploading large files to Amazon S3, it is recommended to use Amazon S3 Transfer Acceleration which can provide fast and secure transfers over long distances. Transfer Acceleration uses Amazon CloudFront’s globally distributed edge locations. 

1.  Redis can provide a much more durable and powerful cache layer to the prototype distributed system, however, you should take note of one keyword in the requirement: multithreaded. In terms of commands execution, Redis is mostly a single-threaded server. It is not designed to benefit from multiple CPU cores unlike Memcached, however, you can launch several Redis instances to scale out on several cores if needed. Memcached is a more suitable choice since the scenario specifies that the system will run large nodes with multiple cores or threads and in addition, the prototype only needs simple data structures which Memcached can adequately provide. 

1. The signed cookies feature is primarily used if you want to provide access to multiple restricted files, for example, all of the files for a video in HLS format or all of the files in the subscribers’ area of website. In addition, this solution is not complete since the users can bypass the restrictions by simply using the direct S3 URLs.
CloudFront 

1.  You can configure a single CloudFront web distribution to serve different types of requests from multiple origins. For example, if you are building a website that serves static content from an Amazon Simple Storage Service (Amazon S3) bucket and dynamic content from a load balancer, you can serve both types of content from a CloudFront web distribution. 

1. RTMP is used for video-on-demand and live streaming video content from CloudFront. The default CloudFront Web distribution already allows serving both dynamic and static web content. 

1.  You can use CloudFront with the on-premises website as the origin. 

1.  When you enable Amazon Redshift to automatically copy snapshots to another region, you specify the destination region where you want snapshots to be copied.  For every 8 hours or 5 GB  But cross region snapshots needs to be configured.. It's not by default. 

1.  You can use an AWS Direct Connect gateway to connect your AWS Direct Connect connection over a private virtual interface to one or more VPCs in your account that are located in the same or different regions. You associate a Direct Connect gateway with the virtual private gateway for the VPC, and then create a private virtual interface for your AWS Direct Connect connection to the Direct Connect gateway. You can attach multiple private virtual interfaces to your Direct Connect gateway. A Direct Connect gateway is a globally available resource. You can create the Direct Connect gateway in any public region and access it from all other public regions. 

1. When you use AWS SCT and an AWS Snowball Edge device, you migrate your data in two stages. First, you use the AWS SCT to process the data locally and then move that data to the AWS Snowball Edge device. You then send the device to AWS using the AWS Snowball Edge process, and then AWS automatically loads the data into an Amazon S3 bucket. Next, when the data is available on Amazon S3, you use AWS SCT to migrate the data to Amazon Redshift. Data extraction agents can work in the background while AWS SCT is closed. You manage your extraction agents by using AWS SCT. The extraction agents act as listeners. When they receive instructions from AWS SCT, they extract data from your data warehouse. 

1. AWS SMS incrementally replicates your server VMs as cloud-hosted Amazon Machine Images (AMIs) ready for deployment on Amazon EC2. Working with AMIs, you can easily test and update your cloud-based images before deploying them in production. 

1. AWS VM Import/Export does not support synching incremental changes from the on-premises environment to AWS. You will need to import the VM again as a whole after you make changes to the on-premises environment. SMS is 

1.  AWS SMS supports up to 16TB volumes so you can use it to migrate the data volumes as well. 

1.  Amazon Connect uses the following services for ML/AI: 

1. Amazon Lex—Lets you create a chatbot to use as Interactive Voice Response (IVR). 

1. Amazon Polly—Provides text-to-speech in all contact flows. 

1. Amazon Transcribe—Grabs conversation recordings from Amazon S3, and transcribes them to text so you can review them. 

1. Amazon Comprehend—Takes the transcription of recordings, and applies speech analytics machine learning to the call to identify sentiment, keywords, adherence to company policies, and more. 

1.  due to encryption overhead when copying files to the Snowball Edge device. Open multiple sessions to the Snowball Edge device and initiate parallel copy jobs to improve the overall copying throughput 

1.  Amazon Cognito identity pools provide temporary AWS credentials for users who are guests (unauthenticated) and for users who have been authenticated and have received a token. 

1.   It is better to use S3 instead of EFS if you are only storing media files and not documents which requires file locking or POSIX-compliant storage. 

1.  AWS strongly recommends that you don’t attach SCPs to the root of your organization without thoroughly testing the impact that the policy has on accounts. Instead, create an OU that you can move your accounts into one at a time, or at least in small numbers, to ensure that you don’t inadvertently lock users out of key services. 

1.  You can use AWS CodeDeploy to deploy an application to Amazon EC2 instances running within an Amazon Virtual Private Cloud (VPC).
However, the AWS CodeDeploy agent installed on the Amazon EC2 instances must be able to access the public AWS CodeDeploy and Amazon S3 service endpoints.For vmware vsphere, aws connecter for vcenter can be used to export vm from wmware and import to ec2. For Microsoft systems center, aws system manager for Microsoft SCVMM can be used 

1. 400 to 600 

1. Lambda@Edge 1. You can author Node.js or Python functions in one Region, US-East-1 (N. Virginia), and then execute them in AWS locations globally that are closer to the viewer, without provisioning or managing servers. 

1.  Amazon Elasticsearch Service is now called Amazon OpenSearch Service. 

1.  Step Functions do not directly support Mechanical Turk. We need to use Amazon SWF for this scenario. 

1.  With AWS CodePipeline, you can create change sets and automatically deploy CloudFormation template updates safely. You can have a CodeBuild stage for building artifacts and testing your new infra. 

1. You can configure Amazon CloudFront to require viewers to interact with your content over an HTTPS connection using the HTTP to HTTPS Redirect feature. If you configure CloudFront to serve HTTPS requests using SNI, CloudFront associates your alternate domain name with an IP address for each edge location. The IP address to your domain name is determined during the SSL/TLS handshake negotiation and isn’t dedicated to your distribution. 

1. DDos & waf 1. cloudFront and ALB 

1. set up an origin failover by creating an origin group with two origins with one as the primary origin and the other as the second origin which CloudFront automatically switches to when the primary origin fails. This will alleviate the occasional HTTP 504 errors that users are experiencing. 

1. DynamoDB stores structured data in tables, indexed by primary key, and allows low-latency read and write access to items ranging from 1 byte up to 400 KB. 

1. DocumentDb does not offer a native cross-region replication or multi-region operation 

1.  there is a delay of up to 15 minutes in Cross-Region Replication. S3 

1. we can trasfer the data from Dynamod DB to Redshift  

1. You cannot route traffic to a NAT gateway through a VPC peering connection, a Site-to-Site VPN connection, or AWS Direct Connect. A
NAT gateway cannot be used by resources on the other side of these connections. 

1. FIFO (first-in-first-out) queues preserve the exact order in which messages are sent and received. If you use a FIFO queue, you don't have to place sequencing information in your messages 

1.  Standard queues provide at-least-once delivery, which means that each message is delivered at least once. 

1. FIFO queues provide exactly-once processing, which means that each message is delivered once and remains available until a consumer processes it and deletes it. Duplicates are not introduced into the queue. 

1.  NLB 1. If you create an internet-facing load balancer, you can select an Elastic IP address for each Availability Zone. This provides your load balancer witt
static IP addresses. 

1.  CloudWatch Agent needs to be install to work the AWS Compute Optimizer correctly. 

1. IAM allows to set policy based on time and date.  aws:CurrentTime and aws:SourceIp 

1. Cloudfront : Create separate cache behaviors for static and dynamic content, and configure CloudFront to forward cookies to your origin only for dynamic content.  Host header is required for both cache behaviors not to break the SSL connection with the ALB. 

1. To move an instance to a placement group using the AWS CLI 

1. Stop the instance using the stop-instances command.Use the modify-instance-placement command and specify the name of the placement group to which to move the instance. ...Start the instance using the start-instances command.  No need to terminate the instances 

1.  WAF 1. If you want to allow or block web requests based on the country that the requests originate from, create one or more geo match conditions. A
geo match condition lists countries that your requests originate from. Later in the process, when you create a web ACL, you specify whether to
allow or block requests from those countries. 

1. Update (12/1/2021): Amazon Kinesis Data Streams On-Demand mode is now the recommended way to natively auto scale your Amazon Kinesis Data Streams. 

1. A stack set lets you create stacks in AWS accounts across regions by using a single AWS CloudFormation template. 

1. DC 1. You can migrate a virtual interface to a new connection within the same Region, but you can't migrate it from one Region to another. 

1.  AWS designed gp3 to provide predictable 3,000 IOPS baseline performance and 125 MiB/s, regardless of volume size. With gp3 volumes, you car
provision IOPS and throughput independently, without increasing storage size, at costs up to 20% lower per GB compared to gp2 volumes. This
means you can provision smaller volumes while maintaining high performance, at a cheaper cost. 

1. The Modify Volume window displays the volume ID and the volume’s current configuration, including type, size, IOPS, and throughput 

1. EBS size can be changed 

1.  If the IAM user is trying to perform some action on the object belonging to another AWS user's bucket, $3 will verify whether the owner of the
IAM user has given sufficient permission to him. It also verifies the policy for the bucket as well as the policy defined by the object owner. 

1. With service managed permissions, With automatic deployment enabled, StackSets automatically deploys to accounts that are added to the target organization or organizational units (OUs) in the future. With retain stacks enabled, when an account is removed from a target OU, stack resources in the account are retained. You can adjust the automatic deployment settings you specified when you created your stack set at any time. 

1. An instance can have multiple ENIs attached to it, and you can deploy those ENIs into different subnets for more granular security
configurations, private and public subnet 

1. Snowball edge 1. 100 TB. 84 usable 
Snowmobile 1. 100 PB 84 usable  

1.  To migrate large datasets of 10PB or more in a single location, you should use Snowmobile. For datasets less than 10PB or distributed in multiple
locations, you should use Snowball. In addition, you should evaluate the amount of available bandwidth in your network backbone. If you have a
high speed backbone with hundreds of Gb/s of spare throughput, then you can use Snowmobile to migrate the large datasets all at once. If you
have limited bandwidth on your backbone, you should consider using multiple Snowballs to migrate the data incrementally. 

1.  You can use AWS Budgets to track your service costs and usage within AWS Service Catalog. You can associate budgets with AWS Service Catalog products and portfolios. AWS Budgets gives you the ability to set custom budgets that alert you when your costs or usage exceed (or are forecasted to exceed) your budgeted amount. If a budget is associated to a product, you can view information about the budget on the Products and Product
details page. If a budget is associated to a portfolio, you can view information about the budget on the Portfolios and Portfolio details page. When you click on a product or portfolio, you are taken to a detail page. These Portfolio detail and Product detail pages have a section with detailed information about the associated budget. You can see the budgeted
amount, current spend, and forecasted spend. You also have the option to view budget details and edit the budget. 

1. If you'd like to migrate your existing files to Amazon FSx for Windows File Server file systems, we recommend the use of AWS DataSyne, an online
data transfer service designed to simplify, automate, and accelerate copying large amounts of data to and from AWS storage services. 

1. Cloud HSM 1. Managing the keys by CloudHSM client, not BY IAM 

1.  When EC2 instances reach third-party API through internet, their privates IP addresses will be masked by NAT Gateway public  IP address. 

1.  AWS DataSync is an online data transfer service that simplifies, automates, and accelerates copying large amounts of data between on-premises storage systems and AWS Storage services, as well as between AWS Storage services. DataSync can copy data between Network File System (NFS) shares, Server Message Block (SMB) shares, Hadoop Distributed File Systems (HDFS), self-managed object storage, Google Cloud Storage, Azure Files, AWS Snowcone, Amazon Simple Storage Service (Amazon S3), Amazon Elastic File System (Amazon EFS) file systems, Amazon FSx for Windows File Server file systems, Amazon FSx for Lustre file systems, and Amazon FSx for OpenZFS file systems 

1.  Jan 2022 1. EFS supports cross region replication . 

1. S3 Glacier Instant Retrieval 1. immediate 

1. Glacier Flexible Retrieval (Formerly S3 Glacier) 1. 5 1. 12 hours  

1.  S3 Glacier Deep Archive 12 to 48 hours 

1.  Ec2 recovery 1. A recovered instance is identical to the original instance, including the instance ID, private IP addreSs 

1.  Restricting physical and logical administrative access to U.S. persons only There will be a separate AWS GovCloud (US) credentials, such as
access key and secret access key than the standard AWS account. The AWS GovCloud (US) Region authentication is completely isolated from Amazon.com If the organization is planning to host on EC2 in AWS
GovCloud then it will be billed to standard AWS account of organization since AWS GovCloud billing is linked with the standard AWS account
and is not be billed separately. 

1. Aws waf rule actions 1. count, allow, block and captcha 

1.  HTTP and HTTPS health checks — Route 53 must be able to establish a TCP connection with the endpoint within four seconds. In addition, the
endpoint must respond with an HTTP status code of 2xx or 3xx within two seconds after connecting. After a Route 53 health checker receives the HTTP status code, it must receive the response body from the endpoint within the next two seconds. Route 53 searches the response body for a
string that you specify. The string must appear entirely in the first 5,120 bytes of the response body or the endpoint fails the health check 

1. Direct Connect gateway is a globally available resource. You can create the Direct Connect gateway in any Region and access it from all other Regions. 

1. If cool down in 7 mins and 2 is min capacity, If an Auto Sealing group is launching more than one instance, the cool down period for each instance
starts after that instance is launched. The group remains locked until the last instance that was
launched has completed its cool down period. In this case the cool down period for the first instance
starts after 3 minutes and finishes at the 10th minute (3+7 cool down), while for the second instance it starts at the 4th minute and finishes at the 11th minute (4+7 cool down). Thus, the Auto Scaling group will receive another request only after 11 minutes. 

1. UDP, global usage 1. AWS Global Accelerator. 

1. Aws org got 2 features 1. All features 2. Consolidated billing.

1. For aws sso 1. first set up the AWS Organizations service and have All features set to enabled

1. RPO RTO -> mode
1. 24 24hr -> backup
1. 12 4hr -> pilot light
1. 1.4 15min -> warm standup
1. 15min 5min -> active-active

1. Kinesis Streams: By default, of a stream are accessible for up to 24 hours from the time they are added to the stream. You can raise this limit to up to 7 days by enabling
extended data retention.

1. Direct Connect location provides access to Amazon Web Services in the region it is associated with, as well as access to other US regions. To use
AWS Direct Connect, your network is collocated with an existing AWS Direct Connect location.”

1. IAM 1. Evaluation Logic for Conditions with Multiple Keys or Values If your policy has multiple condition operators or multiple keys attached to a single condition operator, the conditions are evaluated using a logical AND. If a single condition operator includes multiple values for one key, that condition operator is evaluated using a logical OR. All conditions musi
resolve to true to trigger the desired Allow or Deny effect.

1. s3 access log don't support cross account log delivery

1. RAID 0 offers striping, which translates to better performance, but no-fault tolerance or data redundancy. RAID 1, on the other hand, offers mirroring, so the same data is available in two disks. RAID 1 is slightly slower than RAID 0 because there are two writes, but the read operations are equally fast.

1. Amazon DynamoDB allows atomic increment and decrement operations on scalar values.

1. DynamoDB supports atomic counters, where you use the update method to increment or decrement the value of an existing attribute without
interfering with other write requests. (All write requests are applied in the order in which they are received.)

1. With Global Accelerator, you are provided two global static public IPs that act as a fixed entry point to your application, improving availability. On the back end, add or remove your AWS application endpoints, such as Application Load Balancers, Network Load Balancers, EC2 Instances, and Elastic IPs without making user-facing changes. Global Accelerator automatically re-routes your traffic to your nearest healthy available endpoint to mitigate endpoint failure.

1. An global accelerator supports multiple endpoints in different regions 

1. Global Accelerator does not support client IP address preservation for Network Load Balancer and Elastic IP address endpoints.

1. CloudFront does not support UDP. Instead, Global Accelerator can be used for UDP

1. AWS WAF can be deployed on Amazon CloudFront, the Application Load Balancer (ALB), Amazon API Gateway, and AWS AppSync. As part of Amazon CloudFront it can be part of your Content Distribution Network (CDN) protecting your resources and content at the Edge locations. As part of the Application Load Balancer it can protect your origin web servers running behind the ALBs. As part of Amazon API Gateway, it can help secure and protect your REST APIs. As part of AWS AppSync, it can help secure and protect your GraphQL APIs.

1. NLB not supported by WAF

1. IPS/IDS 1. EC2 does not allow promiscuous mode,  Implement IDS/IPS agents on each Instance running in VPC Or Implement a reverse proxy layer in front of web servers and configure IDS/IPS agents on each reverse proxy server.

1. An internet gateway and NAT gateway is not required to establish an AWS Site-to-Site VPN connection

1. An AWS VPN over a Direct Connect connection to your VPC is likely faster and more secure than a VPN over the internet. An AWS VPN connection over a Direct Connect connection provides consistent levels of throughput and encryption algorithms that protect your data.  Public VIF required and not the private VIF

1. Private VIFs do not provide encryption.. Public VIFs can via IPSEC. you also cannot establish a VPN connection without a Public VIF.

1. If ENI interfaces created using aws-cli-> create-network-interface without option -> DeleteOnTermination, It will persist. Also, Before you can delete a VPC, you must terminate any instances that are running in the VPC. If you delete a VPC using the VPC console, it
also deletes resources that are associated with the VPC, such as subnets, security groups, network ACLs, DHCP options sets, route tables, and Internet gateways.

1. You can stream the file from Amazon S3 to the client through your server without downloading the file to your server, by opening a stream to the Amazon S3 file then read from it and write on the client stream (buffer by buffer).

1. To detect the PutObjectAcl call and modify the ACL on the object, I: Turn on object-level logging in CloudTrail for the buckets I want to monitor.
Create an IAM execution role to be used when the Lambda function is being executed so that Lambda can make API calls to S3 on my behalf.
Create a Lambda function that receives the PutObjectAcl API call event, checks whether the call is for a monitored bucket, and, if so, ensures the object is private. Create a CloudWatch Events rule that matches the PutObjectAcl API call event and invokes the Lambda function created in the previous step.

1. SMS is meant for migration of Virtual machines to AWS whereas Cloud Endure can be used for Physical, Virtual or Cloud Server. 

1. AWS Application Migration Service (MGN) is a highly automated lift-and-shift (rehost) solution that simplifies, expedites, and reduces the cost of
migrating applications to AWS. It enables companies to lift-and-shift a large number of physical, virtual, or cloud servers

1. AWS Application Migration Service (MGN) is the primary migration service recommended for lift-and-shift migrations to the AWS Cloud.
Customers who currently use CloudEndure Migration or AWS Server Migration Service (AWS SMS) are encouraged to switch to MGN for future
migrations.

1. Q: Do | need to use Amazon Cognito to use the Amazon Mobile Analytics service? No. You can initialize Amazon Mobile Analytics using AWS IAM accounts. However, we recommend using Amazon Cognito for security best practices. Amazon Mobile Analytics is now Amazon Pinpoint. ;-)

1. S3 Replication Time Control (S3 RTC) helps you meet compliance or business requirements for data replication and provides visibility into Amazon S3 replication times. S3 RTC replicates most objects that you upload to Amazon S3 in seconds, and 99.99 percent of those objects within 15 minutes.

1. DataSync task can copies objects from one S3 bucket to another without deploying an agent on EC2.  cross-Region and cross-account use cases are supported.

1. 600 to 700 

1. When a Fargate task is launched, its elastic network interface requires a route to the internet to pull container images. If you receive an error similar to the following when launching a task, it is because a route to the internet does not exist:   CannotPullContainerError: API error (500): Get https://11122223333.dkr.ecr.us-east-1.amazonaws.com/v2/:net/http: request canceled while waiting for connection” To resolve this issue, you can: o For tasks in public subnets, specify ENABLED for Auto-assign public IP when launching the task. o For tasks in private subnets, specify DISABLED for Auto-assign public IP when launching the task, and configure a NAT gateway in your VPC to route requests to the internet.

1. When you set up custom NFS client settings, it takes up to three seconds for an Amazon EC2 instance to see a write operation being performed on a file system from another Amazon EC2 instance. To solve this issue, you must unmount and remount your file system with the noac option to disable attribute caching if the

1. NAT instance 1. disable source/destination check, public subnet, public ip or elastic ip

1. An AWS Direct Connect location provides access to Amazon Web Services in the region it is associated with, as well as access to other US regions. For example, you can provision a single connection to any AWS Direct Connect location in the US and use it to access public AWS services in all US Regions and AWS GovCloud (US).

1. AWS GovCloud (US) can access public services in any other public Region (excluding China (Beijing and Ningxia)). In addition, AWS Direct Connect connections in public Regions or AWS GovCloud (US) can be configured to access a VPC in your account in any other public Region (excluding China (Beijing and Ningxia). You can therefore use a single AWS Direct Connect connection to build multi-Region services. All networking traffic remains on the AWS global network backbone, regardless of whether you access public AWS services or a VPC in another Region.

1. The CloudWatch embedded metric format is a JSON specification used to instruct CloudWatch Logs to automatically extract metric values embedded in structured log events. You can use CloudWatch to graph and create alarms on the extracted metric values.

1. Amazon WorkDoes is a fully managed, secure content creation, storage, and collaboration service. With Amazon WorkDoes, you can easily create edit, and share content, and because it's stored centrally on AWS, access it from anywhere on any device. Amazon WorkDocs makes it easy to collaborate with others, and lets you easily share content, provide rich feedback, and collaboratively edit documents. You can use Amazon WorkDo¢s to retire legacy file share infrastructure by moving file shares to the cloud. Amazon WorkDo¢es lets you integrate with your existing systems, and offers a rich API so that you can develop your own content-rich applications  

1. When the user registers a multi-homed instance (an instance that has an Elastic Network Interface (ENI) attached) with a load balancer, the load balancer will route the traffic to the IP address of the primary network interface (eth0). 

1. AWS CloudFormation provides a WaitCondition resource that acts as a barrier and blocks the creation of other resources until a completion signal is received from an external source, such as a user application or management system.

1. DependsOn doesn’t wait for success or failure signals from AWS resources before moving forward. While, on the other hand, WaitCondition waits for success signals from your AWS resources and resumes the execution of the CloudFormation template. However, WaitCondition and DependsOn can work together when there is a resource creation dependency requirement. 

1. ARN format 1. arn:aws:service:region:account:resource

1. For iam resources region is blanks so 1. arn:aws:iam::12345678901 2:instance-profile/Webserver

1. To centrally manage users in multi account architecture, Create a new AWS account to hold user and service accounts, such as an identity account. Create users and groups in the identity account. Create roles with appropriate permissions in the production and testing accounts. Add the identity account to the trust policies for the roles.

1. The maximum number of BGP advertised routes allowed per route table is 100.

1. Amazon Cognito identity pools support both authenticated and unauthenticated identities. Authenticated identities belong to users who are authenticated by a public login provider or your own backend authentication process. Unauthenticated identities typically belong to guest users.

1. DMS supports Elasticsearch as a target, not a source

1. Index snapshots are a popular way to migrate from a self-managed Elasticsearch cluster to Amazon Elasticsearch Service.
