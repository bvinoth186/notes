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

1. AWS Database Migration Services (DMS) provides support for data validation to ensure that your data was migrated accurately from the source to the target. If you enable it for a task, AWS DMS begins comparing the source and target data immediately after a full load is performed for a table, and reports any mismatches. Also, for a CDC-enabled task, AWS DMS compares the incremental changes and reports any mismatches. 

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

1. One cannot modify the VPC of an Aurora cluster or instance. However, you can change the VPC of an Aurora cluster by using one of the following methods:  (Single AZ RDS VPC can be mofified, Aurora storage is multi AZ by default)
   1) Create a clone in a different VPC.
   2) Take a snapshot and then restore the snapshot in a different VPC.
   3) Set up replication using binary logging (MySQL only).

1. You can use the high-performance Advanced Auditing feature in Amazon Aurora MySQL to audit database activity. To do so, you enable the collection of audit logs by setting several DB cluster parameters. When Advanced Auditing is enabled, you can use it to log any combination of supported events.

1. Use the server_audit_logging parameter to enable or disable Advanced Auditing.

1. Use the server_audit_events parameter to specify what events to log.

1. Use the server_audit_incl_users and server_audit_excl_users parameters to specify who gets audited. By default, all users are audited. 


1. CONNECT – Logs both successful and failed connections and also disconnections. This event includes user information.

1. QUERY – Logs all queries in plain text, including queries that fail due to syntax or permission errors..

1. QUERY_DCL – Similar to the QUERY event, but returns only data control language (DCL) queries (GRANT, REVOKE, and so on).

1. QUERY_DDL – Similar to the QUERY event, but returns only data definition language (DDL) queries (CREATE, ALTER, and so on).

1. QUERY_DML – Similar to the QUERY event, but returns only data manipulation language (DML) queries (INSERT, UPDATE, and so on, and also SELECT).

1. TABLE – Logs the tables that were affected by query execution.


1. Each DB instance in an Amazon Aurora DB cluster uses local solid-state drive (SSD) storage to store temporary tables for a session. This local storage for temporary tables doesn’t automatically grow like the Aurora cluster volume. Instead, the amount of local storage is limited. The limit is based on the DB instance class for DB instances in your DB cluster.

1. Instances in Aurora clusters have two types of storage:

1. Storage for persistent data (called the cluster volume). This storage type increases automatically when more space is required.
Local storage for each Aurora instance in the cluster, based on the instance class. This storage type and size is bound to the instance class, and can be changed only by moving to a larger DB instance class. Aurora for MySQL uses local storage for storing error logs, general logs, slow query logs, audit logs, and non-InnoDB temporary tables.

1. Error code 1290 commonly occurs in an Amazon Aurora DB cluster environment whenever the database cluster is configured to be read-only or if the connection is attempting to commit write transactions to an endpoint that points to the Aurora replica. Reader endpoint

1. The maximum number of connections allowed to an Aurora MySQL DB instance is determined by the max_connections parameter in the instance-level parameter group for the DB instance.

1. The default value of the max_connections parameter varies for each DB instance class available to Aurora MySQL. You can increase the maximum number of connections to your Aurora MySQL DB instance by scaling the instance up to a DB instance class with more memory, or by setting a larger value for the max_connections parameter in the DB parameter group for your instance, up to 16,000.

1. With Amazon Aurora with MySQL compatibility, you can backtrack a DB cluster to a specific time, without restoring data from a backup.

1. Backtracking allows you to rewind the DB cluster to the time you specify. When you specify a time for a backtrack, Aurora automatically chooses the nearest possible consistent time. 

1. If two or more Aurora Replicas share the same priority, then Amazon RDS promotes the replica that is largest in size. If two or more Aurora Replicas share the same priority and size, then Amazon RDS promotes an arbitrary replica in the same promotion tier.

1. You can test the fault tolerance of your Amazon Aurora DB cluster by using fault injection queries. Only available with Aurora not in RDS

1. For Aurora MySQL, you can’t delete a DB instance in a DB cluster if both of the following conditions are true:
   - The DB cluster is a read replica of another Aurora DB cluster
   - The DB instance is the only instance in the DB cluster.

1. To delete a DB instance in this case, first promote the DB cluster so that it’s no longer a read replica. After the promotion completes, you can delete the final DB instance in the DB cluster.

1. To delete an Aurora cluster using the AWS CLI, you must first delete all instances inside the cluster. After you delete all instances inside a cluster, you can then delete the cluster by using delete-db-cluster. If you delete the last instance in the cluster using the Amazon RDS console, the empty cluster is also automatically deleted. If you have a cluster with only one instance and delete that instance using the Amazon RDS console, then both that instance and the cluster are deleted.

1. RDS allows you to create an Aurora Read Replica. The migration process begins by creating a DB snapshot of the existing DB Instance and then using it as the basis for a fresh Aurora Read Replica. After the replica has been set up, replication is used to bring it up to date with respect to the source. Once the replication lag drops to 0, the replication is complete. At this point, you can make the Aurora Read Replica a standalone Aurora DB cluster and point your client applications at it.

1. Using database cloning, you can quickly and cost-effectively create clones of all of the databases within an Aurora DB cluster. Not available with RDS

1. To meet your connectivity and workload requirements, Aurora Auto Scaling dynamically adjusts the number of Aurora Replicas provisioned for an Aurora DB cluster using single-master replication. Aurora Auto Scaling is available for both Aurora MySQL and Aurora PostgreSQL. Aurora Auto Scaling enables your Aurora DB cluster to handle sudden increases in connectivity or workload. When the connectivity or workload decreases, Aurora Auto Scaling removes unnecessary Aurora Replicas so that you don’t pay for unused provisioned DB instances.

1. For Amazon Aurora, you can use the LOAD DATA FROM S3 or LOAD XML FROM S3 statement to load data from files stored in an Amazon S3 bucket. The advantages of using this method diminish rapidly as transaction size increases.

1. For fast recovery of the writer DB instance in your Aurora PostgreSQL clusters if there’s a failover, use cluster cache management for Amazon Aurora PostgreSQL. Cluster cache management ensures that application performance is maintained if there’s a failover.

1. Configure all Aurora Replicas to have the same instance class as the primary DB instance. Implement Aurora PostgreSQL DB cluster cache management. Set the failover priority to tier-0 for the primary DB instance and one replica with the same instance class. Set the failover priority to tier-1 for the other Aurora Replicas.

1. To migrate from an RDS PostgreSQL DB instance to an Aurora PostgreSQL DB cluster, it is recommended to create an Aurora Read Replica of your source PostgreSQL DB instance. When the replica lag between the PostgreSQL DB instance and the Aurora PostgreSQL Read Replica is zero, you can stop replication. At this point, you can promote the Aurora Read Replica to be a standalone Aurora PostgreSQL DB cluster. This standalone DB cluster can then accept write loads.

1. To meet your connectivity and workload requirements, Aurora Auto Scaling dynamically adjusts the number of Aurora Replicas provisioned for an Aurora DB cluster using single-master replication.

1. When you create a read replica, you first specify an existing DB instance as the source. Then Amazon RDS takes a snapshot of the source instance and creates a read-only instance from the snapshot. Amazon RDS then uses the asynchronous replication method for the DB engine to update the read replica whenever there is a change to the primary DB instance. The read replica operates as a DB instance that allows only read-only connections. Applications connect to a read replica the same way they do to any DB instance. Amazon RDS replicates all databases in the source DB instance.

1. When creating a read replica, there are a few things to consider. First, you must enable automatic backups on the source DB instance by setting the backup retention period to a value other than 0.

1. DB clusters that are encrypted can’t be modified to disable encryption. You can’t convert an unencrypted DB cluster to an encrypted one. However, you can restore an unencrypted Aurora DB cluster snapshot to an encrypted Aurora DB cluster. To do this, specify a KMS encryption key when you restore from the unencrypted DB cluster snapshot. How about RDS?

1. For Aurora MySQL, you can’t delete a DB instance in a DB cluster if both of the following conditions are true:
   - The DB cluster is a read replica of another Aurora DB cluster
   - The DB instance is the only instance in the DB cluster.
   - To delete a DB instance in this case, first promote the DB cluster so that it’s no longer a read replica. After the promotion completes, you can delete the final DB instance in the DB cluster.

1. Amazon Aurora has four types of endpoints available:
   - cluster endpoint (or writer endpoint),
   - reader endpoint,
   - instance endpoint
   - custom endpoint
   
1. Each Aurora cluster has a single built-in reader and cluster endpoint, whose name and other attributes are managed by Aurora. You cannot create, delete, or modify both kinds of endpoints. An instance endpoint connects to a specific DB instance within an Aurora cluster.

1. Meanwhile, each custom endpoint has an associated type that determines which DB instances are eligible to be associated with that endpoint. Currently, the type can be READER, WRITER, or ANY. Only DB instances that are read-only Aurora Replicas can be part of a READER custom endpoint. Both read-only Aurora Replicas and the read/write primary instance can be part of an ANY custom endpoint. Aurora directs connections to cluster endpoints with type ANY to any associated DB instance with equal probability. The WRITER type applies only to multi-master clusters because those clusters can include multiple read/write DB instances.

1. You can configure an Aurora Serverless DB cluster when you restore a provisioned DB cluster snapshot with the AWS Management Console, the AWS CLI, or the RDS API.   You can't promote existing read replica to aurora serverless can do only from snapshot

1. With Amazon DocumentDB (with MongoDB compatibility), you can audit events that were performed in your cluster. Examples of logged events include successful and failed authentication attempts, dropping a collection in a database, or creating an index. By default, auditing is disabled on Amazon DocumentDB and requires that you opt-in to use this feature.

1. When auditing is enabled, Amazon DocumentDB records Data Definition Language (DDL), authentication, authorization, and user management events to Amazon CloudWatch Logs.

1. DocumentDB requires manual management to scale the database, unlike DynamoDB

1. DynamoDB deletes expired items on a best-effort basis to ensure there’s enough throughput for other data operations. Depending on the size and activity level of a table, an expired item’s actual delete operation can vary. Because TTL is meant to be a background process, the nature of the capacity used to expire and delete items via TTL is variable (but free of charge). TTL typically deletes expired items within 48 hours of expiration. Processing takes place automatically, in the background, and doesn’t affect read or write traffic to the table.

1. A local secondary index maintains an alternate sort key for a given partition key value. A local secondary index also contains a copy of some or all of the attributes from its base table; you specify which attributes are projected into the local secondary index when you create the table. The data in a local secondary index is organized by the same partition key as the base table, but with a different sort key. This lets you access data items efficiently across this different dimension. For greater query or scan flexibility, you can create up to five local secondary indexes per table.

1. To create a Local Secondary Index, make sure that the primary key of the index is the same as the primary key/partition key of the table, just as shown below. Then you must select an alternative sort key which is different from the sort key of the table.

1. Strongly consistent reads are not supported on global secondary indexes.

1. Local secondary indexes are created at the same time that you create a table. You cannot add a local secondary index to an existing table, nor can you delete any local secondary indexes that currently exist.

1. You can reduce the cost of your provisioned DynamoDB by purchasing reserved capacity in advance. By reserving your read and write capacity units ahead of time, you will get significant cost savings compared to on-demand provisioned throughput settings. Any capacity that you provision in excess of your reserved capacity is billed at standard provisioned capacity rates.

1. DynamoDB backups do not guarantee causal consistency across items; however, the skew between updates in a backup is usually much less than a second.

1. While a backup is in progress, you cannot do the following:
   – Pause or cancel the backup operation.
   – Delete the source table of the backup.
   – Disable backups on a table if a backup for that table is in progress

1. All backups in DynamoDB work without consuming any provisioned throughput on the table.

1. You can use the profiler in Amazon DocumentDB (with MongoDB compatibility) to log the execution time and details of operations that were performed on your cluster. The profiler is useful for monitoring the slowest operations on your cluster to improve individual query performance and overall cluster performance.

1. Amazon Neptune provides a Loader command for loading data from external files directly into a Neptune DB instance. You can use this command instead of executing a large number of INSERT statements, addVertex and addEdge steps, or other API calls. The Neptune Loader command is faster, has less overhead, is optimized for large datasets, and supports both Gremlin data and the RDF (Resource Description Framework) data used by SPARQL.

1. Amazon Timestream - database service is designed to easily store and analyze trillions of events per day – data that measures how things change over time

1. Amazon Quantum Ledger Database (Amazon QLDB) is a fully managed ledger database that provides a transparent, immutable, and cryptographically verifiable transaction log owned by a central trusted authority. Amazon QLDB can be used to track every application data change and maintains a complete and verifiable history of changes over time.

1. AUTH can only be enabled for ElastiCache for Redis clusters if in-transit encryption was enabled during creation. Furthermore, you can only enable in-transit encryption when you create an ElastiCache for Redis replication group using the AWS Management Console, the AWS CLI, or the ElastiCache API.

1. By default, the data in a Redis node on ElastiCache resides only in memory and is not persistent. If a node is rebooted, or if the underlying physical server experiences a hardware failure, the data in the cache is lost.

1. If you require data durability, you can enable the Redis append-only file feature (AOF). When this feature is enabled, the node writes all of the commands that change cache data to an append-only file. When a node is rebooted and the cache engine starts, the AOF is “replayed”; the result is a warm Redis cache with all of the data intact.

1. AOF is disabled by default

1. The default TCP port for Redis is 6379. This port must be allowed in the associated security group of the cluster. For Memcached, the default port is 11211.

1. You can add data to your Amazon Redshift tables either by using an INSERT command or by using a COPY command. At the scale and speed of an Amazon Redshift data warehouse, the COPY command is many times faster and more efficient than INSERT commands.

1. The COPY command uses the Amazon Redshift massively parallel processing (MPP) architecture to read and load data in parallel from multiple data sources. You can load from data files on Amazon S3, Amazon EMR, or any remote host accessible through a Secure Shell (SSH) connection. Or you can load directly from an Amazon DynamoDB table.

1. Previously, you need to extract data from your PostgreSQL database to Amazon Simple Storage Service (Amazon S3) and load it to Amazon Redshift using COPY, or query it from Amazon S3 with Amazon Redshift Spectrum. Now, Amazon Redshift Federated Query enables you to use the analytic power of Amazon Redshift to directly query data stored in Amazon Aurora PostgreSQL and Amazon RDS for PostgreSQL databases.

1. Loading flat files with LOAD DATA LOCAL INFILE can be the fastest and least costly method of loading data as long as transactions are kept relatively small. Compared to loading the same data with SQL, flat files usually require less network traffic, lowering transmission costs, and load much faster due to the reduced overhead in the database.

1. Amazon Redshift also includes Amazon Redshift Spectrum, allowing you to run SQL queries directly against exabytes of unstructured data in Amazon S3 data lakes. No loading or transformation is required, and you can use open data formats, including Avro, CSV, Grok, Amazon Ion, JSON, ORC, Parquet, RCFile, RegexSerDe, Sequence, Text, and TSV. Redshift Spectrum automatically scales query compute capacity based on the data retrieved, so queries against Amazon S3 run fast, regardless of data set size.

1. Concurrency Scaling is a feature in Amazon Redshift that provides consistently fast query performance, even with thousands of concurrent queries. With this feature, Amazon Redshift automatically adds transient capacity when needed to handle heavy demand. Amazon Redshift automatically routes queries to scaling clusters, which are provisioned in seconds and begin processing queries immediately.

1. When you launch an Amazon Redshift cluster, you can choose to encrypt it with a master key from the AWS Key Management Service (AWS KMS). AWS KMS keys are specific to a region. If you want to enable cross-region snapshot copy for an AWS KMS-encrypted cluster, you must configure a snapshot copy grant for a master key in the destination region so that Amazon Redshift can perform encryption operations in the destination region.

1. Amazon Redshift uses the Amazon Simple Notification Service (Amazon SNS) to communicate notifications of Amazon Redshift events. You enable notifications by creating an Amazon Redshift event subscription. In the Amazon Redshift subscription, you specify a set of filters for Amazon Redshift events and an Amazon SNS topic.

