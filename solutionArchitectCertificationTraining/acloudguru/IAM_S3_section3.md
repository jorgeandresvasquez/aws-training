## IAM
- You can use AWS IAM to securely control individual and group access to your AWS resources.
- Allows you to manage users and their level of access to the AWS Console
- New users have NO permissions when first created
- Programatically access AWS system
    - Access KeyID
    - Secret Access Keys
- Always setup Multifactor Authentication on your root account
- IAM is universal
    - Does not apply to regions
- Features:
    - Centralized control of your AWS account
    - Shared Access to your AWS account
    - Granular Permissions
    - Identity Federations
    - Multifactor Authentication
    - Provide temporary access for users/devices and services when necessary
    - Allows to setup your own pwd rotation policy
    - Integrates with many AWS services
    - Supports PCI DSS Compliance 
- Key Terminology:
    - Users
        - End users 
    - Groups
        - Collection of Users
    - Policies
        - JSON-formatted documents
    - Roles 
        - You assign them to AWS resources    

## S3
- Simple Storage Service
- Object Storage
- Files from 0 bytes-5TB
- The largest object that can be uploaded in a single PUT is 5 gigabytes
- For objects larger than 100 megabytes, customers should consider using the Multipart Upload capability.
- Unlimited Storage
- Files are stored in buckets
    - buckets must be universally unique
- http 200 code when upload is successful
- S3 objects consist of the following:
    - key is name of object
    - value is data of object
    - VersionId is important when versioning
    - Metadata
    - ACLs
    - Torrents
- Data consistency model for S3
    - Read after write for PUTS of new objects
    - Eventual Consistency for overwrite PUTS and DELETES
        - If you update a file or delete it you might get the older version immediately after
- Guarantees:
    - Built for 99.99% availability
    - Amazon guarantees:  99.9% availability
    - 99.999999999% durability (11 x 9's)
        - All Storage classes offer this durability
    - Amazon S3 Standard, S3 Standard-IA, and S3 Glacier are all designed to sustain data in the event of an entire S3 Availability Zone loss.
- Amazon S3 Standard, S3 Standard-Infrequent Access, and S3 Glacier storage classes replicate data across a minimum of three AZs to protect against the loss of one entire AZ.
- Features:
    - Tiered Storage
        - Storage Classes:
            - S3 Standard:
                - 99.99% availability
                - 99.999999999% durability
                - Stored redundantly across multiple devices in multiple facilities
                - Designed to sustain the loss of 2 facilities concurrently
            - S3 - Infrequent Access (IA):
                - 99.9% availability
                - Data that is accessed less frequently but requires rapid access when needed
                - Lower fee than Standard but you are charged a retrieval fee
            - S3 One Zone (IA)
                - 99.5% availability
                - lower cost option for infrequently accessed data but without multiple AZ resilience
                - Used to be known as RRS or "Reduced Redundancy Storage"
            - S3 Intelligent Tiering
                - Automatically moves data to the most cost-effective access tier, without performance impact or operational overhead
                    - Based on ML and how often you use your objects
            - Glacier
                - S3 Glacier
                    - Data archiving 
                    - Retrieval times configurable from minutes to hours
                    - Expedited retrievals allow you to quickly access your data when occasional urgent requests for a subset of archives are required
                - S3 Glacier Deep Archive
                    - Lowest storage class when a retrieval time of 12 hours is acceptable
        - Storage classes are per object and not global by bucket
    - Lifecycle Management
    - Versioning
    - Encryption
        - SSE-S3 (Server-Side Encryption where Amazon manages the keys)
        - SSE-C (Server-Side Encryption where customer manages the keys)
        - SSE-KMS
        - Using Client library 
    - MFA delete
    - Secure your data using ACLs and Bucket Policies
        - You can limit access to your bucket from a specific Amazon VPC Endpoint or a set of endpoints using Amazon S3 bucket policies.
    - Amazon Macie
        - Used to protect against security threats by continuously monitoring your data and account credentials. 
- Charges $
    - There is no Data Transfer charge for data transferred within an Amazon S3 Region via a COPY request
    - There is no Data Transfer charge for data transferred between Amazon EC2 and Amazon S3 within the same region,
    - Storage
        - Storage usage is measured in “TimedStorage-ByteHrs,” which are added up at the end of the month to generate your monthly charges.
    - Requests
        - Per individual PUT, COPY, GET, POST, LIST request independent of size
        - DELETE requests are free
        - Data Scanned by S3 SELECT
    - Storage Management Pricing
    - Data Transfer Pricing
        - Network Data Transferred In
        - Network Data Transferred Out
    - Transfer Acceleration
    - Cross Region Replication Pricing
    - Option to configure as a Requester pays bucket
- Transfer Acceleration
    - Users upload their files to Edge locations instead of S3 bucket itself
    - For your bucket to work with transfer acceleration, the bucket name must conform to DNS naming requirements and must not contain periods (".").
    - AWS has expanded its HIPAA compliance program to include Amazon S3 Transfer Acceleration as a HIPAA eligible service.
- Amazon S3 creates bucket in a region you specify.
- Bucket URLs
    - Amazon S3 supports both virtual-hosted-style and path-style URLs to access a bucket:
        - Virtual-hosted style:
            - https://bucket.s3.aws-region.amazonaws.com
            OR 
            - https://bucket.s3.amazonaws.com
            - Buckets created in regions launched after March 20, 2019 are not reachable via:  https://bucket.s3.amazonaws.com
        - Path-style:
            - https://s3.aws-region.amazonaws.com/bucket
        - Buckets created after September 30, 2020, will support only virtual hosted-style requests.
        - When using virtual hosted–style buckets with SSL, the SSL wild-card certificate only matches buckets that do not contain periods. To work around this, use HTTP or write your own certificate verification logic.
- An object can be accessed using http or https
- CORS
    - Cross-origin resource sharing (CORS) defines a way for client web applications that are loaded in one domain to interact with resources in a different domain.
    - With CORS support, you can build rich client-side web applications with Amazon S3 and selectively allow cross-origin access to your Amazon S3 resources.
- Headers
    - x-amz-storage-class
- Prefix naming is required for optimal performance if we expect a higher number of objects.  Amazon S3 maintains an index of object key names in each AWS region.  Object keys are stored in UTF-8 binary ordering across multiple partitions in th index.  The key name determines which partition the key is stored in.  Although Amazon S3 automatically scales to high request rates, using a sequential prefix, such as timestamp or an alphabetical sequence, increases the likelihood that Amazon S3 will target a specific partition for a large number of your keys, potentially overwhelming the I/O capacity of the partition.  When your workload is a mix of request types, introduce some randomness to key names by adding a hash string as a prefix to the key name.  By introducing randomness to your key names the I/O load will be distributed across multiple index partitions.  (Ex:  Compute the MD5 hash of the character sequence that you plan to assign as the key and add 3 or 4 characters from the hash as a prefix to the key name)
    - This S3 request rate performance increase removes any previous guidance to randomize object prefixes to achieve faster performance. That means you can now use logical or sequential naming patterns in S3 object naming without any performance implications.
- System-defined metadata
    - Modifiable by User
        - x-amz-server-side-encryption
        - x-amz-storage-class
        - x-amz-website-redirect-location
        - x-amz-server-side-encryption-aws-kms-key-id
        - x-amz-server-side-encryption-customer-algorithm	
    - Not Modifiable by User
        - Date
        - Content-Length
        - Last-Modified
        - Content-MD5
        - x-amz-version-id
        - x-amz-delete-marker
- User-defined metadata
    - key/value pairs
    - Stored in lower case
    - REST:
        - Preffix:  "x-amz-meta-"  
- You cannot tag an inner folder
- Cross-Region Replication (CRR)
    - Enables automatic, asynchronous copying of objects across buckets in different AWS Regions. Buckets configured for cross-region replication can be owned by the same AWS account or by different accounts.
    - Requirements:
        - Versioning must be enabled on both the source and the destination buckets
        - Regions must be unique
        - Files in an existing bucket are not replicated automatically
        - All subsequent updated files will be replicated automatically
        - Delete markers are not replicated
        - Deleting individual versions or delete markers will not be replicated
- You can securely upload/download your data to/from Amazon S3 via SSL or HTTP endpoints using HTTPS.

## Bucket-level properties
- Versioning
- Server-access logging
 - Each access log record provides details about a single access request, such as the requester, bucket name, request time, request action, response status, and an error code, if relevant.
- Static website hosting
- Object-level logging
    - Data-plane operations

## Object-level properties
- Storage class
- Metadata

## Default options when creating S3 bucket:
- Encryption is not Enabled
- Bucket policy is not created
- Versioning is not enabled
- Transfer Acceleration is not enabled

## Cloudfront
- It is a CDN
- Key Terminology:
    - Edge Location:  Location where content will ge cached, separate to region/AZ
    - Origin:  Origin of all the files the CDN will distribute.  Ex:  S3 bucket, Ec2 instance, ELB, Route53
    - Distribution:  Name of CDN (Collection of Edge Locations)
        - 2 types of distributions:
            - Web Distribution (ex: websites)
            - RTMP (for Adobe Media Streaming)
    - Objects are cached for the TTL
    - Edge locations are read and write
    - Cached objects can be cleared but cost $
- The more requests that CloudFront is able to serve from edge caches as a proportion of all requests (that is, the greater the cache hit ratio), the fewer viewer requests that CloudFront needs to forward to your origin to get the latest version or a unique version of an object.
- Restricting Access to Amazon S3 Content by Using an Origin Access Identity (OAI)
    - To restrict access to content that you serve from Amazon S3 buckets, you create CloudFront signed URLs or signed cookies to limit access to files in your Amazon S3 bucket, and then you create a special CloudFront user called an origin access identity (OAI) and associate it with your distribution.
    - In general, if you're using an Amazon S3 bucket as the origin for a CloudFront distribution, you can either allow everyone to have access to the files there, or you can restrict access. If you limit access by using, for example, CloudFront signed URLs or signed cookies.
    


## Snowball
- BIG BIG disks
    - petabyte scale
- Can be 1/5 the $ of high-speed internet
- Snowball Edge
    - 100 TB
    - onboard storage or compute capability
    - Like having a portable version of AWS
- Typical sizes of 50TB or 80TB
- Snowmobile
    - Exabyte Scale
    - 100 PB per snowmobile
    - 45 ft long shipping container
- Can export/import to/from S3

## Storage Gateway
- Hybrid storage solution
- Service that connects an on-premises software appliance with cloud-based storage to provide seamless and secure integration between an organization's on-premises IT environment and AWS's storage infrastructure.
- Can be a physical or virtual device
    - Software Appliance as a VM image to install on datacenter host
    - Supports either VMware ESXi or Microsoft Hyper-V
- Comes in the following 3 flavors:
    1.  File Gateway (NFS & SMB)
        - Files are stored as objects in your S3 buckets, accessed through a Network File System (NFS) mount point
        - Ownership, permissions, and timestamps are durably stored in S3 in the user-metadata of the object associated with the file.
    2.  Volume Gateway (iSCSI)
        - Presents your applications with disk volumes using the iSCSI block protocol
            - Data written to these volumes can be asynchronously backed up as point-in-time snapshots of your volumes, and stored in the cloud as Amazon EBS snapshots
        - Stored Volumes
            - Allows to store primary data locally while asynchronously backing up that data to AWS
            - Data is asynchronously backed up to S3 in the form of Amazon EBS snapshots
            - 1 GB - 16 TB in size
        - Cached Volumes
            - Let you use S3 as your primary data storage while retaining frequently accessed data locally in your storage gateway
            - 1 GB - 32 TB in size
            - By using cached volumes, you can use Amazon S3 as your primary data storage, while retaining frequently accessed data locally in your storage gateway. Cached volumes minimize the need to scale your on-premises storage infrastructure, while still providing your applications with low-latency access to their frequently accessed data. You can create storage volumes up to 32 TiB in size and attach to them as iSCSI devices from your on-premises application servers. Your gateway stores data that you write to these volumes in Amazon S3 and retains recently read data in your on-premises storage gateway's cache and upload buffer storage
    3.  Tape Gateway (VTL)
        - Durable, cost-effective solution to archive data in AWS
        - Lets you leverage your existing tape-based backup application infrastructure to store data on virtual tape cartridges that you create on your tape gateway
        - Supported by popular backup software like:  NetBackup, Backup exec, Veeam, etc.


## NOTES from official FAQs

# TODOs:
    - Read on presigned URLs
        - https://docs.aws.amazon.com/AmazonS3/latest/dev/ShareObjectPreSignedURL.html
        - expiration
    - prefixes on S3
        - https://stackoverflow.com/questions/52443839/s3-what-exactly-is-a-prefix-and-what-ratelimits-apply
        - You can use them to parallelize reads
    - Read on performance optimization on S3 buckets:
        - https://aws.amazon.com/about-aws/whats-new/2018/07/amazon-s3-announces-increased-request-rate-performance/
    - system metadata on S3 (versionId, )
    - Bucket URL names
    - Read official AWS S3 docs
    - Read FAQs
    - Go through bucket console options and understand everything
    - Multipart uploads
        - https://docs.aws.amazon.com/AmazonS3/latest/dev/mpuoverview.html
    - IAM roles and S3:
        - https://aws.amazon.com/blogs/security/how-to-restrict-amazon-s3-bucket-access-to-a-specific-iam-role/
    
