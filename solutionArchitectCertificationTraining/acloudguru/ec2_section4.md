# EC2 101
- Amazon Elastic Compute Cloud is a web service that provides resizable compute capacity in the cloud
- AWS philosophy on pricing:
    - You pay as you go, pay for what you use, pay less as you use more, and pay even less when you reserve capacity
- Pricing models:
    - On demand
        - pay a fixed rate by the hour with no commitment
        - Apps with unpredictable workloads
        - Apps being developed and tested for the first time
    - Reserved
        - Capacity reservation
        - Contract terms:
            - 1 year or 3 years
        - Useful for apps with steady state or predictable usage
        - Cannot be moved from one region to another
        - Pricing types:
            - Standard reserved instances
                - up to 75% of on demand instances
                - the more the commitment the greater the discount
            - Convertible Reserved instances
                - up to 54% off on demand instances
                - You can change attributes of the RI as long as exchange results in RIs of equal or greater value
            - Scheduled Reserved Instances
                - Available to launch within the time windows you reserve
    - Spot
        - bid on capacity, you set the price, if hits the price you get instances otherwise you loose them again in couple of minutes
        - Useful for applications that have flexible start and end times
        - Apps that are feasible at very low compute prices
        - If the spot instance is terminated by Amazon EC2 you will not be charged by partial hour of usage, if you terminate it they will charge you for any hour in which the instance run.
    - Dedicated
        - physical hosts dedicated
        - useful for regulatory requirements
            - Ex:  No multi-tenant virtualization
        - useful for licensing agreements
        - Can be purchased on demand (hourly)
        - Can be purchased as reservation for up to 70% off
        - Types:
            - Host
                - isolated server with configurations that you can control
            - Dedicated
        - You can change the tenancy of an instance from dedicated to host, or from host to dedicated after you've launched it.
- EC2 instance types
    - An instance type is an available configuration of CPU, memory, storage, and networking capacity
    - F1
        - Field Programmable Gate Array, reprogramming of chips after manufactured
    - I3 
        - High Speed Storage (IOPS)
    - G3
        - Graphics Intensive
    - H1
        - High Disk Throughput
            - Data transfer rate to/from the storage media
    - T3
        - Lowest cost, general purpose (Think T2 Micro)
        - Web Servers
    - D2 
        - Dense Storage
    - R5
        - RAM
        - Memory Optimized
    - M5 
        - Main Choice for General Purpose
    - C5
        - Compute-Optimized
    - P3
        - General purpose GPU
        - Pics for Graphics
    - X1
        - Memory Optimized
        - Extreme Memory
        - X1e was specifically created to run high performance databases
    - Z1D
        - High Compute Capacity and high memory footprunt
    - A1
        - ARM-based workloads
    - U-6tb1
        - Bare Metal
    - FIGHTDRMCPXZAU
    - The numbers of the instance types represent the generation
- Support key pairs to securely login into instance, Amazon stores public key and customer the private key
- Instance Store Volumes
    - Storage volumes for temporary data that's deleted when you stop or terminate your instance
- Amazon EBS Volumes
    - Persistent storage volumes for your data using Amazon Elastic Block Store (Amazon EBS)
- AMI
    - Preconfigured templates for an EC2 instance
    - Virtual Machine you want to run
    - Types of virtualization:
        - PV:  Paravirtual
        - HVM:  Hardware Virtual Machine,  RECOMMENDED!
    - Backing type for root device:
        - Amazon EBS
            - Can be stopped and you you will not loose the data on the instance
        - Instance Store
            - aka:  ephemeral storage
            - Cannot be stopped
            - If host fails you will loose all your data
    - When you copy an AMI from one region to another launch permissions, user-defined tags, or AWS S3 bucket permissions are not copied from the source
- EBS Root Volumes of your DEFAULT AMIs cannot be encrypted
    - You can use a third party tool (ex:  bit locker) to encrypt the root volume, or it can be done when creating AMIs
    - Root device volume is volume in which OS system is installed
- On an EBS-backed instance, the default action is for the root EBS volume to be deleted when an instance is terminated
- By default, the public IP address of an EC2 Instance is released after the instance is stopped and started.
- For all new AWS accounts, there is a soft limit of 20 EC2 instances per region.
- Underlying Hypervisors:
    - Xen
    - Nitro
- AWS will use commercially reasonable efforts to make Amazon EC2 and Amazon EBS each available with a Monthly Uptime Percentage (defined below) of at least 99.95%

## Security Groups
    - All inbound traffic is blocked by default
    - All outbound traffic is allowed
    - Stateful, therefore inbound traffic automatically allows outbound traffic on same port as well
    - Changes to security groups take effect immediately
    - You can have any number of EC2 instances within a security group
    - You can have multiple security groups attached to EC2 instances
    - You cannot block specific IP addresses using Security Groups, instead use Network Access Control Lists (NACLs)
    - You can specify allow rules but not deny rules
    - Source:
        - Can be IP range or name of another security group (can include a security group in another account)
    - Can only exist in one region

## EBS Volumes
    - EBS:  Elastic Block Store
    - Provides persistent block storage volumes for use with EC2 instances
    - Each EBS volume is automatically replicated within its AZ to protect from component failure
    - 5 different types:
        - General Purpose SSD
            - Volume size: 1GB-16TB
            - Max IOPS:  16,000
            - Usage:  Most Workloads
            - gp2
            - max throughput:  250 MB/s
        - Provisioned IOPS SSD
            - Volume size: 4GB-16TB
            - Max IOPS:  64,000
            - Usage:  For mission-critical apps.  Ex:  Databases
            - io1
            - max throughput:  1000 MB/s
        - Throughput Optimized HDD
            - Volume size: 500GB-16TB
            - Max IOPS:  500
            - Usage:  Big Data
            - st1
        - Cold HDD
            - Lowest cost
            - Volume size: 500GB-16TB
            - Max IOPS:  250
            - Usage:  File Servers
            - sc1
        - EBS Magnetic
            - Previous Generation HDD
            - 1GB-1TB
            - Max IOPS:  40-200
            - Standard
    - Volumes are in same AZ as the instance using them
        - You want hard disk drive to be as close to motherboard as possible
    - One of the known fallacies of EBS is that all the data of a single volume lives in a single Availability Zone. Thus it cannot withstand Availability zone failures.
    - Snapshots exist on S3
        - Point in time copies of Volumes
        - They're incremental, only deltas between blocks are used
    - AMIs can be created from both volumes and snapshots
    - To create snapshot of root EBS volume, you should stop the instance before
    - You can take a snapshot while the instance is running
    - You can change EBS sizes and storage type on the fly
    - Migration from one AZ to another:
        1. Take a snapshot
        2. Create AMI from snapshot
        3. Use the AMI to launch EC2 instance in a new AZ 
    - Move EC2 volume from one region to another:
        1. Take a snapshot
        2. Create AMI from snapshot
        3. Copy AMI from one region to another
        4. Use copied AMI to new region in order to launch new EC2 instance in new region
    - Root Device EBS Volumes can be encrypted during EC2 instance creation (new feature)
    - The root device EBS volume can be encrypted afterwards also
        - Steps:  
            1. Create snapshot of EBS root device volume (Still unencrypted)
            2.  Copy the snapshot and select Encrypt option
            3.  Create AMI image from encrypted snapshot
            4.  You can now launch a new EC2 isntance from the encrypted AMI
    - Snapshots of encrypted volumes are encrypted automatically
    - Volumes restored from encrypted snapshots are encrypted automatically
    - You can share snapshots, but only if they are unencrypted
        - Snapshots can be shared with other AWS accounts or made public
    - An EBS volume can be detached from an instance without stopping it as long as it is not the root volume
    - You can add multiple volumes to an EC2 instance and then create your own RAID 5/RAID 10/RAID 0
        - You can use any of the standard RAID configurations that you can use with a traditional bare metal server, as long as that particular RAID configuration is supported by the operating system for your instance.
        - This is because all RAID is accomplished at the software level. 

## Cloudwatch
- Monitoring service to monitor AWS resources as well as applications that you run on AWS
- Used for monitoring performance
- Can monitor things like:
    - Compute
        - EC2 instances
        - Autoscaling Groups
        - Elastic Load Balancers
        - Route53 Health Checks
    - Storage & Content Delivery
        - EBS Volumes
        - Storage Gateways
        - Cloudfront
- Alarms:  You can create cloudwatch alarms which trigger notifications when particular thresholds are hit
- Events:  Cloudwatch events help you response to state changes in your AWS resources
- Logs:  Cloudwatch logs help you to aggregate, monitor, and store logs 
- Cloudwatch & EC2
    - Host level metrics:
        - CPU
        - Network
        - Disk
        - Status Check
    - Everything else is a custom metric. i.e. Memory is a custom CloudWatch metric.
    - Types of Monitoring:
        1. Standard:  Will monitor events every 5 minutes by default
        2.  Detailed:  Detailed monitoring turned on will give you 1 minute intervals
- Supports the following alarm states:
    1. OK - the metric is within the threshold
    2. ALARM - The metric is outside the threshold
    3. INSUFFICIENT_DATA - The alarm has just started, but the metric is not available, or not enogh data is available for the metric to determine the alarm state


## CloudTrail
- Increases visibility into your user and resource activity by recording AWS Management Console actions and API calls
- You can identify which users and accounts called AWS, the source IP address from which the calls were made, and when the calls occurred

## IAM roles
- Are more secure than storing your access key and secret access key on individual EC2 instances
- Easier to manage
- Roles can be assigned to an EC2 instance after it is created
- They are universal (can be used in any region)

## Bootstrap Scripts on EC2 instances
- shebang and path to interpreter in order to run commands:  `#!/bin/bash`

## EC2 instances metadata
- from within EC2 instance run:  curl http://169.254.169.254/latest/user-data
    - Will contain the bootstrap script
- 169.254.169.254 is a link-local address and is valid only from the instance
- Used to get information about an instance from the instance itself
- curl http://169.254.169.254/latest/meta-data/
- Can get the instance IP address via: http://169.254.169.254/latest/meta-data/public/ipv4/

## EFS
- Similar to EBS except that in EBS you can only mount your virtual disk to one EC2 instance
- Elastic File System
- No need to pre-provision storage like in EBS, it can just keep growing
- Great for file servers
- Great to share files between EC2 instances
- Throughput mode:
    - Bursting
        - Throughput on Amazon EFS scales as the size of your file system in the standard storage class grows.
    - Provisioned
        - You can instantly provision the throughput of your file system (in MiB/s) independent of the amount of data stored.
- Performance Mode:
    - General Purpose
    - Max I/O
- Supports the Network File System version 4 (NFSv4)
- Only pay for the storage you use
- Can support thousands of concurrent NFS connections
- Data is stored across multiple AZ's within a region
- Read After write consistency
- You can now connect to Amazon EFS file systems from EC2 instances in other AWS regions using an inter-region VPC peering connection, and from on-premises servers using an AWS VPN connection.
- Use cases:
    - Big Data and Analytics
    - Media Processing Workflows
        - Ex:  Video editing, rendering, sound design
    - Content Management and Web Serving
    - Home Directories

## EC2 Placement Groups
- When you launch a new EC2 instance, the EC2 service attempts to place the instance in such a way that all of your instances are spread out across underlying hardware to minimize correlated failures. You can use placement groups to influence the placement of a group of interdependent instances to meet the needs of your workload.
- Cannot span multiple AZs
- Name must be unique per AWS account
- Only certain types of instances can be launched in a placement group
- Placement groups cannot be merged
- There is no charge for creating a placement group
- You can't move an existing instance into a placement group
    - you can create an AMI from existing instance and launch a new instance from AMI into a placement group
- 3 types of placement groups:
    1. Clustered Placement Group
        - Useful for Low network latency/High Network Throughput
        - Can't span multiple AZs
        - Recommended for homogenous instances (same instance types)
    2. Spread Placement Group
        - A spread placement group is a group of instances that are each placed on distinct racks
        - Individual Critical EC2 instances on separate hardware (separate racks)
            - each rack having its own network and power source.
        - Can be in same or different AZs
        - You can have a maximum of seven running instances per Availability Zone per group.
    3.  Partition
        - Multiple EC2 instances within one partition
        - Logical segments called partitions
        - No two partitions share the same racks
        - Help reduce the likelihood of correlated hardware failures for your application.
        - The max number of partitions is 7
# AWS Command Line

## Whizlabs Exam Notes
- EBS-Optimized Instances?
## EBS Test
- The number and size of available instance store volumes for you instance varies by instance type.  Some instance types do not support instance store volumes.  
- You can specify instance store volumes for an instance only when you launch it.  You can't detach an instance store volume from one instance and attach it to a different instance. 
- You can use Amazon Data Lifecycle Manager (Amazon DLM) to automate the creation, retention, and deletion of snapshots taken to back up your Amazon EBS volumes.
- You are working for a data management company which uses AWS platform to manage the data for various customers. They are using AWS EBS backed EC2 instance with “Delete EBS volume on termination” checked. EC2 instances are used to run data  streaming  application  which generates logs and are stored on EBS volumes. The log files are critical for auditing purposes. How would you protect the data stored on EBS volumes from accidental terminations of EC2 instances?
    - Setup a Data LifeCycle Manager policy scheduler to create EBS snapshots for your EBS volumes.  When EC2 instance is terminated, it automatically creates a snapshot of EBS volume and then deletes the EBS volume.
- While performing a copy of an encrypted snapshot we cannot disable encryption
- You are working as an AWS architect in your organization. An application is being developed on AWS EC2 instance and would need a local volume with low latency to handle database workloads. They figured out Provisioned IOPS SSD volume type suits best. However, when the application team is launching an EC2 instance, they found an option named “EBS-optimized”. They reached out to you asking the purpose of EBS optimized instances. What do you suggest?
    - Amazon EBS–optimized instance provides additional, dedicated capacity for Amazon EBS I/O.
    - An Amazon EBS–optimized instance uses an optimized configuration stack and provides additional, dedicated capacity for Amazon EBS I/O. This optimization provides the best performance for your EBS volumes by minimizing contention between Amazon EBS I/O and other traffic from your instance.
    - Not all instance types support EBS Optimization.
- You can configure your AWS account to enforce the encryption of your EBS volumes and snapshots.
    - Encryption by default is a Region-specific setting.
- When you create an encrypted EBS volume and attach it to a supported instance type, the following types of data are encrypted:
    - Data at rest inside the volume
    - All data moving between the volume and the instance
    - All snapshots created from the volume
    - All volumes created from those snapshots
- Encrypted EBS Volumes ARE NOT SUPPORTED in all instance types
- Available Cloudwatch metrics in EBS:
    - VolumeReadBytes
    - VolumeWriteBytes
    - VolumeReadOps
    - VolumeWriteOps
    - VolumeTotalReadTime
    - VolumeTotalWriteTime
    - VolumeIdleTime
    - VolumeQueueLength
    - VolumeThroughputPercentage
    - VolumeConsumedReadWriteOps
    - BurstBalance
- You can modify the volume and increase the EBS volume size as long as supported by instance type.

## EFS Test
- Encryption at rest option can only be set during EFS creation. You need to create encryption-at-rest EFS, copy data from old EFS to new EFS and delete old EFS.
- AWS uses NFS protocol for EFS. NFS is not an encrypted protocol and anyone on the same physical network could sniff the traffic and reassemble the information being passed back and forth.  However, AWS provides an option to encrypt data at transit through NFS to EFS.
- We recommend General Purpose performance mode for most file systems. Max I/O performance mode is optimized for applications where tens, hundreds, or thousands of EC2 instances are accessing the file system — it scales to higher levels of aggregate throughput and operations per second with a tradeoff of slightly higher latencies for file operations.
- Provisioned Throughput mode is available for applications with high throughput to storage (MiB/s per TiB) ratios, or with requirements greater than those allowed by the Bursting Throughput mode. For example, say you're using Amazon EFS for development tools, web serving, or content management applications where the amount of data in your file system is low relative to throughput demands. Your file system can now get the high levels of throughput your applications require without having to pad your file system.
- File systems in the Max I/O mode can scale to higher levels of aggregate throughput and operations per second with a tradeoff of slightly higher latencies for file operations. Highly parallelized applications and workloads, such as big data analysis, media processing, and genomics analysis, can benefit from this mode


## Random Questions and answers
- How is the Public IP address managed in an instance session via the instance GUI/RDP or Terminal/SSH session?
    - The Public IP address is not managed on the instance; it is instead, an alias applied as a network address translation of the private address.