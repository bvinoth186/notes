# 2022 AWS Database - Specialty

1. An authentication token is a unique string of characters that Amazon RDS generates on request. Authentication tokens are generated using AWS Signature Version 4. Each token has a lifetime of 15 minutes.

1. With MySQL, authentication is handled by AWSAuthenticationPlugin—an AWS-provided plugin that works seamlessly with IAM to authenticate your IAM users.

1. AWSAuthenticationPlugin - works only with mysql

1. IAM database authentication works with MySQL and PostgreSQL. With this authentication method,  

1. By default, IAM database authentication is disabled on DB instances. You can enable IAM database authentication (or disable it again) using the AWS Management Console, AWS CLI, or the API.

1. To allow an IAM user or role to connect to your DB instance, you must create an IAM policy that allows rds-db:connect action. After that, you attach the policy to an IAM user or role. To use IAM authentication with PostgreSQL, connect to the DB instance, create database users, and then grant them the rds_iam role.

1. With IAM database authentication, you don’t need to assign database passwords to the user accounts you create. If you remove an IAM user that is mapped to a database account, you should also remove the database account with the DROP USER statement.

1. CREATE USER jon_dojo IDENTIFIED WITH AWSAuthenticationPlugin AS 'RDS';

1. The IDENTIFIED WITH clause allows MySQL to use the AWSAuthenticationPlugin to authenticate the database account (jon_dojo). The AS 'RDS' clause refers to the authentication method, and the specified database account should have the same name as the IAM user or role.

1. When you connect using SSL, your client can choose whether to verify the certificate chain. If your connection parameters specify sslmode=verify-ca or sslmode=verify-full, then your client requires the RDS CA certificates to be in their trust store or referenced in the connection URL. This requirement is to verify the certificate chain that signs your database certificate.

1. You can require that connections to your PostgreSQL DB instance use SSL by using the rds.force_ssl parameter. By default, the rds.force_ssl parameter is set to 0 (off). You can set the rds.force_ssl parameter to 1 (on) to require SSL for connections to your DB instance. Updating the rds.force_ssl parameter also sets the PostgreSQL ssl parameter to 1 (on) and modifies your DB instance’s pg_hba.conf file to support the new SSL configuration.

1. You can access Microsoft SQL Server error logs, agent logs, trace files, and dump files using the Amazon RDS console, AWS CLI, or RDS API. In addition to viewing and downloading DB instance logs, you can publish logs to Amazon CloudWatch Logs. With CloudWatch Logs, you can perform real-time analysis of the log data, store the data in highly durable storage, and manage the data with the CloudWatch Logs Agent. AWS retains log data published to CloudWatch Logs for an indefinite period unless you specify a retention period.

1. Amazon RDS provides an empty default option group for each new DB instance. You cannot modify this default option group, but any new (custom) option group that you create derives its settings from the default option group.


1. To use Performance Insights, you must enable it on your DB instance. Enabling and disabling Performance Insights does not cause downtime, a reboot, or a failover. 

1. Performance Insights can be activated without enabling Enhanced Monitoring.

1. Amazon RDS Performance Insights is configured on a per-instance level. Not cluster level

1. Amazon RDS for MySQL deployments does not support the Backtrack feature.

1. An Amazon RDS DB instance in the STORAGE_FULL state doesn’t have enough available space to perform basic operations, such as connecting to or restarting the instance. To resolve this issue, follow these steps:
   - Confirm that the DB instance status is STORAGE_FULL.
   - Add more storage space to the instance.
   - Increase the allocated storage property of your DB instance.

1. RDS storage autoscaling feature can’t completely prevent storage-full situations for large data loads because further storage modifications can’t be made until six hours after storage optimization has completed on the instance. If you perform a large data load, and autoscaling doesn’t provide enough space, the database might remain in the storage-full state for several hours.

1. If you disable point-in-time recovery and later re-enable it on a table, you reset the start time for which you can recover that table. As a result, you can only immediately restore that table using the LatestRestorableDateTime.

1. To launch an Amazon RDS DB instance, an RDS DB subnet group must contain at least two subnets. These subnets must be in different Availability Zones in the same AWS Region. You can remove or delete a subnet from the DB subnet group only if there are no DB instances associated with the subnet group and launched in the subnet that you’re trying to delete. If you launch a DB instance with a DB subnet group that contains two subnets in two Availability Zones, then you can’t delete any subnet from the DB subnet group.

1. If you have a Multi-AZ deployment that has two or more subnets in a subnet group, then you can launch the DB instance in any of the subnets of the two Availability Zones. If you have a Single-AZ deployment that has two or more subnets in the subnet group, then you can specify the Availability Zone when you create a DB instance. If you didn’t specify the Availability Zone when you created the DB instance, then the DB instance is launched in any of the subnets of the two Availability Zones.

1. To delete a subnet from a DB subnet group, isolate the subnet by moving the DB instance to another subnet. Then, remove the subnet from the DB subnet group
   - If the primary instance in a DB cluster using single-master replication fails, Aurora automatically fails over to a new primary instance in one of two ways:
   - By promoting an existing Aurora Replica to the new primary instance
   - By creating a new primary instance


1. When you export a DB snapshot, Amazon RDS extracts data from the snapshot and stores it in an Amazon S3 bucket in your account. The data is stored in an Apache Parquet format that is compressed and consistent.

1. You can export all types of DB and DB cluster snapshots including manual snapshots, automated system snapshots, and snapshots created by the AWS Backup service. By default, all data in the snapshot is exported. However, you can choose to export specific sets of databases, schemas, or tables.

1. automated snapshots can only be retained for a maximum of 35 days.

1. For Oracle – AWS DMS uses either the Oracle LogMiner API or binary reader API (bfile API) to read ongoing changes. AWS DMS reads ongoing changes from the online or archive redo logs based on the system change number (SCN).

1. For Microsoft SQL Server – AWS DMS uses MS-Replication or MS-CDC to write information to the SQL Server transaction log. It then uses the fn_dblog() or fn_dump_dblog() function in SQL Server to read the changes in the transaction log based on the log sequence number (LSN).

1. For MySQL – AWS DMS reads changes from the row-based binary logs (binlogs) and migrates those changes to the target.

1. For PostgreSQL – AWS DMS sets up logical replication slots and uses the test_decoding plugin to read changes from the source and migrate them to the target.

1. For Amazon RDS as a source – AWS recommends ensuring that backups are enabled to set up CDC. It is also recommended to ensure that the source database is configured to retain change logs for a sufficient time—24 hours is usually enough.

1. You can set up the DMS replication instance in the same VPC, Availability Zone, and AWS Region of either the source or target database. However, if you are only migrating or replicating a subset of data using filters or transformations, it is recommended that you launch the replication instance on the same VPC and Availability Zone where the source database is, to optimize the processing. Most of the time, the amount of data transferred over the network to the target database is less compared with the source database.

1 AWS Database Migration Services (DMS) provides support for data validation to ensure that your data was migrated accurately from the source to the target. If you enable it for a task, AWS DMS begins comparing the source and target data immediately after a full load is performed for a table, and reports any mismatches. Also, for a CDC-enabled task, AWS DMS compares the incremental changes and reports any mismatches. 

1. Create the AWS DMS task with Enable validation and Enable CloudWatch logs settings turned on.

1. An important part of the AWS Schema Conversion Tool is the database migration assessment report that it generates to help you convert your schema. The report summarizes all of the schema conversion tasks and details the action items for schema that can’t be converted to the DB engine of your target DB instance. You can view the report in the application. To do so, export it as a comma-separated value (CSV) or PDF file.

1. The migration assessment report includes:
   1) License evaluation.
   2) Cloud support, indicating any features in the source database not available on the target.
   3) Current source hardware configuration
   4) Recommendations, including conversion of server objects, backup suggestions, and linked server changes

1. When you migrate data from one database to another, you might take the opportunity to rethink how your LOBs are stored, especially for heterogeneous migrations. In limited LOB mode, you set a maximum size LOB that AWS DMS should accept. Doing so allows AWS DMS to pre-allocate memory and load the LOB data in bulk. LOBs that exceed the maximum LOB size are truncated, and a warning is issued to the log file. As of Aug 2021, the maximum permitted value is 102400 KB (100 MB). In limited LOB mode, you get significant performance gains over full LOB mode. We recommend that you use limited LOB mode whenever possible, especially when the LOB size is deterministic. Note, however, that if the LOB sizes are significantly large (e.g., 3GB), performance could be better if you use a DMS task in full lob mode with inline lob mode settings.

1. the large replication instance is expensive, and migrating all the data using AWS DMS will be costly.

1. By default, AWS DMS loads eight tables at a time. You might see some performance improvement by increasing this slightly when using a huge replication server, such as a dms.c4.xlarge or larger instance. However, at some point, increasing this parallelism reduces performance. If your replication server is relatively small, such as a dms.t2.medium, we recommend that you reduce the number of tables loaded in parallel.
If you want to improve the performance when migrating a large table, you can break the migration into more than one task. To break the migration into multiple tasks using row filtering, use a key or a partition key.

1. DeleteAutomatedBackup – A value that indicates whether to remove automated backups immediately after the DB instance is deleted. This parameter isn’t case-sensitive. The default is to remove automated backups immediately after the DB instance is deleted. 

1. Amazon RDS for MySQL uses asynchronous replication and sometimes the replica isn’t able to keep up with the primary DB instance. This can cause a replication lag.
   - Reasons for replication lag
   - Long-running queries on the primary DB instance
   - Insufficient DB instance class size or storage
   - Parallel queries executed on the primary DB instance
   - Binary logs synced to the disk on the replica DB instance
   - Binlog_format on the replica is set to ROW
   - Replica creation lag

1. Performance Insights is not capable of providing you how different processes or threads on a DB instance use the CPU in real-time. It can only visualize the database load and filter the load by waits, SQL statements, hosts, or users.

1. Amazon RDS provides metrics in real time for the operating system (OS) that your DB instance runs on. You can view the metrics for your DB instance using the console, or consume the Enhanced Monitoring JSON output from CloudWatch Logs in a monitoring system of your choice. By default, Enhanced Monitoring metrics are stored in the CloudWatch Logs for 30 days

1. DB instance only accepts allocated storage modifications if the database is in STORAGE_FULL state. In this scenario, you have to increase the allocated storage property of the RDS DB instance first before you can enable storage autoscaling. 

1. autoscaling feature can’t completely prevent storage-full situations because further storage modifications can’t be made until six hours after the storage optimization has completed.

1. You can only enable encryption for an Amazon RDS DB instance when you create it, not after the DB instance is created. As a workaround, you can encrypt a copy of an unencrypted DB snapshot, then restore a DB instance from the encrypted snapshot.
   – DB instances that are encrypted can’t be modified to disable encryption.
   – You can’t have an encrypted read replica of an unencrypted DB instance or an unencrypted read replica of an encrypted DB instance.
   – Encrypted read replicas must be encrypted with the same key as the source DB instance when both are in the same AWS Region.
   – You can’t restore an unencrypted backup or snapshot to an encrypted DB instance.
   – To copy an encrypted snapshot from one AWS Region to another, you must specify the KMS key identifier of the destination AWS Region. This is because KMS encryption keys are specific to the AWS Region that they are created in.

1. Rds - for minimal application downtime during failover - Enable Aurora DB cluster cache management in the associated parameter group. Set the TCP keepalive parameters for the DB and the application client to a low value.

1. You can modify parameter values in a custom DB parameter group. However, you can’t change the parameter values in a default DB parameter group. To change an RDS DB instance configuration, you must change the parameter values of the DB parameter group for your RDS DB instance.

1. Instances require a manual reboot in the following circumstances:
   – If you replace the current parameter group with a different parameter group.
   – If you modify and save a static parameter in a custom parameter group.

1. In a Multi-AZ deployment, Amazon RDS applies operating system updates by following these steps:
   1) Perform maintenance on the standby.
   2) Promote the standby to primary.
   3) Perform maintenance on the old primary, which becomes the new standby.

1. Take note that this does not apply to database engine updates. If database engine modification occurs, both the primary and secondary DB instances are shut down at the same time.

1. Here are some automated monitoring tools to watch Amazon RDS and report when something is wrong:
   – Amazon RDS Enhanced Monitoring — Look at metrics in real-time for the operating system
   – Amazon RDS Performance Insights — Assess the load on your database, and determine when and where to take action.
   – Database log files – View, download, or watch database log files using the Amazon RDS console or Amazon RDS API operations. You can also query some database log files that are loaded into database tables.
   – AWS CloudTrail – You can view a record of actions taken by a user, role, or an AWS service in Amazon RDS. CloudTrail captures all API calls for Amazon RDS as events.

1. Remember that after you create an RDS DB instance, you cannot modify the DB instance’s allocated storage size.

1. The snapshot will create the DB instance with the same storage size as the old instance even though if rds didn't consume full storage

1. Before you move the RDS DB instance to a new network, configure the new VPC, including the security group inbound rules, the subnet group, and the route tables. When you change the VPC for a DB instance, the instance reboots when the instance moves from one network to another. Because the DB instance isn’t accessible during the network modification, change the VPC during a scheduled maintenance window.

