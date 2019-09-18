## LightSail
- It is designed to be the easiest way to launch and manage a virtual private server with AWS.

## AWS Hyperplane (internal AWS Service)
- AWS Hyperplane, the Network Function Virtualization platform used for Network Load Balancer and NAT Gateway, has supported inter-VPC connectivity for offerings like AWS PrivateLink, and we are now leveraging Hyperplane to provide NAT capabilities from the Lambda VPC to customer VPCs.

## X-Ray
- AWS X-Ray helps developers analyze and debug production, distributed applications, such as those built using a microservices architecture.
- X-Ray provides an end-to-end view of requests as they travel through your application, and shows a map of your application’s underlying components. 

## ECS
- Amazon Elastic Container Service (ECS) is a highly scalable, high performance container management service that supports Docker containers and allows you to easily run applications on a managed cluster of Amazon EC2 instances.
- Only supports docker containers
- ECS Tasks detail the repository for the docker image, port mappings, environment variables, and volumes to attach. Most everything used in a command line run from Docker will be mapped to the Task definition.
- Once your Tasks are defined, ECS Services detail the auto-scaling nature of the Tasks. If a Task is stopped, the corresponding ECS Service can restart the Task or launch a new instance to replace it.
- Docker encourages you to split your applications up into their individual components, and Elastic Container Service is optimized for this pattern. Tasks allow you to define a set of containers that you would like to be placed together (or part of the same placement decision), their properties, and how they may be linked.
- The Amazon ECS Service scheduler can manage long-running applications and services. The Service scheduler helps you maintain application availability and allows you to scale your containers up or down to meet your application's capacity requirements. 
- You can use any AMI that meets the Amazon ECS AMI specification.

## Fargate
- Fargate removes the responsibility of provisioning, configuring and managing the EC2 instances by allowing AWS to manage the EC2 instances
- Fargate eliminates the need to manage servers, but also puts a requirement of your Task definitions to be stateless.

## Amazon Macie
- Amazon Macie is a security service that uses machine learning to automatically discover, classify, and protect sensitive data in AWS. Amazon Macie recognizes sensitive data such as personally identifiable information (PII) or intellectual property, and provides you with dashboards and alerts that give visibility into how this data is being accessed or moved.
- Presently only supports S3

## AWS Config
- AWS Config is a service that enables you to assess, audit, and evaluate the configurations of your AWS resources. Config continuously monitors and records your AWS resource configurations and allows you to automate the evaluation of recorded configurations against desired configurations. 
- Enables you to simplify compliance auditing, security analysis, change management, and operational troubleshooting.

## AWS Directory Service (AWS Managed Microsoft AD)
- AWS Directory Service for Microsoft Active Directory, also known as AWS Managed Microsoft AD, enables your directory-aware workloads and AWS resources to use managed Active Directory in the AWS Cloud. AWS Managed Microsoft AD is built on actual Microsoft Active Directory and does not require you to synchronize or replicate data from your existing Active Directory to the cloud. 
- Can be used as the Active Directory over VPN or direct connect
- Simple AD is a standalone managed directory that is powered by a Samba 4 Active Directory Compatible Server
- Simple AD provides a subset of the features offered by AWS Managed Microsoft AD, including the ability to manage user accounts and group memberships, create and apply group policies, securely connect to Amazon EC2 instances, and provide Kerberos-based single sign-on (SSO).

## Amazon Workspaces
- Managed, secure cloud desktop service. You can use Amazon WorkSpaces to provision either Windows or Linux desktops in just a few minutes and quickly scale to provide thousands of desktops to workers across the globe.

## AWS OpsWorks Stacks
- lets you manage applications and servers on AWS and on-premises. 
- With OpsWorks Stacks, you can model your application as a stack containing different layers, such as load balancing, database, and application server.

## VM Import/Export
- VM Import/Export enables you to easily import virtual machine images from your existing environment to Amazon EC2 instances and export them back to your on-premises environment. 
- This offering allows you to leverage your existing investments in the virtual machines that you have built to meet your IT security, configuration management, and compliance requirements by bringing those virtual machines into Amazon EC2 as ready-to-use instances. 
- You can also export imported instances back to your on-premises virtualization infrastructure, allowing you to deploy workloads across your IT infrastructure

## AWS Shield
- WS Shield is a managed Distributed Denial of Service (DDoS) protection service that safeguards applications running on AWS. 

## AWS WAF
- AWS WAF is a web application firewall that helps protect your web applications from common web exploits that could affect application availability, compromise security, or consume excessive resources. 
- AWS WAF gives you control over which traffic to allow or block to your web applications by defining customizable web security rules. 
- You can use AWS WAF to create custom rules that block common attack patterns, such as SQL injection or cross-site scripting, and rules that are designed for your specific application.
- Configuration options:
    - Web ACLs
        - Action
            - The action that you want AWS WAF to take on web requests that match all the conditions in the rule: allow, block, or count the requests
        - Rules
            - Conditions
                - SQL Injection Match
                - String Match
                    - identifies the strings that you want AWS WAF to search for in a request
                - IP Match Condition
                    - IP addresses or IP address ranges that requests originate from
                - Geo Match Condition
                    - A geo match condition specifies the country or countries that requests originate from.
                - Size constraint conditions
                    - Identifies the part of web requests, such as a header or a query string, that you want AWS WAF to check for length
                - Cross-site scripting match conditions 
                    - Identifies the part of web requests, such as a header or a query string

## KMS
- AWS KMS allows you to centrally manage and securely store your keys. These are known as customer master keys or CMKs. You can generate CMKs in KMS, in an AWS CloudHSM cluster, or import them from your own key management infrastructure. 
- The AWS KMS custom key store feature combines the controls provided by AWS CloudHSM with the integration and ease of use of AWS KMS. You can configure your own CloudHSM cluster and authorize KMS to use it as a dedicated key store for your keys rather than the default KMS key store.
- Features
    - AWS managed keys
    - Customer managed keys
        - Key Admin Permissions
        - Key Usage Permissions
    - Custom Key Stores

## Consolidated Billing
- You can use the consolidated billing feature in AWS Organizations to consolidate billing and payment for multiple AWS accounts or multiple Amazon Internet Services Pvt. Ltd (AISPL) accounts. 
- Every organization in AWS Organizations has a master account that pays the charges of all the member accounts. For more information about organizations, see the AWS Organizations User Guide. For the rest of this guide, the master account is called a payer account, and the member account is called a linked account, even when we talk about organizations.
- Account charges can be tracked individually

## Amazon Athena
- Amazon Athena is an interactive query service that makes it easy to analyze data in Amazon S3 using standard SQL. Athena is serverless, so there is no infrastructure to setup or manage, and you can start analyzing data immediately. You don’t even need to load your data into Athena, it works directly with data stored in S3
- Amazon Athena uses Presto with full standard SQL support and works with a variety of standard data formats, including CSV, JSON, ORC, Apache Parquet and Avro

## AWS Support
- Support plans:
    - Basic
    - Developer
    - Business
        - Maximum response time for a Business Level Production down support case is 1 hour
    - Enterprise
## AWS Server Migration Service (SMS)
- Automates the migration of your on-premises VMware vSphere, Microsoft Hyper-V/SCVMM, and Azure virtual machines to the AWS Cloud. 
- Incrementally replicates your server VMs as cloud-hosted Amazon Machine Images (AMIs) ready for deployment on Amazon EC2.
- An SMS replication job will fail for VMs with more than 22 volumes attached.
- AWS SMS creates AMIs that use Hardware Virtual Machine (HVM) virtualization. It can't create AMIs that use Paravirtual (PV) virtualization.

## Good Links
- Cheat Sheets:
    - https://tutorialsdojo.com/aws-cheat-sheets/
- https://www.mattbutton.com/2017/10/12/aws-solution-architect-associate-exam-study-notes-ec2-elastic-compute-cloud-and-lambda/
- https://aws.amazon.com/blogs/apn/the-5-pillars-of-the-aws-well-architected-framework/
