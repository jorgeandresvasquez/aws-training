# Section Test Notes from Whizlabs

## Kinesis
- Streaming data is data that is generated continuosly by thousands of data sources, which typically send in the data records simultaneously and in small sizes (order of kilobytes)
    - Examples of streaming data:
        - Purchases from online stores
        - Stock prices
        - Game data (as the gamer plays)
        - Social network data
        - Geospatial Data (ex:  uber)
        - iOT sensor data
- Kinesis is a platform on AWS to send your streaming data to, making it easy to load and analyze streaming data, and also providing the ability for you to build your own custom applications for your business needs.
- 3 different types of kinesis:
    1. Kinesis Streams
        - Data Producers stream data to kinesis
        - Kinesis streams is a place to put all that data, default retention is 24 hours but can go up to 7 days
        - Data is contained in shards
        - Data consumers (EC2 instances) can go an analyze that data in shards
        - After data has been processed it can be stored in different places:    DynamoDB, S3, EMR, Redshift
        - Shards:
            - 5 transactions/sec for reads and up to a max total data read of 2MB/sec
            - Up to 1000 records/sec for writes and up to max total data write rate of 1MB/sec
            - Total capacity of the stream is the sum of the capacities of its shards
    2. Kinesis Firehose
        - No persistent storage
        - You need to do something with that data as soon as it comes in, normally using lambda functions and outputting somewhere safe (S3-->Redshift, or ElasticSearch Cluster)
    3. Kinesis Analytics
        - Works with Kinesis Streams and Kinesis Firehose
        - It can analyze the data on the fly
        - Can store data either on S3, Redshift or Elastic Search Cluster
- Server-side encryption is a feature in Amazon Kinesis Data Streams that automatically encrypts data before it's at rest by using an AWS KMS customer master key (CMK) you specify. Data is encrypted before it's written to the Kinesis stream storage layer, and decrypted after it’s retrieved from storage. As a result, your data is encrypted at rest within the Kinesis Data Streams service. This allows you to meet strict regulatory requirements and enhance the security of your data.
- The metrics that you configure for your streams are automatically collected and pushed to CloudWatch every minute. Metrics are archived for two weeks; after that period, the data is discarded.
- Monitoring types for Kinesis data streams:
    1. Basic (stream-level)
        - Stream-level data is sent automatically every minute at no charge.
    2. Enhanced (shard-level)
        - Shard-level data is sent every minute for an additional cost. To get this level of data, you must specifically enable it for the stream using the EnableEnhancedMonitoring operation.
- ![Kinesis Data Streams High-Level Architecture](images/Kinesis_Data_Streams_Architecture.png)
- https://docs.aws.amazon.com/streams/latest/dev/server-side-encryption.html
- AWS KMS throttles API requests at different limits depending on the API operation. Throttling means that AWS KMS rejects an otherwise valid request because the request exceeds the limit for the number of requests per second. When a request is throttled, AWS KMS returns a ThrottlingException error
- If you are developing an application using the Kinesis Client Library (KCL), your policy must include permissions for Amazon DynamoDB and Amazon CloudWatch; the KCL uses DynamoDB to track state information for the application, and CloudWatch to send KCL metrics to CloudWatch on your behalf
- You’re developing an application that is going to make use of Kinesis Data Streams. The data streams are going to be processed by Lambda functions. Which of the following is required to ensure that Kinesis can work with Lambda?
    - AWS service role of the type AWS Lambda – This role grants AWS Lambda permissions to assume the role.
    - AWSLambdaKinesisExecutionRole – This is the access permissions policy that you attach to the role.
- Your team is developing an application which will make use of Kinesis streams. Which of the following permissions need to be given to the producers for the streams?
    - DescribeStream
    - PutRecord

## KMS
- Deleting a customer master key (CMK) in AWS Key Management Service (AWS KMS) is destructive and potentially dangerous. It deletes the key material and all metadata associated with the CMK, and is irreversible. After a CMK is deleted you can no longer decrypt the data that was encrypted under that CMK, which means that data becomes unrecoverable. You should delete a CMK only when you are sure that you don't need to use it anymore. If you are not sure, consider disabling the CMK instead of deleting it. You can re-enable a disabled CMK if you need to use it again later, but you cannot recover a deleted CMK.

- AWS KMS is integrated with AWS CloudTrail, so all AWS KMS API activity is recorded in CloudTrail log files. If you have CloudTrail turned on in the region where your customer master key (CMK) is located, you can examine your CloudTrail log files to view a history of all AWS KMS API activity for a particular CMK, and thus its usage history. 

- You are the Security Administrator for your company. You have to set the Key Policies for a set of users for a set of CMK keys. These users are developers who need to have the ability to encrypt and decrypt data using CMK keys. Which of the following is the minimum set of permissions that should be applied in the Key policies?
    - Allow actions on kms:Encrypt, kms:Decrypt, kms:ReEncrypt, kms:GenerateDataKey

- A company is planning on using the AWS KMS service for encryption and decryption of their underlying data. The company needs to ensure that they use their own key material to prove ownership. Which of the following steps must be done in order to fulfill this requirement?
    - You can use kms:KeyOrigin condition key in key policy to control access to the CreateKey operation based on the value of the Origin parameter in the request.
    - The following example policy statement uses the kms:KeyOrigin condition key to allow a user to create a CMK only when the key origin is EXTERNAL, that is, the key material is imported.
    ```
    {
    "Effect": "Allow",
    "Principal": {
        "AWS": "arn:aws:iam::111122223333:user/ExampleUser"
    },
    "Action": "kms:CreateKey",
    "Resource": "*",
    "Condition": {
        "StringEquals": {
        "kms:KeyOrigin": "EXTERNAL"
        }
    }
    }
    ```

- With encryption at rest, DynamoDB transparently encrypts all customer data in an encrypted table, including its primary key and local and global secondary indexes, whenever the table is persisted to disk. (If your table has a sort key, some of the sort keys that mark range boundaries are stored in plaintext in the table metadata.) When you access an encrypted table, DynamoDB decrypts the table data transparently. You do not need to change your applications to use or manage encrypted tables.
    - Encryption at rest uses AWS KMS customer master keys in your AWS to protect your DynamoDB tables on disk. This integration also enables you to audit access to your DynamoDB tables by examining the DynamoDB API calls to AWS KMS in audit logs and trails

- When you enable `annual rotation of a CMK's key material` AWS KMS creates new key material for the CMK each year and sends a corresponding event to Cloudwatch events.  

- Your company is making extensive use of the AWS KMS service. They have defined a number of CMK keys. They want to be notified if any request was made with the root user to create a key in the KMS service. How could you achieve this? Choose 2 answers from the options given below.
    - Use Cloudwatch rule to detects any AWS account root user API event
    - Use the SNS service to send notifications

- You can connect directly to AWS KMS through a private endpoint in your VPC instead of connecting over the internet. When you use a VPC endpoint, communication between your VPC and AWS KMS is conducted entirely within the AWS network.

- AWS KMS supports Amazon Virtual Private Cloud (Amazon VPC) interface endpoints that are powered by AWS PrivateLink. Each VPC endpoint is represented by one or more Elastic Network Interfaces (ENIs) with private IP addresses in your VPC subnets.

- Each CMK can have multiple aliases, but each alias points to only one CMK. The alias name must be unique in the AWS account and region. To simplify code that runs in multiple regions, you can use the same alias name but point it to a different CMK in each region.

- You can use AWS KMS API operations to list, create, and delete aliases. You can also update an alias, which associates an existing alias with a different CMK. There is no operation to edit or change an alias name. If you create an alias for a CMK that already has an alias, the operation creates another alias for the same CMK. To change an alias name, delete the current alias and then create a new alias for the CMK.

## WAF
- A company is hosting a web application which is sitting behind an Application Load Balancer. There is a plan to use the WAF service to protect the application against SQL injection and other types of web layer attacks. You need to initially get detailed information on the requests that get logged in the WAF service. Which of the following would you need to have in place for this requirement to be fulfilled?
    - Have an S3 bucket in place for storage of the logs
    - Use the AWS Kinesis Firehose service
- You can enable logging to get detailed information about traffic that is analyzed by your web ACL. Information that is contained in the logs include the time that AWS WAF received the request from your AWS resource, detailed information about the request, and the action for the rule that each request matched.
- To get started, you set up an Amazon Kinesis Data Firehose. As part of that process, you choose a destination for storing your logs. Next, you choose the web ACL that you want to enable logging for. After you enable logging, AWS WAF delivers logs through the firehose to your storage destination.
- A company is hosting a web application which is sitting behind an Application Load Balancer. The IT Security team needs to respond to possible layer 7 DDoS attacks in the most efficient time possible. Which of the following 2 actions can help achieve this?
    - Investigate and mitigate the attack on your own: If you determine that activity represents a DDoS attack, you can create your own AWS WAF rules to mitigate the attack. AWS WAF is included with AWS Shield Advanced at no additional cost. AWS provides preconfigured templates to get you started quickly.
    - If you are an AWS Shield Advanced customer, you also have the option of contacting the AWS Support Center: If you want assistance in applying mitigations, you can contact the AWS Support Center. Critical and urgent cases are routed directly to DDoS experts. With AWS Shield Advanced, complex cases can be escalated to the DRT (DDos Response Team), which has deep experience in protecting AWS, Amazon.com, and its subsidiaries.

- If you want to allow or block web requests based on the country that the requests originate from, create one or more geo match conditions. A geo match condition lists countries that your requests originate from. Later in the process, when you create a web ACL, you specify whether to allow or block requests from those countries.

- You can install a single AWS Marketplace rule group from your preferred AWS partner and also add your own customized AWS WAF rules for increased protection. If you are subject to regulatory compliance like PCI or HIPAA, you might be able to use AWS Marketplace rule groups to satisfy web application firewall requirements.

## Cloudfront
- Your company is planning on using an S3 bucket and a cloudfront distribution to distribute objects to users across the world. They want to use their own domain name with the cloudfront distribution and also ensure that the communication is secure. Which of the following steps need to be part of the implementation plan.
    - Change the Viewer protocol Policy
    - If you're using your own domain name, such as example.com, you need to change several CloudFront settings. You also need to use an SSL/TLS certificate provided by AWS Certificate Manager (ACM), import a certificate from a third-party certificate authority into ACM or the IAM certificate store
    - On the Behaviors tab, choose the cache behavior that you want to update, and then choose Edit. Specify one of the following values for Viewer Protocol Policy:
        - http and https
        - Redirect http to https
        - https only

- To ensure that your users access your objects using only CloudFront URLs, regardless of whether the URLs are signed, perform the following tasks:
    1.  Create an origin access identity, which is a special CloudFront user, and associate the origin access identity with your distribution. (For web distributions, you associate the origin access identity with origins, so you can secure all or just some of your Amazon S3 content.)
    2.  Change the permissions either on your Amazon S3 bucket or on the objects in your bucket so only the origin access identity has read permission (or read and download permission). 

- If you use CloudFront signed URLs or signed cookies to limit access to files in your Amazon S3 bucket, you probably also want to prevent users from accessing your Amazon S3 files by using Amazon S3 URLs. If users access your files directly in Amazon S3, they bypass the controls provided by CloudFront signed URLs or signed cookies, for example, control over the date and time that a user can no longer access your content and control over which IP addresses can be used to access content. In addition, if users access files both through CloudFront and directly by using Amazon S3 URLs, CloudFront access logs are less useful because they're incomplete.
    - To ensure that your users access your files using only CloudFront URLs, regardless of whether the URLs are signed, do the following:
        1. Create an origin access identity, which is a special CloudFront user, and associate the origin access identity with your distribution. You associate the origin access identity with origins, so that you can secure all or just some of your Amazon S3 content. You can also create an origin access identity and add it to your distribution when you create the distribution. For more information, see Creating a CloudFront Origin Access Identity and Adding it to Your Distribution.
        2. Change the permissions either on your Amazon S3 bucket or on the files in your bucket so that only the origin access identity has read permission (or read and download permission). When your users access your Amazon S3 files through CloudFront, the CloudFront origin access identity gets the files on behalf of your users. If your users request files directly by using Amazon S3 URLs, they're denied access. The origin access identity has permission to access files in your Amazon S3 bucket, but users don't. For more information, see Granting the Origin Access Identity Permission to Read Files in Your Amazon S3 Bucket.

- A team has setup a Cloudfront distribution with a web application hosted on an EC2 Instance as the Origin point. There is a security requirement to ensure that all requests via Cloudfront are recorded. Which of the following implementation steps need to be carried out. Choose 2 answers from the options given below
    - Enable logging for a distribution
    - Create a destination S3 bucket for the logs
    - When you enable logging for a distribution, you specify the Amazon S3 bucket that you want CloudFront to store log files in. If you're using Amazon S3 as your origin, we recommend that you do not use the same bucket for your log files; using a separate bucket simplifies maintenance.  You can store the log files for multiple distributions in the same bucket. When you enable logging, you can specify an optional prefix for the file names, so you can keep track of which log files are associated with which distributions.

- A team has setup a Cloudfront distribution with a web application hosted on an EC2 Instance as the Origin point. The Web application serves videos to various users. There is a requirement to ensure that a certain section of files need to be accessed by only a certain subscriber in the web site. Which of the following would you consider for this requirement?
    - Use signed Cookies
        - Use signed cookies in the following cases:
            - You want to provide access to multiple restricted files, for example, all of the files for a video in HLS format or all of the files in the subscribers' area of a website.
            - You don't want to change your current URLs.

- A team has setup a Cloudfront distribution with a web application hosted on an EC2 Instance as the Origin point. A security requirement mandates that all configuration changes to the Cloudfront distribution needs to be recorded. Which of the following service would you use for this purpose?
    - AWS Config
        - You can use AWS Config to record configuration changes for CloudFront distribution settings changes. For example, you can capture changes to distribution states, price classes, origins, geo restriction settings, and Lambda@Edge configurations.

## Active Directory
- A company is planning on moving their on-premise workloads to AWS. They are planning on setting up their own Active Directory setup on a set of EC2 Instances. They need to ensure that for the time being, resources from their on-premise data centre can access the Active Directory setup. Which of the following implementation steps need to be carried out. Choose 2 answers from the options given below
    - Ensure that the Network ACL’s have been set for allowing traffic
    - Ensure that the Security Groups have been set for allowing traffic
    - If you're deploying and managing your own AD DS installation domain controllers and member servers will require several security group rules to allow traffic for services such as AD DS replication, user authentication, Windows Time services, and Distributed File System (DFS), among others. You should also consider restricting these rules to specific IP subnets that are used within your VPC.

- A company is planning on moving their on-premise workloads to AWS. They are planning on setting up their own Active Directory setup on a set of EC2 Instances. You need to ensure that administrators have secure access without the need to traverse through the Internet. How can you achieve this?
    -  Use the Direct Connect

- You can enable multi-factor authentication (MFA) for your AWS Managed Microsoft AD directory to increase security when your users specify their AD credentials to access Supported Amazon Enterprise Applications. When you enable MFA, your users enter their username and password (first factor) as usual, and they must also enter an authentication code (the second factor) they obtain from your virtual or hardware MFA solution. These factors together provide additional security by preventing access to your Amazon Enterprise applications, unless users supply valid user credentials and a valid MFA code.

- Your company has setup the AWS Managed Microsoft AD directory. There are on-premise nodes that will be using the AWS Managed Microsoft AD directory for authentication. You need to ensure that all traffic is encrypted in transit. How can you achieve this in the most IDEAL manner?
    - To mitigate this form of data exposure, AWS Managed Microsoft AD provides an option for you to enable LDAP over Secure Sockets Layer (SSL)/Transport Layer Security (TLS), also known as LDAPS. With LDAPS, you can improve security across the wire and meet compliance requirements by encrypting all communications between your LDAP-enabled applications and AWS Managed Microsoft AD directory

- Your company has setup the AWS Managed Microsoft AD directory service. They need to ensure that users accounts get locked after a specified number of failed logon attempts. How can you achieve this?
    - Use Password policies in the directory service

## IAM
- You share resources in one account with users in a different account. By setting up cross-account access in this way, you don't need to create individual IAM users in each account. In addition, users don't have to sign out of one account and sign into another in order to access resources that are in different AWS accounts. After configuring the role, you see how to use the role from the AWS Management Console, the AWS CLI, and the API.

- Your company has just started using an AWS account. They want to ensure that they apply the right security principles to the root user for their AWS Account. Which of the following are the right security measures to put in place? Choose 2 answers from the options given below
    - Delete any Access keys which are present for the root account
    - Have a rotation policy in place for changing the root account password

