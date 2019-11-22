# Exam Simulation Notes from Whizlab tests 

- Your company is performing a security audit to AWS environment and the security specialist asked you to provide a document that contained the status of all IAM users in the AWS account. In this document, it should include information such as when users were created, when passwords were used or changed last time, whether or not MFA was enabled, etc. What is the best way to provide this documentation?
    - Generate and download an IAM credential report through AWS Management Console or AWS SDKs.

- As a DevOps engineer, you need to maintain Jenkins pipelines. Recently, you have created a new pipeline for a migration project. In one stage, you encrypted a file with below command:

aws kms encrypt \
    --key-id 1234abcd-fa85-46b5-56ef-1234567890ab \
    --plaintext fileb://ExamplePlaintextFile \
    --output text \
    --query CiphertextBlob | base64 \
    --decode > ExampleEncryptedFile

A CMK key was used in the encryption operation. Then in another stage, the encrypted file needs to be decrypted with aws kms decrypt. In terms of this command, which statement is correct?
    - There is no need to add the CMK key information for this command.
        
aws kms decrypt \
    --ciphertext-blob fileb://ExampleEncryptedFile \
    --output text \
    --query Plaintext | base64 --decode > ExamplePlaintextFile
        
- You have maintained an AWS account A which contains a S3 bucket that another AWS account B needs to upload files to. In the S3 bucket policy, s3:PutObject is allowed for the IAM user in account B. And the IAM user in account B can use “aws s3api put-object” to upload objects to the S3 bucket successfully. However, it has been found that the new uploaded objects cannot be opened by users in AWS account A. How should you fix this issue?
    - When user in account B uploads objects, add the option of --acl "bucket-owner-full-control" to aws s3api put-object in order to give permissions to the bucket owner for the objects.

- A company is deploying a new web application on AWS. Based on their other web applications, they anticipate being the target of frequent DDoS attacks. Which steps can the company use to protect their application? Select 2 answers from the options given below.
    - Use CloudFront and AWS WAF to prevent malicious traffic from reaching the application
    - Use an ELB Application Load Balancer and Auto Scaling group to scale to absorb application layer traffic.

- A company has external vendors that must deliver files to the company. These vendors have cross-account permission to upload objects to one of the company’s S3 buckets.

- What combination of steps needs to be followed by vendor to allow company user to access the uploaded files? Select 2 answers from the options given below
    - Upload the file to the company’s S3 bucket
    - Add a grant to the object’s ACL giving full permissions to bucket owner.
    - __NOTE:   A bucket owner can enable other AWS accounts to upload objects. These objects are owned by the accounts that created them. The bucket owner does not own objects that were not created by the bucket owner. Therefore, for the bucket owner to grant access to these objects, the object owner must first grant permission to the bucket owner using an object ACL__

- You have a paid service providing custom digital art that is hosted on AWS using S3.  In order to promote your service, you wish to provide a limited sample of artwork to unauthenticated guest users for free.  Which combination of steps will enable guest users to view your free subset of artwork (Select TWO).
    - Enable Unauthenticated identities in Amazon Cognito Identity Pools
    - Assign an IAM Role with appropriate S3 Access permissions

- Your application backend services are hosted on AWS and provide several REST API methods managed via AWS API Gateway.  You’ve decided to start using AWS Cognito for your application’s user management.  What combination of steps is required to properly authorize a call to one of the REST API methods using an access token (Select TWO)?
    - Create a COGNITO_USER_POOLS authorizer
    - Configure a single-space separated list of OAuth Scopes on the API method
        - Because OAuth Scopes must be specified on the API method so that they can be compared against scopes that's claimed in the incoming token.
        - With the COGNITO_USER_POOLS authorizer, if the OAuth Scopes option isn't specified, API Gateway treats the supplied token as an identity token and verifies the claimed identity against the one from the user pool. Otherwise, API Gateway treats the supplied token as an access token and verifies the access scopes that are claimed in the token against the authorization scopes declared on the method.

- Your company CSO (Chief Security Officer) has directed you to enhance the security of a critical application by implementing a CAPTCHA as part of the user sign-in process.  What is the most efficient method to implement this capability?
    - Create an Auth Challenge Lambda Trigger
        - AWS Lambda functions can be created and then triggered during user pool operations such as user sign-up, confirmation, and sign-in (authentication) with a Lambda trigger in order to customize User Workflows.

- You are responsible for deploying a critical application onto AWS. Part of the requirements for this application is to ensure that the controls set for this application meet PCI compliance. Which of the following services can be used to fulfill this requirement? Choose 2 answers from the options given below
    - Amazon Cloudwatch Logs
    - Amazon Cloudtrail

- A company wishes to enable Single Sign On (SSO) so its employees can login to the management console using their corporate directory identity. Which of the following step is required as part of the process?
    - Create an IAM role that establishes a trust relationship between IAM and the corporate directory identity provider (IdP)

- Company policy requires that all insecure server protocols, such as FTP, Telnet, HTTP, etc be disabled on all servers. The security team would like to regularly check all servers to ensure compliance with this requirement by using a scheduled CloudWatch event to trigger a review of the current infrastructure.  What process will check compliance of the company’s EC2 instances?
    - Run an Amazon Inspector assessment using the Runtime Behavior Analysis rules package against every EC2 instance.

- An application running on EC2 instances must use a username and password to access a database. The developer has stored those secrets in the SSM Parameter Store with type SecureString using the customer managed KMS CMK.  Which combination of configuration steps will allow the application to access the secrets via the API? Select 2 answers from the options below.
    - Add permission to read the SSM parameter to the EC2 instance role.
    - Add permission to use the KMS key to decrypt to the EC2 instance role

- A company wants to have an Intrusion detection system available for their VPC in AWS. They want to have complete control over the system. Which of the following would be ideal to implement?
    - Use VPC Flow logs to detect the issues and flag them accordingly.

- A security team must present a daily briefing to the CISO that includes a report of which of the company’s thousands of EC2 instances and on-premises servers are missing the latest security patches. All instances/servers must be brought into compliance within 24 hours so they do not show up on the next day’s report. How can the security team fulfill these requirements?
    - Use Systems Manager Patch Manager to generate the report of out of compliance instances/ servers. Use Systems Manager Patch Manager to install the missing patches.

- You can use the CloudWatch Logs agent installer on an existing EC2 instance to install and configure the CloudWatch Logs agent. After installation is complete, logs automatically flow from the instance to the log stream you create while installing the agent. The agent confirms that it has started and it stays running until you disable it.
    - In addition to using the agent, you can also publish log data using the AWS CLI, CloudWatch Logs SDK, or the CloudWatch Logs API. The AWS CLI is best suited for publishing data at the command line or through scripts. The CloudWatch Logs SDK is best suited for publishing log data directly from applications or building your own log publishing application.

- You are working in the IT security team in a big company. In order to perform security checks in AWS services, you have written dozens of custom AWS Config rules. One of them is to check if the S3 bucket policy contains certain explicit denies. This particular Config rule is supposed to be applied for all S3 buckets. Your manager has asked you how to trigger the custom Config rule. Which answers are correct? (Select TWO.)
    - It can be triggered whenever there is a configuration change for a S3 bucket.
    - The custom Config rule can be triggered periodically such as every hour.
    - __There are two types of triggers: Configuration Changes and Periodic (Supported Frequency is every 1 hour, 3 hours, 6 hours, 12 hours, 24 hours). There is no difference between AWS managed Config rule and custom Config rule in terms of triggers.__

- When you import key material into a CMK, the CMK is permanently associated with that key material. You can reimport the same key material, but you cannot import different key material into that CMK. Also, you cannot enable automatic key rotation for a CMK with imported key material. However, you can manually rotate a CMK with imported key material.

- How can you ensure that instance in an VPC does not use AWS DNS for routing DNS requests. You want to use your own managed DNS instance. How can this be achieved?
    - Create a new DHCP options set and replace the existing one.
        - In order to use your own DNS server , you need to ensure that you create a new custom DHCP options set with the IP of the custom DNS server. You cannot modify the existing set , so you need to create a new one.

- A windows machine in one VPC needs to join the AD domain in another VPC. VPC Peering has been established. But the domain join is not working. What is the other step that needs to be followed to ensure that the AD domain join can work as intended
    - Ensure the security groups for the AD hosted subnet has the right rule for relevant subnets
        - In addition to VPC peering and setting the right route tables , the security groups for the AD EC2 instance needs to ensure the right rules are put in place for allowing incoming traffic.

- You need to have a cloud security device which would allow to generate encryption keys based on FIPS 140-2 Cryptographic Module Validation Program. Which of the following can be used for this purpose. Please select 2 Options.
    - AWS KMS
    - AWS Cloud HSM

- Your financial services organization is using AWS S3 service to store highly sensitive data.  What is the correct IAM Policy that must be applied to ensure that all objects uploaded to the S3 bucket are encrypted?
```
{
   "Version":"2012-10-17",
   "Id":"PutObjPolicy",
   "Statement":[{
         "Sid":"DenyUnEncryptedObjectUploads",
         "Effect":"Deny",
         "Principal":"*",
         "Action":"s3:PutObject",
         "Resource":"arn:aws:s3:::SensitiveDataBucket/*",
         "Condition":{
            "StringNotEquals":{
               "s3:x-amz-server-side-encryption":"AES256"
            }
         }
      }
   ]
}
```

- A company stores critical data in an S3 bucket. There is a requirement to ensure that an extra level of security is added to the S3 bucket. In addition , it should be ensured that objects are available in a secondary region if the primary one goes down. Which of the following can help fulfil these requirements? Choose 2 answers from the options given below
    - Enable bucket versioning and also enable Cross Region Replication
    - For the Bucket policy add a condition for { "Null": { "aws:MultiFactorAuthAge":true }}
    - NOTE:  __You can enforce the MFA authentication requirement using the aws:MultiFactorAuthAge key in a bucket policy. IAM users can access Amazon S3 resources by using temporary credentials issued by the AWS Security Token Service (STS). You provide the MFA code at the time of the STS request.__

- Your company manages thousands of EC2 Instances. There is a mandate to ensure that all servers don’t have any critical security flaws. Which of the following can be done to ensure this? Choose 2 answers from the options given below.
    - Use AWS Inspector to ensure that the servers have no critical flaws.
    - Use AWS SSM to patch the servers

- You need to inspect the running processes on an EC2 Instance that may have a security issue. How can you achieve this in the easiest way possible. Also you need to ensure that the process does not interfere with the continuous running of the instance.
    - Use the SSM Run command to send the list of running processes information to an S3bucket.
        - The SSM Run command can be used to send OS specific commands to an Instance. Here you can check and see the running processes on an instance and then send the output to an S3 bucket.

- You are trying to use the Systems Manager to patch a set of EC2 systems. Some of the systems are not getting covered in the patching process. Which of the following can be used to troubleshoot the issue? Choose 3 answers from the options given below.
    - For ensuring that the instances are configured properly you need to ensure the following
        1. You installed the latest version of the SSM Agent on your instance
        2. Your instance is configured with an AWS Identity and Access Management (IAM) role that enables the instance to communicate with the Systems Manager API
        3. You can use the Amazon EC2 Health API to quickly determine the following information about Amazon EC2 instances The status of one or more instances
            - The last time the instance sent a heartbeat value
            - The version of the SSM Agent
            - The operating system
            - The version of the EC2Config service (Windows)
            - The status of the EC2Config service (Windows)

- You are trying to use the AWS Systems Manager run command on a set of Amazon Linux AMI Instances. The run command is not working on a set of Instances. What can you do to diagnose the issue? Choose 2 answers from the options given below
    - Ensure that the SSM agent is running on the target machine
    - Check the /var/log/amazon/ssm/errors.log file

- You are working for a company and been allocated the task for ensuring that there is a federated authentication mechanism setup between AWS and their On-premise Active Directory. Which of the following are important steps that need to be covered in this process? Choose 2 answers from the options given below.
    - Determining how you will create and delineate your AD groups and IAM roles in AWS is crucial to how you secure access to your account and manage resources. SAML assertions to the AWS environment and the respective IAM role access will be managed through regular expression (regex) matching between your on-premises AD group name to an AWS IAM role.
        - One approach for creating the AD groups that uniquely identify the AWS IAM role mapping is by selecting a common group naming convention. For example, your AD groups would start with an identifier, for example, AWS-, as this will distinguish your AWS groups from others within the organization. Next, include the 12-digit AWS account number. Finally, add the matching role name within the AWS account.
    - ADFS federation occurs with the participation of two parties; the identity or claims provider (in this case the owner of the identity repository – Active Directory) and the relying party, which is another application that wishes to outsource authentication to the identity provider; in this case Amazon Secure Token Service (STS). The relying party is a federation partner that is represented by a claims provider trust in the federation service.

- Which technique can be used to integrate AWS IAM (Identity and Access Management) with an on-premise LDAP (Lightweight Directory Access Protocol) directory service for single sign-on access to AWS console?
    - Use SAML (Security Assertion Markup Language) to enable single sign-on between AWS and LDAP.

- You have an EBS volume attached to an EC2 Instance which uses KMS for Encryption. Someone has now gone ahead and deleted the Customer Key which was used for the EBS encryption. What should be done to ensure the data can be decrypted.
    - Even when the scheduled time elapses and AWS KMS deletes the CMK, there is no immediate effect on the EC2 instance or the EBS volume, because Amazon EC2 is using the plaintext data key, not the CMK.
    - However, when the encrypted EBS volume is detached from the EC2 instance, Amazon EBS removes the plaintext key from memory. The next time the encrypted EBS volume is attached to an EC2 instance, the attachment fails, because Amazon EBS cannot use the CMK to decrypt the volume's encrypted data key.
    - NOTE: __once the key has been deleted , you cannot recover it.__

- You work as an administrator for a company. The company hosts a number of resources using AWS. There is an incident of a suspicious API activity which occurred 11 days ago. The Security Admin has asked to get the API activity from that point in time. How can this be achieved?
    - Search the Cloudtrail event history on the API events which occurred 11 days ago.
        - The Cloud Trail event history allows to view events which are recorded for 90 days. So one can use a metric filter to gather the API calls from 11 days ago.

- As an AWS account administrator, you wish to perform an audit and create a report of all services that have not been used in the “DevelopmentOU” in the past 6 months. What AWS service can you use to most efficiently accomplish this task?
    - AWS Organization Service Access Report
        - AWS Organization Service Access Report produces a temporal report of AWS Service Activity

- In order to meet data residency compliance requirements for a large bank, you must ensure that all S3 buckets are created in eu-west-2 region.  You plan to use SCP to enforce this rule.  Which SCP will accomplish this?
```
{
  "Version":"2012-10-17",
  "Statement":[
    {
      "Sid":"DataGovernancePolicy",
      "Effect":"Deny",
      "Action":[
        "s3:CreateBucket"
      ],
      "Resource":[
        "arn:aws:s3:::*"
      ],
      "Condition": {
        "StringNotLike": {
          "s3:LocationConstraint": "eu-west-2"
        }
      }
    }
  ]
}
```

- You are designing a data lake for analysis of financial data.  The system consists of a data ingestion component utilizing AWS Kinesis and a storage component utilizing AWS S3.  Compliance requirements specify that all data must be encrypted by a CMK managed using AWS KMS.  What is the best way to ensure that all encrypted data in S3 is written by the AWS Kinesis service?
```
Update the AWS KMS Key Policy using kms:ViaService condition:
"Condition": {
    "StringEquals": {
        "kms:ViaService": "kinesis.%REGION_NAME%.amazonaws.com"
    }
}
```

- You currently have an S3 bucket hosted in an AWS Account. It holds information that needs be accessed by a partner account. Which is the MOST secure way to allow the partner account to access the S3 bucket in your account? Select 3 options.
    - Ensure an IAM role is created which can be assumed by the partner account.
    - Ensure the partner uses an external id when making the request
    - Provide the ARN for the role to the partner account

- You have a set of Customer keys created using the AWS KMS service. These keys have been used for around 6 months. You are now trying to use the new KMS features for the existing set of key’s but are not able to do so. What could be the reason for this.
    - You have not explicitly given access via the key policy
        - By default , keys created in KMS are created with the default key policy. When features are added to KMS, you need to explicitly update the default key policy for these keys.

- Your company is using S3 for storage of data in the cloud.  They want to ensure that all data in the bucket is encrypted.  Which option meets this requirement with least overhead?
    - Enable AES-256 encryption (`Remember:  Least Overhead`)

- You’re building a distributed HPC system that will process large multi-GB files.  The sensitive nature of the data in these files requires them to be encrypted.  What steps are the best approach to use AWS KMS to satisfy the encryption requirement (Select TWO)?
    - Use GenerateDataKeyWithoutPlaintext API.  Distribute data key to system components.
        - This operation returns a data key that is encrypted under a customer master key (CMK) that you specify. GenerateDataKeyWithoutPlaintext is identical to GenerateDataKey except that returns only the encrypted copy of the data key.
    - Use Decrypt API to decrypt data key.  Use plaintext key to encrypt the data.

- A company is using a Redshift cluster to store their data warehouse. There is a requirement from the Internal IT Security team to ensure that data gets encrypted at rest for the Redshift database. How can this be achieved?
    - Use AWS KMS with a Customer Master Key

- You serve as a KMS Key Administrator for your company department.  A KMS customer managed key with imported key material is about to expire.  What is the correct sequence of steps to rectify this situation?
    - Delete the existing key material in the KMS CMK. Download the wrapping key and import token. Encrypt the same key material.  Import the same key material into the KMS CMK.
        - To change the expiration date of a KMS key, you must reimport the same key material and specify a new expiration date.

- You serve as a KMS Key Administrator for your company department.  You’ve created a new KMS CMK with imported key material.  You’re importing the key material into the KMS CMK.  The import operation is failing.  What are the possible causes of this problem (Select TWO)?
    - Your key material is not a 256-bit symmetric key.
    - You waited longer than 24 hours and the import token has expired.

- You’ve accidentally lost the administrator password for an EBS-backed Windows Server EC2 instance.  You want to regain access to your instance.  What is the easiest way to resolve this issue?
    - Use Systems Manager Run Command to run AWSSupport-RunEC2RescueForWindowsTool command document.

- Your company hosts critical data in an S3 bucket. There is a requirement to ensure that all data is encrypted. There is also metadata about the information stored in the bucket that needs to be encrypted as well. Which of the below measures would you take to ensure that the metadata is encrypted?
    - Put the metadata in a DynamoDB table and ensure the table is encrypted during creation time.
        - One key thing to note is that when the S3 bucket objects are encrypted , the meta data is not encrypted. So the best option is to use an encrypted DynamoDB table

- One of the EC2 Instances in your company has been compromised. What steps would you take to ensure that you could apply digital forensics on the Instance. Select 2 answers from the options given below
    - Turn on AWS CloudTrail in every AWS Region
    - Protect log information
    - NOTE:  _Terminating the instance means that you cannot conduct forensic analysis on the instance_

- One of your company’s EC2 Instances have been compromised. The company has strict policies and needs a thorough investigation on finding the culprit for the security breach. What would you do in this case. Choose 3 answers from the options given below.
    - Take a snapshot of the EBS volume
    - Isolate the machine from the network
    - Make sure that logs are stored securely for auditing and troubleshooting purpose

- A company has a large set of keys defined in AWS KMS. The application uses the keys frequently. What is one of the ways that can be used to reduce the cost of accessing the keys in the AWS KMS service?
    - Data key caching stores data keys and related cryptographic material in a cache. When you encrypt or decrypt data, the AWS Encryption SDK looks for a matching data key in the cache. If it finds a match, it uses the cached data key rather than generating a new one. Data key caching can improve performance, reduce cost, and help you stay within service limits as your application scales.

- Your company has a set of EC2 Instances defined in AWS. They need to ensure that all traffic packets are monitored and inspected for any security threats. How can this be achieved? Choose 2 answers from the options given below
    - Use a host based intrusion detection system
    - Use a third party firewall installed on a central EC2 Instance

- You suspect that an EC2 instance has been compromised.  What actions should you take to begin a forensics investigation into the security incident (Select TWO)?
    - Update the compromised instance security group.
        - You should quarantine the EC2 instance by updating its security group.  The new security group should have very restrictive rules restricting all egress traffic and allowing only ingress traffic from a dedicated incident response subnet.
    - Provision a forensic analysis EC2 instance.
        - use a dedicated incident response subnet that will be used to perform forensic analysis.
    - NOTE:  _DO NOT stop the instance since this will remove useful volatile data (e.g. running processes)._

- Your company has a set of EBS volumes defined in AWS. The security mandate is that all EBS volumes are encrypted. What can be done to notify the IT admin staff if there are any unencrypted volumes in the account.
    - The encrypted-volumes config rule for AWS Config can be used to check for unencrypted volumes.
        - "Checks whether the EBS volumes that are in an attached state are encrypted. If you specify the ID of a KMS key for encryption using the kmsId parameter, the rule checks if the EBS volumes in an attached state are encrypted with that KMS key".

- You have a bucket and a VPC defined in AWS. You need to ensure that the bucket can only be accessed by the VPC endpoint. How can you accomplish this?
    - Modify the bucket Policy for the bucket to allow access for the VPC endpoint
```
{
   "Version": "2012-10-17",
   "Id": "Policy1415115909152",
   "Statement": [
     {
       "Sid": "Access-to-specific-VPCE-only",
       "Principal": "*",
       "Action": "s3:*",
       "Effect": "Deny",
       "Resource": ["arn:aws:s3:::examplebucket",
                    "arn:aws:s3:::examplebucket/*"],
       "Condition": {
         "StringNotEquals": {
           "aws:sourceVpce": "vpce-1a2b3c4d"
         }
       }
     }
   ]
}
```

- In order to encrypt data in transit for a connection to an AWS RDS instance, which of the following would you implement
    - SSL from your application

- A device manufacturer wants to digitally sign their firmware software in order to ensure authenticity.  How can this be accomplished in the most efficient way?
    -  Use sign command in key_mgmt_util of CloudHSM
        - There is no SignDocument KMS API.  AWS KMS is used for management of symmetric keys and does not provide PKI functionality.
        - There is no SignDocument CloudHSM API. AWS CloudHSM API only provides actions for management of CloudHSM cluster.

- A Devops team is currently looking at the security aspect of their CI/CD pipeline. They are making use of AWS resources for their infrastructure. They want to ensure that the EC2 Instances don’t have any high security vulnerabilities. They want to ensure a complete DevSecOps process. How can this be achieved?
    - Amazon Inspector offers a programmatic way to find security defects or misconfigurations in your operating systems and applications. Because you can use API calls to access both the processing of assessments and the results of your assessments, integration of the findings into workflow and notification systems is simple. DevOps teams can integrate Amazon Inspector into their CI/CD pipelines and use it to identify any pre-existing issues or when new issues are introduced.

- A company security team would like to receive notifications if any EC2 instance in their AWS environment is querying an IP address that is associated with cryptocurrency-related activity.  What steps can the security team take to achieve this most efficiently (SELECT TWO)?
    - Enable Amazon GuardDuty
    - Subscribe to SNS Topic

- Your application currently use AWS Cognito for authenticating users. Your application consists of different types of users. Some users are only allowed read access to the application and others are given contributor access. How would you manage the access  effectively?
    - Create different cognito groups, one for the readers and the other for the contributors.

- DDoS attacks that happen at the application layer commonly target web applications with lower volumes of traffic compared to infrastructure attacks. To mitigate these types of attacks, you should probably want to include a Non-AWS WAF (Web Application Firewall) as part of your infrastructure. To inspect all HTTP requests, WAFs sit in-line with your application traffic. Unfortunately, this creates a scenario where WAFs can become a point of failure or bottleneck. To mitigate this problem, you need the ability to run multiple WAFs on demand during traffic spikes. This type of scaling for WAF is done via a “WAF sandwich.” Which of the following statements best describes what a “WAF sandwich" is? Choose the correct answer from the options below
    - ![WAF sandwich](images/waf_sandwich)
    - The EC2 instance running your WAF software is included in an Auto Scaling group and placed in between two Elastic load balancers.

- A company that has multiple accounts, has hired a third-party security auditor, and the auditor needs read-only access to all AWS resources and logs of all VPC records and events that have occurred on AWS. How can the company meet the auditor's requirements without comprising security in the AWS environment? Choose the correct answer from the options below
    - Enable Cloudtrail logging and use cross account IAM roles for providing read only access to auditor on required AWS resources including the bucket containing the CloudTrail Logs
        - Cross-account IAM roles allow customers to securely grant access to AWS resources in their account to a third party, like an APN Partner, while retaining the ability to control and audit who is accessing their AWS account. Cross-account roles reduce the amount of sensitive information APN Partners need to store for their customers, so that they can focus on their product instead of managing keys. In this blog post, I explain some of the risks of sharing IAM keys, how you can implement cross-account IAM roles, and how cross-account IAM roles mitigate risks for customers and for APN Partners, particularly those who are software as a service (SaaS) providers.

- Your company has a hybrid environment , with on-premise servers and servers hosted in the AWS cloud. They are planning to use the Systems Manager for patching servers. Which of the following is a pre-requisite for this to work?
    - Ensure that an IAM service role is created
        - You need to ensure that an IAM service role is created for allowing the on-premise servers to communicate with the AWS Systems Manager

- An application deployed to EC2 is configured to use AWS Secrets Manager for rotation of secrets for RDS database.  The application experiences occasional intermittent sign-in failures.  What options can resolve this issue (SELECT TWO)?
    - Implement Exponential Backoff in your application
        - implements retry functionality in the application.
    - Use Multi-User Rotation
        - When using “Single-User Rotation” mode in AWS Secrets Manager, Secrets Manager uses a single user to rotate its own credentials.  Sign-in failures can occur between the moment when the old password is removed by the rotation and the moment when the updated password is made accessible as a new version of the secret.  This time window should be very short, but it can happen.
        - In this scenario, separate “master” user credentials are used for secret rotation. The old version of the secret continues to operate and handle service requests while the new version is prepared and tested. The old version isn't deleted until after the clients switch to the new version. There's no downtime while changing between versions.