# 2021 AWS DevOps Professional 

- Deployment strategies - all at once, blue green, immutable, custom  

- Elastic beanstalk deployment - All at once, Rolling, Rolling with additional batch, and Immutable 

- Code deploy - Blue green  

- Pipeline cannot call another pipeline  

- ApiGateway can do cannery traffic routing  

- Enable cloud trail integrity feature and use digest file to verify integrity  

- Local secondary index - same partition different sort key. Can’t create after table created  

- Global secondary index - don’t support strong consistency  

- S3 access logs can't send cloud watch logs. Create cloud trail with data events and use lambda to send logs to cloud watch  

- Oracle RAC not supported by RDS 

- Trusted advice notification sends summary only for every week. Use cloud watch events for immediate actions.  

- Trusted adviser can be redress via api using lambda  

- Terminating elastic beanstalk - db security group must be removed  

- Api gateway only be integrated with network load balancer  

- For DR, Much like Amazon RDS Multi-AZ, the Aurora multi-master has its data replicated across multiple Availability Zones only but not to another AWS Region. You should use Amazon Aurora Global Database instead. 

- Use RDS backup and availability events for DR use case 

- In a CodePipeline stage, you can specify parameter overrides for AWS CloudFormation actions. Parameter overrides let you specify template parameter values that override values in a template configuration file. AWS CloudFormation provides functions to help you specify dynamic values (values that are unknown until the pipeline runs). 

- To properly instrument your applications in Amazon ECS, you have to create a Docker image that runs the X-Ray daemon, upload it to a Docker image repository, and then deploy it to your Amazon ECS cluster. You can use port mappings and network mode settings in your task definition file to allow your application to communicate with the daemon container. 

- The AWS X-Ray daemon is a software application that listens for traffic on UDP port 2000, gathers raw segment data, and relays it to the AWS X-Ray API. The daemon works in conjunction with the AWS X-Ray SDKs and must be running so that data sent by the SDKs can reach the X-Ray service. 

- You can use CloudWatch Alarms to track metrics on your new deployment and you can set thresholds for those metrics in your Auto Scaling groups being managed by CodeDeploy. 

- You will also have the option to automatically roll back a deployment when a deployment fails or when a CloudWatch alarm is activated.  

- Set up an Amazon S3 data events in CloudTrail to track any object changes. Create a rule to run your Lambda function in response to an Amazon S3 data event that checks if the user who initiated the change is an administrator 

- For code deploy in onprem instances, create role, use sts, cron to refresh credentials, register instance in code deploy, create tags, create deployment group 

- Elastic beanstalk
Command section - run before application extracted and deployed
Container section - run after the application extraction but before deployment
File section - after the deployment hook 

- Elastic beanstalk support only single tier which  is web tier. App tier and  

- Dynamodb support TTL to delete old items 

- Elasticcache for redis cluster is serverlesd 

- Rds read replica or cross region read replicas are too costly  

- Rds auto snapshots happens only once per day  

- Autoscaling lifecycle target supports only CW event, sns and sqs. It can't call lambda function directly. This can be achieved only via cw event 

- Instance profile can only assume another iam role. Policies can't be attached directly to instance profile 

- Use lambda backed custom resource in cloud formation to pull new ami ids
User 

- Userdata can be used to attach instance to specific ENI 

- In ECS task definition use digest pull latest image  

- Rds EngineVersion not DBEngineVersion 

- Cloud formation intrinsic functions should be in resources section not in parameter section.  If template url used, intrinsic function not needed. Templatename.outputs.variablename 

- ELB support IPv6. Dual stack 

- Asg termination protection can't protect unhealty instances
