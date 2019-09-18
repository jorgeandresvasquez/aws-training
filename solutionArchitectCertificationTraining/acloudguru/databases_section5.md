# Databases 101
- 6 different flavors of Relational databases on AWS:
    1. SQL Server
    2. Oracle
    3. MySQL Server
    4. PostgreSQL
    5. Aurora
    6. MariaDB
- 2 key features:
    - Multi-AZ-For Disaster Recovery
        - If you loose our primary database AWS will detect and will automatically update DNS record to point to Secondary DB in another AZ
        - Allows you to have an exact copy of your production database in another AZ
        - Synchronous replication
        - You can force it by rebooting an RDS instance
    - Read Replicas - For Performance
        - You can have up to 5 copies of read replica
        - You can have replicas of read replicas (watch out for replication latency)
        - replication is asynchronous
        - Different EC2 instances can read from different replicas but they all write to same instance
        - Usage for very read-heavy database workloads
        - You van have read replicas that have Multi A-Z
        - You can have a read replica in a second region
        - To setup a read replica backups must be turned on
        - A read replica can be promoted to master but it will break the read replica
- Non-Relational DBs
    - Collection = Table
    - Document = Row
    - Key Value Pairs = Fields
- Data Warehousing
    - Used for Business Intelligence
    - Used to pull in very large and complex data sets.  Usually used by management to do queries on data.
- OLTP (Online Transaction Processing) vs OLAP (Online Analytics Processing)
- The Multi-AZ deployments are not a read scaling solution, you cannot use a standby replica to serve read traffic. Multi-AZ maintains a standby replica for HA/failover. It is available for use only when RDS promotes the standby instance as the primary. To service read-only traffic, you should use a Read Replica instead.

## RDS
- Runs on virtual machines but you cannot log into these operating systems
- RDS is not serverless
- Patching of the RDS Operating System and DB is Amazon's responsibility
- Aurora Serverless is Serverless
- Backups
    - Automated Backups
        - Allow you to recover your database to any point in time within a "retention period" (1-35 days)
        - Will take a full daily snapshot and will also store transaction logs throughout the day
        - When you do a recovery, AWS will choose the most recent daily back up, and then apply transaction logs relevant to that day
        - Allows to to a point in time recovery down to a second
        - Are enabled by default
        - Backup data is stored in S3 and you get free storage space equal to the size of your DB
        - Backups are taken within a defined window
            - during this window storage I/O may be suspended and you may experience elevated latency
        - Under normal circumstances, all automatic backups of an RDS instance are deleted upon termination.
    - Database Snapshots
        - Done manually
        - User initiated
        - They are stored even after you delete the original RDS instance, unlike automated backups
            - Unless the 'SkipFinalSnapshot' parameter was set to 'False.'
    - Restoring backups
        - When you restore either an automated backup or a manual snapshot the restored version of the DB will be a new RDS instance with a new DNS endpoint
- Encryption
    - Uses KMS service
    - Once RDS instance is encrypted, the data at rest in the storage is encrypted as well as automated backups, read replicas, and snapshots
- For production workloads, it is recommended that you use Provisioned IOPS and DB instance classes that are optimized for Provisioned IOPS for fast, consistent performance.
- When creating an RDS instance, you can select the Availability Zone into which you deploy it.
- RDS Reserved instances are available for multi-AZ deployments.
- Under what circumstances would I choose provisioned IOPS over standard storage when creating an RDS instance?
    - If you use Online Transaction Processing in your production environment
    - Note:  The options for Storage type are:  
        - Provisioned IOPS (SSD)
            - For I/O intensive workloads
        - General Purpose (SSD)
- If you are using Amazon RDS Provisioned IOPS storage with a Microsoft SQL Server database engine, what is the maximum size RDS volume you can have by default?
    - 16 TB
- In RDS, changes to the backup window take effect immediately
- With a DB security group the RDS instance port number is automatically applied to the RDS DB Security Group.
- A reboot simulates a failover from one AZ to another
- If the Amazon RDS instance is configured for Multi-AZ, you can perform the reboot with a failover. An Amazon RDS event is created when the reboot is completed. If your DB instance is a Multi-AZ deployment, you can force a failover from one Availability Zone (AZ) to another when you reboot. When you force a failover of your DB instance, Amazon RDS automatically switches to a standby replica in another Availability Zone, and updates the DNS record for the DB instance to point to the standby DB instance.
- How many databases or schemas can I run within a DB instance?
    - RDS for Oracle: 1 database per instance; no limit on number of schemas per database imposed by software
        - Supports Read Replicas
    - RDS for SQL Server: Up to 100 databases per instance see here: Amazon RDS SQL Server User Guide
    - Other:  No limit imposed by software
- If your application requires more compute resources than the largest DB instance class or more storage than the maximum allocation, you can implement partitioning (aka:  sharding), thereby spreading your data across multiple DB instances.

## DynamoDB
- NoSQL database service
- Stored on SSD storage
- Spread across 3 geographically distinct data centres
- Eventual Consistence Reads (Default)
    - Best Read Performance
    - 1 second rule, if application is happy reading updated after 1 sec then this is a valid option otherwise Strongly Consistent Reads
- Strongly Consistent Reads 
    - If reading data within 1 second of it written to the table
- Amazon DynamoDB stores data in partitions. A partition is an allocation of storage for a table, backed by solid state drives (SSDs) and automatically replicated across multiple Availability Zones within an AWS Region. Partition management is handled entirely by DynamoDB—you never have to manage partitions yourself.
- The cumulative size of attributes per item must fit within the maximum DynamoDB item size (400 KB).
- There is a 10GB limit on every partition key. 
- 2 types of Read/write capacity modes:
    1. On-demand
        - No capacity planning
    2. Provisioned
        - Requires you to specify the number of reads and writes per second as required by the application
        - Requires the following capacity units:
            1. RCU (Read Capacity Units)
                - Depend on consistent read model
            2. WCU (Write Capacity Units)
- When you request a strongly consistent read, DynamoDB returns a response with the most up-to-date data, reflecting the updates from all prior write operations that were successful. A strongly consistent read might not be available if there is a network delay or outage. Strongly consistent reads are not supported on global secondary indexes
- If data consistency is absolutely required the development team has to code for strongly consistent reads
            
## Redshift
- AWS's Data Warehousing solution
- Fully managed petabyte-scale 
- Use different architecture from database perspective and infrastructure layer
- Configuration Options:
    - Single Node (160GB)
    - Multi Node
        - Leader Node
            - Manages Client Connections and receives queries
        - Compute Node
            - Store data and perform queries and computations
            - up to 128 compute nodes
- Columnar data stores can be compressed much more than row-based data stores 
- Doesn't require indexes or materialized views and so uses less space than traditional relational database systems
- When loading data into an empty table automatically samples it and selects most appropriate compression scheme
- Massive Parallel Processing (MPP)
    - Automatically distributes data and query load across all nodes
- Backups:
    - Enabled by default for 1 day retention period
    - Maximum retention period is 35 days
    - Can aysnchronously replicate snapshots to S3 in another region for disaster recovery
- Pricing:
    - Compute Node Hours
    - Not charged for leader node hours but for compute node hours
    - Biller for 1 unit per node per hour
    - Charged for backups
- Security
    - Encrypted at Rest using AES-256 encryption
    - Encrypted in transit using SSL
    - Takes care of key management by default
    - You can also use:
        - HSM
        - KMS
- Currently only available in 1 AZ
- Can restore snapshots to new AZs in the event of outage
- Amazon Redshift Enhanced VPC Routing provides VPC resources access to Redshift.
    - Redshift will not be able to access the S3 VPC endpoints without enabling Enhanced VPC routing

## Aurora
- Start with 10GB, scales in 10GB increments to 64TB (Storage autoscaling)
- Compute resources can scale up to 32vCPUs and 244GB of memory
- 2 copies of your data contained in each AZ, with a minimum of 3 AZs, therefore 6 copies of your data
    - Only available in regions with 3 AZs
- Designed to transparently handle the loss of up to 2 copies of data without affecting write availability and up to 3 copies without affecting read availability
- Self healing
- Read Replicas 
    - Upt to 15
- Backups do not impact db performance
- You can take snapshots and they don't impact performance
- You can share Aurora snapshots with other AWS accounts
- You can do migrations from mysql to aurora simply by creating read replica and promoting it or creating a snapshot and restoring from it
- Database Options:
    - One writer and multiple readers
    - One writer and multiple readers - parallel query
    - Multiple writers
    - Serverless
- Your company currently has data hosted in an Amazon Aurora MySQL DB. Since this data is critical, there is a need to ensure that it can be made available in another region in case of a disaster. How can this be achieved?
    - You can create an Amazon Aurora MySQL DB cluster as a Read Replica in a different AWS Region than the source DB cluster. Taking this approach can improve your disaster recovery capabilities, let you scale read operations into an AWS Region that is closer to your users, and make it easier to migrate from one AWS Region to another.
- Aurora replicates your data across three availability zones, at the storage layer... but the database server instance, itself, is still a virtual machine running on a single physical machine that is located in a single availability zone
    - The Aurora storage layer is outside that instance, and is able to let access continue uninterrupted without data loss, even in the event of the loss of up to two AZs, but the loss of the zone containing the db instance will still cause an outage for you, if you only have a single Aurora instance in your cluster (1 master, 0 replicas). Loss of an entire availability zone is one of those things that is highly improbable but not impossible. Your db instance is still a single point of failure when you only have one
    - Multi-AZ makes allowance for a complete redundant database instance, in a different AZ, which will automatically take over for the primary within one minute, if it works as designed, in case of the loss of the AZ hosting the primary instance or a catastrophic failure of the primary instance. It's a second virtual machine, on a second physical machine, in a second availability zone. It's always running, but you can't access it. It's in the background, managed and monitored by the RDS infrastructure, but it is only accessible to you in the case of primary instance failure. The secondary machine can also be used to reduce downtime in the event of a software upgrade or maintenance event on the primary. When failover occurs, if you are using DNS to connect to your database (as you should), you'll find that the DNS entry is automatically pointed to the secondary.

## Elasticache
- Web Service that makes it easy to deploy, operate, and scale an in-memory cache in the cloud
- improves performance by retrieving info from fast, managed, in-memory caches instead on relying entirely on slower disk-based databases
- Supports 2 open source caching engines:
    - memcached
        - simpler and easier
        - Does not support native encryption at rest
    - redis
        - more advanced data types
        - multi AZ
        - can do backups and restores
        - Supports native encryption at rest




