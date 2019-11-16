## Security Basics
- 3 different models:
    1. CIA
        - Confidentiality
            - Information that you want to keep confidential but expose to other third parties as needed
            - To ensure confidentiality you use:  encryption, 2 factor authentication, biometric verification
            - Relevant services:  IAM, MFA, bucket policies
        - Integrity
            - Maintaining consistency, accuracy, and trustworthiness of data over its entire lifecycle
            - Ensure data cannot be altered by an authorized people
            - To consider:  file permissions, version control, checksums
            - Relevant services:  SSL certificates (Cerificate Manager), IAM, bucked policies, version control, MFA inside S3 to delete objects
        - Availability
            - Keeping your systems available
            - HA Clusters
            - Multiple AZs, regions
            - Relevant services:  Auto-scaling, multi-AZ, route53 with health checks
    2. AAA
        - Authentication
            - IAM
        - Authorization
            - policies
        - Accounting
            - What is it that you're doing on the AWS platform
            - CloudTrail
    3. Non-Repudiation
        - Can't deny that you did something
        - Cloudtrail

## Security OF AWS
- How does AWS secure its stuff?
    - Physical and Environmental security
        - Fire detection and suppression
        - Power
            - 2 independent feeds
        - Climate and Temperature
        - Management
        - Storage Device Decommissioning
    - Business Continuity Management
        - Availability
        - Incident Response
        - Company-Wide Executive Review
        - Communication
    - Network Security
        - Secure Network Architecture
        - Secure Access Points
        - Transmission Protection
        - Amazon Corporate Segregation
            - amazon.com is segregated from AWS network
        - Fault-Tolerant Design
        - Network Monitoring and Protection
            - Includes DDOS mitigation
    - AWS Access
        - Account Review and Audit
        - Background Checks 
        - Credentials Policy
            - Complex pwds that have to be changed every 90 days
    - Secure Design Principles
        - Formal design reviews by security team
        - threat modeling
        - Static analysis 
        - Penetration testing
    - Change Management
        - Software
            - Changes impacting customers are reviewed and well communicated
        - Infrastructure
            - Routine emergency and configuration changes
- Read whitepaper:  "AWS-Security-Processes" pdf
- Why should we trust AWS?
    - AWS Compliance Programs
        - FIPS
        - HIPAA
            - Medical records regulations
        - * ISO 27001
        - PCI DSS Level 1
        - FISMA
        - Department of Defense

## Shared Responsibility Model
- One of most fundamental concepts in AWS Security
- Analogy to when you rent a house (landlord vs tenant's responsibility in security measures)
- AWS manages security "OF" the cloud, the customer is responsible for security "IN" the cloud
- Customers retain control of what security they choose to implement to protect their own content, platform, applications, systems, and networks
- AWS Security Responsibilities:
    - global infrastructure
        - Ex: datacenters
    - hardware, software, networking, and facilities
        - Ex:  hypervisors
    - managed services
        - Ex:  S3, dynamodb
- Customer Security Responsibility:
    - IAAS
    - Including updates and security patches
    - Configuration of AWS-provided firewalls
        - Security Groups
        - NACLs
    - Data encryption
    - Network Traffic Protection
- The model changes for 3 different service types:
    1. Infrastructure
        - Examples:  EC2, EBS, Autoscaling, Amazon VPC
        - Customer responsible for:  From the Operating system and up (Applications, policies, configuration, etc.) 
    2. Container
        - Examples:  RDS, EMR, Elastic Beanstalk
        - Customer responsible for:  Setting up and managing network controls, platform level identity and access management, firewall rules. 
    3. Abstracted
        - Examples:  S3, Glacier, DynamoDB, SQS, SES.
        - Customer responsible for:  Access policies, data encrypted at rest

## Security in AWS
- Cloud Controls:
    - Visibility
        - What assets do you have?
            - AWS Config
                - provides an inventory of your AWS resources and a history of configuration changes to these resources
                - Service that enables you to assess, audit, and evaluate the configurations of your AWS resources.
    - Auditability
        - Do we comply with policies and regulations?
            - AWS Cloudtrail
                - Records every single API call made within your environment
                - Provides event history of your AWS account activity, including actions taken through the AWS Management Console, AWS SDKs, command line tools, and other AWS services.
    - Controllability
        - Is my data controlled?
            - AWS KMS
                - Multi-tenant
            - AWS CloudHSM
                - Dedicated hardware
                - Compliant with FIPS 140-2
    - Agility
        - How quickly can we adapt to changes?
            - AWS Cloudformation
            - AWS Elastic Beanstalk
    - Automation
        - Are our processes repeatable?
            - AWS OpsWorks
                - AWS OpsWorks is a configuration management service that provides managed instances of Chef and Puppet. Chef and Puppet are automation platforms that allow you to use code to automate the configurations of your servers.
            - AWS CodeDeploy
                - AWS CodeDeploy is a fully managed deployment service that automates software deployments to a variety of compute services such as Amazon EC2, AWS Fargate, AWS Lambda, and your on-premises servers.
    - Scale
        - The Scale of AWS is on your side.
- Services that are cross-cutting
    - AWS IAM
    - AWS Trusted Advisor
        - Advices on security, budgets, etc.
    - AWS Cloudwatch

## References
- https://aws.amazon.com/security/
- https://info.acloud.guru/resources/aws-certified-security-speciality-exam-prep-guide
- https://www.whizlabs.com/aws-certified-security-specialty/practice-test/
    
## TODOs
- Read whitepapers
- Read about AWS Config, AWS Trusted Advisor