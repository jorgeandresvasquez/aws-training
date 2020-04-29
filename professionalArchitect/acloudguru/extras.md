## AWS Organizations
- If you receive a single bill monthly for multiple accounts, you're using AWS Organizations.  This is a "legacy" mode supporting only consolidated billing features.  You must migrate your organization to use advanced governance and management capabilities.
    - Note:  After All Features is enabled, it cannot be changed back.
- Concepts and Terms
    - Organization:  All of the accounts that you manage and control in the organization
        - There's a service access report under Organization Activity that displays last accessed services
    - AWS Account:  Smallest unit of management in the Organization
        - Can only sit in one organization at a time
    - Master Account:  Account used to create the organization
        - Payer account
        - Used as a central management and governance hub
    - Organizational Unit (OU): Set of AWS accounts logically grouped within an organization
        - You can nest OUs inside other OUs to form a hierarchical structure
        - An Organizational Unit can have AWS accounts and other Organizational Units as members. This makes the whole structure similar to a tree. The accounts are organized in a hierarchical, tree-like structure.
        - An OU can have only one parent. 
        - An AWS account can be a member of only one OU.
        - One thing to note is that user cannot move an OU to another place from console or CLI command. So user has to create a new OU and move accounts to it
    - Policy:  Document describing controls to be applied to a selected set of accounts
- Capabilities
    - Manage and define your organization and accounts
        - Create new AWS accounts programatically
        - Group accounts into OUs for management
        - Centrally provision accounts (AWS Cloudformation Stacksets)
        - Tag AWS Accounts
        - Manage service quotas (limits) for new accounts (using AWS Service Quotas)
    - Control Access and permissions
        - Deploy console and CLI access to accounts (AWS Single Sign-on)
        - Define permissions based on membership in an organization (aws:PrincipalOrgId condition key)
        - Author fine-grained permission guardrails across accounts (Service Control Policies)
            - SCPs define the maximum available permissions for IAM entities in an account but do not grant specific permissions
            - Can be attached to the root, OUs and individual accounts
            - ![Permissions are the intersection of SCP and IAM Permissions](images/scp_iam_intersection.png)
            - Authoring SCPs
                - Whitelisting - only allow specific actions
                - Blacklisting - block specific actions
                    - Requires and Allow:* to avoid denying access to all servies
                    - Typical evaluation:  Explicit Deny -->Implicit Deny all-->Explicit Allow
                - Sample scenarios:  Deny launching resources outside of specific region(s)
    - Audit, monitor, and secure your environment
        - Aggregate AWS Config data in a central location for compliance auditing of your accounts
        - Centrally create, provision, and modify web application firewalls to secure your applications (AWS Firewall Manager)
        - Accept business agreements for organization accounts (AWS Arttifact)
        - Allow organization-wide notification publishing (AWS Cloudwatch Events)
        - Centrally enable audit logging
            - Ensure that all of your AWS accounts have audit logging enabled centrally
            - Prevent auditing from being modified in an account
            - Have all audit logs consolidated in one central place
    - Share resources across accounts
        - Centrally define critical resources and make them available to your logically isolated workloads in accounts
        - AWS Service Catalog
        - AWS Resource Access Manager (RAM)
            - Amazon VPC Transit Gateways and subnets
            - AWS License Manager configurations
            - Amazon Route 53 resolver rules
    - Centrally Manage Costs and billing
        - Consolidate usage across all accounts into a single bill

## Restricting Access to Amazon S3 Content by Using an Origin Access Identity (OAI)
- To restrict access to content that you serve from Amazon S3 buckets, follow these steps:
    1. Create a special CloudFront user called an origin access identity (OAI) and associate it with your distribution.
    2. Configure your S3 bucket permissions so that CloudFront can use the OAI to access the files in your bucket and serve them to your users. Make sure that users can’t use a direct URL to the S3 bucket to access a file there.
- After you take these steps, users can only access your files through CloudFront, not directly from the S3 bucket.
- In general, if you’re using an Amazon S3 bucket as the origin for a CloudFront distribution, you can either allow everyone to have access to the files there, or you can restrict access. If you restrict access by using, for example, CloudFront signed URLs or signed cookies, you also won’t want people to be able to view files by simply using the direct Amazon S3 URL for the file.
- For more details see:  https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/private-content-restricting-access-to-s3.html

## WAF
- AWS WAF is a web application firewall that helps protect your web applications or APIs against common web exploits that may affect availability, compromise security, or consume excessive resources.
-  You can get started quickly using Managed Rules for AWS WAF, a pre-configured set of rules managed by AWS or AWS Marketplace Sellers. The Managed Rules for WAF address issues like the OWASP Top 10 security risks.
- With AWS WAF, you pay only for what you use. The pricing is based on how many rules you deploy and how many web requests your application receives. There are no upfront commitments.
- You can deploy AWS WAF on Amazon CloudFront as part of your CDN solution, the Application Load Balancer that fronts your web servers or origin servers running on EC2, or Amazon API Gateway for your APIs.

## Other resources
- https://d1.awsstatic.com/whitepapers/AWS_Serverless_Multi-Tier_Architectures.pdf