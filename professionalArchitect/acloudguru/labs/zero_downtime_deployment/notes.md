## Problem Statement
- API that can be called at anytime to generate a hardy dose of Shakespearian insults
- You are preparing to release version 2 but cannot tolerate any downtime at all during deployment

## Solution 1: using Elastic Beanstalk
- Steps:
    1. Zip the solutions from github repositories for v1 and v2 and create a node.js application in Elastic Beanstalk with the v1 zip file
    1. Create a new environment by cloning the previous one (to be used for v2)
    1. Install linux utility `watch` to be able to test this locally and run the following command to invoke the v1 URL every second
        ```watch -n 1 curl -s %URL-v1%```
    1. Upload another version of the application (v2) and deploy it to the new environment
    1. Select one of the environments and then option `Swap URLs` 

## Solution2: using EC2 instances and route53 weighted routing
- Steps:
    1. Create 2 public EC2 instances with one running v1 and the other running v2.
    1. Create 1 A record in route53 mapping the first EC2 instance to a DNS entry, example:  `blue.thepragmaticloud.com`
    1. Create 1 A record in route53 mapping the other EC2 instance to a DNS entry, example:  `green.thepragmaticloud.com`
    1. Create 2 weighted CNAME record entries with name: `prod.thepragmaticloud.com` pointing to `blue.thepragmaticloud.com` with weight 1 and `green.thepragmaticloud.com` with weight 0, add a low TTL and switch the weights.

