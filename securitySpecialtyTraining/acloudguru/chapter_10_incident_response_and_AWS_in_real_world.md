# Incident Response & AWS in the Real World

## I've leaked my keys on github accidentally
- Make the keys inactive, create a new pair and delete the old key pair pair

## Reading Cloudtrail Logs
- json files

## Pen Testing - AWS Market Place
- Before you needed to request permission from AWS fro pen testing or vulnerability scans
- AWS customers are welcome to carry out security assessments or penetration tests against their AWS infrastructure without prior approval for 8 services:
    - Amazon EC2 instances, NAT Gateways, and Elastic Load Balancers
    - Amazon RDS
    - Amazon CloudFront
    - Amazon Aurora
    - Amazon API Gateways
    - AWS Lambda and Lambda Edge functions
    - Amazon Lightsail resources
    - Amazon Elastic Beanstalk environments

- The following is prohibited:
    - DNS Zone Walking
    - DDos Simulated attckes
    - Port Flooding
    - Protocol Flooding
    - Request Flooding
- Lots of penetration testing tools available in the EC2 AMI Marketplace
    - Ex:  Kali Linux
- Check this link:  https://aws.amazon.com/security/penetration-testing/

## AWS Certificate Manager
- Domains on route53 can be auto-renewed or extended for more years
- When creating a certificate you can do:  DNS or email validation.
    - For DNS validation youu need to add a CNAME record to your Domain or if on route53 AWS can update DNS config for you
- SSL certificates renew automatically, provided you purchased the domain name from route53 and it's not for a Route53 private hosted zone
- You can use Amazon SSL certificates with both Load Balancers and Cloudfront
- You cannot export the certificates from ACM
- Certificates can also be uploaded to IAM and be used in Load Balancer or Cloudfront distribution, not available ia UI but via CLI only
- To associate a certificate to a ALB:
    - Add Https listener
    - Choose the certificate type:  `ACM` or `IAM` 

## Securing your Load Balancers using Perfect Forward Secrecy
- [Forward Secrecy Definition](https://en.wikipedia.org/wiki/Forward_secrecy)
-  Feature of specific key agreement protocols that gives assurances that session keys will not be compromised even if the private key of the server is compromised
- [Diffie–Hellman key exchange](https://en.wikipedia.org/wiki/Diffie%E2%80%93Hellman_key_exchange)
    - The Diffie–Hellman key exchange method allows two parties that have no prior knowledge of each other to jointly establish a shared secret key over an insecure channel. This key can then be used to encrypt subsequent communications using a symmetric key cipher.
- Port 80 for http 
- Port 443 for https
- When setting up the ALB under Security Settings select the default security policy:  ELBSecurityPolicy-2016-08 (default), this has support for ECDHE (Elliptic Curve Diffie-Hellman Ephemeral.). 
- ECDHE enables ephemeral keys during key exchange, activating Perfect Forward Security, or PFS. PFS uses a unique secret each time a secret key is generated; one secret per session. 

## API Gateway (throttling and caching)
- To prevent your API from being overwhelmed by too many requests, Amazon API Gateway throttles requests to your API
- When request submissions exceed the steady-state request rate and burst limits, APi Gateway fails the limit-exceeding requests and returns 429 (Too Many Requests) error responses to the client
- By default, API Gateway limits the steady-state request rate to 10,000 requests per second (rps)
- It limits the burst to 5,000 requests across all APIs within an AWS account
- The account-level limit and burst limit can be increased uipon request
- API Gateway Caching:
    - You can enable API caching in Amazon API Gateway to cache your endpoint's response.
    - By stage
    - TTL period in seconds (default is 300 seconds, maximum is 3600 seconds, TTL=0 means caching is disabled)
- Scenarios:
    - If a caller submits 10,000 requests in a one second period evenly (for example, 10 requests every millisecond), API Gateway processes all requests without dropping any.
    - If the caller sends 10,000 requests in the first millisecond, API Gateway serves 5,000 of those requests and throttles the rest in the one-second period.
    - If the caller submits 5,000 requests in the first millisecond and then evenly spreads another 5,000 requests through the remaining 999 milliseconds (for example, about 5 requests every millisecond), API Gateway processes all 10,000 requests in the one-second period without returning 429 Too Many Requests error responses.
    - If the caller submits 5,000 requests in the first millisecond and waits until the 101st millisecond to submit another 5,000 requests, API Gateway processes 6,000 requests and throttles the rest in the one-second period. This is because at the rate of 10,000 rps, API Gateway has served 1,000 requests after the first 100 milliseconds and thus emptied the bucket by the same amount. Of the next spike of 5,000 requests, 1,000 fill the bucket and are queued to be processed. The other 4,000 exceed the bucket capacity and are discarded.

## AWS Systems Manager Parameter Store
- AWS Systems Manager gives you visibility and control of your infrastructure on AWS. Systems Manager provides a unified user interface so you can view operational data from multiple AWS services and allows you to automate operational tasks across your AWS resources. 
- Confidential information such as passwords, database connection strings, and license codes can be stored in SSM Parameter Store
- You can store values as plain text or you can encrypt the data.
- You can then reference these values by using their names (names have to be unique).
- You can use this service with EC2, CloudFormation, Lambda, EC2 run Command, etc.

## AWS Systems Manager EC2 Run Command
- Allows you to manage a large number of EC2 instances and on premise systems.
- Allows to automate common admin tasks and ad hoc configuration changes at scale (ex:  installing applications, applying latest security patches, joining new instances to an Windows Domain without having to login to each instance, etc.)

## Compliance Frameworks
- Main 3:
    - PCI
        - The Payment Card Industry Data Security Standard (PCI DSS)
        - Widely Accepted set of policies and procedures intended to optimize the security of credit, debit and cash card transactions and protect cardholders against misuse fo their personal information.
        - v3.2
        - 12 requirements
    - ISO
        - ISO 27001
    - HIPAA
        - Federal Health Insurance Portability and Accountability Act of 1996
    - NIST
        - National Institute of Standards and Technology
    - FIPS 140-2 is a U.S government computer security standard used to approve cryptographic modules.
        - Rated from Level 1 to Level 4, with 4 being the highest security.  Cloud HSM meets the level 3 standard.
- https://aws.amazon.com/compliance/


