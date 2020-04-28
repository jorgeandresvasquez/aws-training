## Concepts
- ![S3 Outage on February 2017 Downtime](images/s3_outage_feb_2017_minutes.png)
- "Everything fails all the time" (Werner Vogels, Amazon CTO)
- We need to architect for everything to fail
- High Availability (HA)
    - Designing in `redundancies` to reduce the chance of impacting service levels
- Fault Tolerance
    - Designing in the ability to `absorb problems` without impacting service levels
    - Ability to tolerate faults
        - Can be very expensive and sometimes cost prohibitive
- Service Level Agreement (SLA)
    - An agreed `goal or target` for a given service on its performance or availability
- Recovery Time Objective (RTO)
    - Time that it takes after a disruption to restore business processes to their service levels
    - `T` is for Time
- Recovery Point Objective (RPO)
    - Acceptable amount of data loss measured in time
    - `P`is for the data that gets "poof"
- ![RTO and RPO](images/rto_rpo.png)
- ![Business Continuity Plan, HA, DR, RTO and RPO](images/business_continuity_plan_ha_hr_rpo_rto.png)
- ![Disaster Categories](images/disaster_categories.png)
    - Human error is always another category

## Disaster Recovery Architectures
- ![4 main disaster recovery architectures compared by time and cost](images/disaster_recovery_architectures.png)
- ![Backup and Restore DR Architecture](images/backup_and_restore_dr.png)
- ![Pilot Light DR Architecture](images/pilot_light_dr.png)
- ![Warm Standby DR Architecture](images/warm_standby_dr.png)
- ![Multi Site DR Architecture](images/multi_site_dr.png)
    - Strictly speaking DNS records do have a TTL so once health checks trigger a change in route53 they might be some downtime

## Storage HA Options
- EBS Volumes
    - Annual Failure rate less than 0.2% compared to commodity hard drive at 4% (Given 1000 EBS volumes, expect around 2 to fail per year)
    - Availability target of 99.999%
    - Replicated automatically `within a single AZ`
        - `Vulnerable to AZ failure.  Plan accordingly`
    - Easy to snapshot, which is stored on S3 and multi-AZ durable
    - You can copy snapshots to other regions as well
    - Supports RAID (`R`edundant `A`rray of `I`ndependent `D`isks) configurations
        - RAID-0 
            - is not fault tolerant but it is very fast
        - RAID-5 
            - you need 3 or more disks
            - Most common setup used
            - Parity is distributed amongst all disks
        - AWS recommends RAID-0 and RAID-1 with their EBS volumes because EBS volumes are accessed over a network and writing those parity bits sucks a lot of IO
        - ![RAID Configuration Comparison](images/raid_comparison.png)
    - ![How RAID levels impact throughput and IOPS](images/raid_iops_throughput.png)
- S3
    - Standard Storage Class (99.99% availability = 52.6 minutes/year)
    - Standard Infrequent Access (99.9% availability = 8.76 hours/year)
    - One-zone Infrequent Access (99.5% availability = 1.83 days/year)
    - Eleven 9's of durability (99.999999999 %)
        - All the Storage classes have this same durability, except that one-zone IA only has 1 zone durability
    - Standard & Standard-IA have multi-AZ durability; one-zone only as single AZ durability
    - Backing service for EBS snapshots and many other AWS services
- Amazon EFS
    - Implementation of the NFS file system
    - True file system as opposed to block storage (EBS) of object storage (S3)
    - File locking, strong consistency, concurrently accessible
    - Each file object and metadata is stored across multiple AZs
    - Can be accessed from all AZs concurrently
    - Mount targets are highly available
    - ![EFS HA example](images/efs_ha_example.png)
- Other Options
    - Amazon Storage Gateway
        - Good way to migrate on-premise data to AWS for offsite backup
        - Best for continuous sync needs
    - Snowball
        - Various options for migrating data to AWS based on volume
        - Only for batch transfers of data
    - Glacier
        - Safe offsite archive storage
        - Long-term storage with rare retrieval needs
    - ![Other Storage Opeions Usage Examples](images/other_storage_usage_examples.png)

## Compute Options
- HA Approaches for Compute
    - Up-to-date AMIs are critical for rapid fail-over
    - AMIs can be copied to other regions for safety or DR staging
    - Horizontally scalable architectures are preferred because risk can be spread across multiple smaller machines versus one large machine
    - Reserved instances is the only way to guarantee that resources will be available when needed
    - Auto Scaling and Elastic Load Balancing work together to provide automated recovery by maintaining minimum instances
    - Route53 Health Checks also provide "self-healing" redirection of traffic
 - ![Route 53 Health Checks Example](images/route53_health_checks_example.png)  

## Database HA Options
- HA Approaches for Databases
    - If possible, choose DynamoDB over RDS because of inherent fault tolerance
    - If DynamoDB can't be used, choose Aurora because of redundancy and automatic recovery features
    - If Aurora can't be used, choose multi-AZ RDS
    - Frequent RDS snapshots can protect against data corruption or failure - and they won't impact performance of multi-AZ deployment.


## Pro Tips

## Sample Questions Notes

## Other resources



