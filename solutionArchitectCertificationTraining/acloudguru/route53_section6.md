# Route 53

## DNS 101
- Used to convert human-friendly domain names (ex:  http://acloud.guru) into IP4 or IP6 address
- Ex:  Yellow pages to get someone's telephone number
- IPv4:  32 bit
- IPv6:  128 bits
- We're presently using both IPv4 and IPv6
- Last word in a domain name represents "the top level domain name" (ex:  .com, .edu)
- The second word is optional and is known as second level domain name (Ex:  .gov in .gov.uk)
- Top level domain names are controlled by the IANA (Internet Assigned Numbers Authority) in a root zone database
- The names in a given domain name have to be unique
- Domain registrar is an authority that can assign domain names directly under one or more top-level domain names.
    - Popular domain name registrars: Amazon, godaddy,...
- Domains are registered with InterNIC, as service of ICANN, which enforces uniqueness of domain names across the internet
    - central db knows as the WhoIs database
- SOA record (Start of Authority)
    - One per zone
    - contains time spans
        - ttl
    - email address of person responsible for zone
- NS record
    - Name server records
    - Used by Top Level Domain Servers to direct traffic to the Content DNS server which contains the authoritative DNS records
- Ex:
    - hellocloudgurus2019.com-->.com-->NS Records-->SOA
        - Inside of SOA we will have all of our DNS records
- A record
    - Fundamental type of DNS record
    - Stands for "Address"
    - Translates the name of the domain to an IP address
    - One A record can have multiple IPs
- TTL
    - In seconds
    - The lower the fastest it takes for DNS records to propagate through the internet
- CName
    - Canonical Name:  Used to resolve one domain name to another
- Alias Records
    - Amazon Route 53-specific extension to DNS
    - Map Resource record sets in your hosted zone to ELBs, Cloudfront Distributions or S3 buckets configured as websites
    - Work like a CNAME record
    - Supports domain APEXs (naked domain name) to pointed to and endpoint directly. Because in a standard DNS you would only be able to refer to another hostname via a CNAME. But you will not be able to provision it at the apex of your domain ("example.com"). Route53 allows Alias records to be created to solve this problem, so that you can point your own domain apex records directly to hit the ELB public DNS name.
        - Ex of naked domain names:  naked.com (instead of www.naked.com)
    - Alias Records can also point to AWS Resources that are hosted in other accounts by manually entering the ARN
- MX Record
    - For email
- PTR records
    - Reverse of A record, Way of looking up a name against an IP address
- Hosted Zone
    - Represents a collection of resource record sets that are managed together under a single domain name.
- Your computer's DNS cache stores the locations (IP addresses) of web servers that contain web pages which you have recently viewed. If the location of the web server changes before the entry in your DNS cache updates, you can no longer access the site.  This can be flushed
- Cool tool to switch between countries within VPN:  NordVPN

## Route53
- ELBs do not have pre-defined IPv4 addresses; you resolve them using a DNS name
- Domain names can be purchased directly within AWS
- It can take up to 3 days to register depending on circumstances
- Routing Policies
    1. Simple Routing
        - Setup one A record with multiple IP addresses
        - Route53 returns all values to the user in a random order
        - Cannot be associated to a health check
    2. Weighted Routing
        - Allows to weight traffic
        - Create multiple A records for each IP address, for each entry select:  "Weighted" as Routing policy option, for Weight add value between 1-100. 
        - Option:  "Associate with health checks", EC2 instance that fail health checks won't be receiving traffic 
            - Have to define the health check (IP+frequency+endpoint path+rate to conclude failure)
        - The formula used is:  (weight for a specified record)/(Sum of the weights for all records)
    3. Latency-based Routing
        - Create multiple A records for each IP address, for each Route Policy entry select:  "Latency", automatically selects the region for each IP
        - Select the latency based on the IP on the region with the lowest latency from where the request is coming
        - Not necesarily will always be the region closest to you, sometimes the latency can be better for regions further
    4. Failover Routing
        - 2 A records:  Active and Passive.  The Active needs a health check to be associated, if the health check passes all traffic goes to Active, once it failt all traffic goes to Passive IP.
    5. Geolocation Routing
        - Lets you choose where your traffic will be sent based on the geographic location of your users (location from which DNS queries originate)
        - Ex:  All queries from Europe routed to a fleet of EC2 instances preconfigured for European users (currency in euros preconfigured)
        - When configuring in Location you can choose continent or country.
    6. Geoproximity Routing (Traffic Flow Only)
        - You need to create a Traffic Policy using Traffic Flow 
        - Allows you to route traffic to your resources based on the geographic location of your users and your resources
        - You can also optionally choose to route more traffic or less to a given resource by specifying a value known as a bias
    7. Multivalue Answer Routing
        - Exactly the same as simple routing however it allows you to associate with healthchecks 
- You need to understand route53 since you need to know how to route DNS traffic to your VPC
- Default limit of 50 domains per AWS account, can be increased by contacting AWS support

## Notes from Whizlabs Exam
- Which of the following are main functions of AWS Route 53? (Choose multiple)
    - Register Domain Names
    - Route Internet traffic to the resources for your domain
    - Check the health of your resources
- Your organization had created an S3 bucket for static website hosting. They had created and configured all necessary components for static website and ready to use with host name http://example-bucket.com.s3-website-us-east-2.amazonaws.com. However, they would like to get the website served through domain name www.example-bucket.com which is already registered. Which type of record set you need to create?
    - A - IPv4 Address with Alias=YES
- Which of the following is not an AWS service that AWS Route 53 can route traffic to?
    - Amazon Cloudwatch
- Your organization had setup a web application on AWS VPC with 4 EC2 instances in a private subnet. They had configured an elastic load balancer to distribute traffic between all 4 EC2 instances. They decided to route traffic from internet to the elastic load balancer via a domain “www.example-web-application.com” which they had already registered. Which type of record set you need to create?
    - A - IPv4 Address with Alias=YES
- Which of the following are correct options for logging and monitoring AWS Route 53 service? Choose 3 correct options.
    - Amazon Cloudwatch
    - AWS Route 53 dashboard
    - AWS CloudTrail
    NOTE:  VPC Flow logs are for logging the network traffic going in/coming out of a specific VPC. Route 53 is not a VPC specific service.
- You must use a CNAME record to associate a domain name with an Amazon RDS DB instance.
- You can't create a CNAME record that has the same name as the hosted zone.
- Types of health checks:
    - Health checks that monitor an endpoint
    - Health checks that monitor Cloudwatch alarms





## Read more
https://aws.amazon.com/route53/faqs/

