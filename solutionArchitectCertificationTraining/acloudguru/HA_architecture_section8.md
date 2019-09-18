## Load Balancer Theory
- 3 Load Balancer Types:
    1. Application Load Balancer
        - Best suited for balancing of http and https traffic
        - Operate at layer 7 and are application-aware
        - Intelligent, you can create advanced request routing
        - Ex:  Change of language can be identified and Load Balance across servers in specific language
        - Can see individual html
        - Uses target groups registered as listeners
    2. Network Load Balancer
        - Best suited for load balancing of TCP traffic where extreme performance is required
        - Operates at connection level (OSI layer 4)
        - Capable of handling millions of requests per second
    3. Classic Load Balancer
        - Legacy ELBs
        - Support certain layer 7-specific features such as:  X-Forwarded and sticky sessions
            - X-Forwarded-For header:  IPv4 address of your end user
        - If application stops responding it responds with a 504 error (Gateway Timeout Error)
            - Troubleshoot the app and see if it is web server or DB layer
        - Cheapest option
- Load Balancers can be internet-facing or internal
- Instances monitored by ELB are reported as "InService" or "OutofService"
- Health checks check the instance by talking to it
- Load Balancers have their own DNS name.  You are never given an IP address
- Sticky Sessions
    - Allow you to bind a user's session to a specific EC2 instance.  This ensures that all requests from the user during the session are sent to the same instance 
    - Can be enabled for Classic Load Balancers and ALBs, but for ALBs the traffic will be sent at the Target Group Level
- Cross Zone Load Balancing
    - Allow ELBs to send traffic to other AZs
- Path Patterns
    - Allow you to create a listener with rules to forward requests based on the URL path
    - Only supported by ALBs
- If the load balancer is not responding to requests, check for the following:
    - Verify that you specified public subnets for your load balancer.  A public subnet has a route to the Internet Gateway for your VPC.
- An ELB can help you deliver stateful services, but not stateless

## ALB
- You cn use the following features to monitor your ALBs:
    - Cloudwatch metrics
    - Access logs
        - Elastic Load Balancing provides access logs that capture detailed information about requests sent to your load balancer. 
    - Request Tracing
        - You can use request tracing to track HTTP requests from clients to targets or other services. When the load balancer receives a request from a client, it adds or updates the X-Amzn-Trace-Id header before sending the request to the target.
    - Cloudtrail logs

## Target Group
- Your ELB will route requests to the targets in the target group
- When you create a target group, you specify its target type, which determines the type of target you specify when registering targets with this target group. After you create a target group, you cannot change its target type.
    - The following are the possible target types:
        - instance
        - ip
        - lambda

## Launch Configurations
- A launch configuration is an instance configuration template that an Auto Scaling group uses to launch EC2 instances.
- Launch configurations cannot be modified after creation. If you need to make a change, create a new launch configation and update your auto scaling group to use it.
- Launch configurations can belong to multiple Auto Scaling groups, however you can only specify one launch configuration at a time for an Auto Scaling group.

## Autoscaling Groups
- When you delete an ASG the instances beneath it get deleted as well
- You can only specify one launch configuration for an Auto Scaling group at a time
- You can't modify a launch configuration after you've created it
- Whenever you create an Auto Scaling group, you must specify a launch configuration, a launch template, or an EC2 instance
- Adding Lifecycle Hooks to Auto Scaling group puts the instance into wait state before termination. During this wait state, you can perform custom activities to retrieve critical operational data from a stateful instance. Default Wait period is 1 hour.
- Scaling Options:
    - Maintain Current instance levels at all times
    - Manual Scaling
    - Scale based on a schedule
        - adjusting the size of a group at a specific time
    - Scale based on demand (Dynamic)
        - Automatically adjust the size of the group based on a specificed increase in demand
    - Predictive scaling
- The cooldown period helps to ensure that your Auto Scaling group doesn't launch or terminate additional instances before the previous scaling activity takes effect. You can configure the length of time based on your instance warmup period or other application needs.
    - With a cooldown period in place, the Auto Scaling group launches an instance and then suspends scaling activities due to simple scaling policies or manual scaling until the specified time elapses. (The default is 300 seconds.) This gives newly launched instances time to start handling application traffic. After the cooldown period expires, any suspended scaling actions resume.
- You can attach a load balancer to your Auto Scaling group. The load balancer automatically distributes incoming traffic across the instances in the group.
- Default metric types for autoscaling group policies:
    - ASGAverageCPUUtilization
    - ASGAverageNetworkIn
    - ASGAverageNetworkOut
    - ALBRequestCountPerTarget
    NOTE:  You can choose other available Amazon CloudWatch metrics by specifying a customized metric.
- You control when an AG adds instances (referred to as scaling out) or removes instances (referred to as scaling in) from your network architecture.
- Default Termination Policy:
    - Default option used by an Auto Scaling Group when a scale-in event occurs
    - Designed to help ensure that your instances span Availability Zones evenly for high availability.
- Health Check Grace Period:
    - Frequently, an Auto Scaling instance that has just come into service needs to warm up before it can pass the health check. Amazon EC2 Auto Scaling waits until the health check grace period ends before checking the health status of the instance. Amazon EC2 status checks and Elastic Load Balancing health checks can complete before the health check grace period expires. However, Amazon EC2 Auto Scaling does not act on them until the health check grace period expires. To provide ample warm-up time for your instances, ensure that the health check grace period covers the expected startup time for your application. If you add a lifecycle hook, the grace period does not start until the lifecycle hook actions are completed and the instance enters the InService state.
- AutoScaling Group Termination Policy:
    - ![Autoscaling Group Termination Policy](images/auto-scaling-group-termination-policy.png)

## HA Architecture
- Everything fails, you need to plan for failure
- Simian Army from Netflix
    - https://medium.com/netflix-techblog/the-netflix-simian-army-16e57fbab116
- Use Multiple AZs and mulltiple regions wherever you can
- Know difference between Multi-AZ and Read Replicas for RDS
- Scaling Up
    - Increase resources inside EC2 instance (RAM, CPU)
- Scaling out
    - We add additional EC2 instances using ASG
- Always consider the cost element of scenarios
- Know the different S3 storage classes
    - Standard S3 and Standard S3 IA are both highly available, the one that isn't is S3 IA One Zone
- Load balancers are regional service i.e. they operate within an AWS region
- Route 53 is global service i.e. they operate independent of region
- Route 53 has routing policy to route requests to different endpoints which are in different region.


## Cloudformation
- Change sets:
    - Contain a summary of proposed changes
    - allow us to preview how changes will impact to our resources
- Stack:
    - Set of resources created by a cloudformation template
- After you create a stack from a template, you can detect drift from the Console, CLI, or from your own code. You can detect drift on an entire stack or on a particular resource, and see the results in just a few minutes.
- CloudFormation is Amazon's IAC solution
- QuickStart is a bunch of Cloudformation templates already built by AWS Solutions Architects allowing you to create complex environments very quickly
- When you delete a stack all the resources associated to that stack get deleted by default as well
- AWS CloudFormation Drift Detection can be used to detect changes made to AWS resources outside the CloudFormation Templates. AWS CloudFormation Drift Detection only checks property values that are explicitly set by stack templates or by specifying template parameters. It does not determine drift for property values that are set by default. 
- Elements of a template:
    - Description
    - Resources
    - Outputs
    - Parameters
    - Conditions
    - Mappings
    - Transform

## Elastic Beanstalk
- With Elastic Beanstalk, you can quickly deploy and manage applications in the AWS Cloud without having to learn about the infrastructure that runs those applications.
- It automatically handles the details of capacity provisioning, load balancing, scaling, and application health monitoring
- To use Elastic Beanstalk, you create an application, upload an application version in the form of an application source bundle (for example, a Java .war file) to Elastic Beanstalk, and then provide some information about the application
- Aimed at developers who know very little about AWS
- You can configure it to have Load Balancer, change the instances, change capacity
- Resources are required to run the application 24/7, not for only at night or day.
- Elastic Beanstalk supports the deployment of web applications from Docker containers. With Docker containers, you can define your own runtime environment. You can choose your own platform, programming language, and any application dependencies (such as package managers or tools), that aren't supported by other platforms. Docker containers are self-contained and include all the configuration information and software your web application requires to run. All environment variables defined in the Elastic Beanstalk console are passed to the containers.

## HA Sample Wordpress Site
- 2 buckets
    - wp media
        - jorgevasquez-wp-media
    - wp code
        - jorgevasquez-wp-code
- Cloudfront
    - Web Distribution
- RDS (mySQL)
    - acloudguru
- IAM role to communicate with S3 buckets
    - S3AdminAccess
- EC2 instances
    - MyGoldenImage
- ELB
    - MyALBWP

## Other
- Resiliency:  can be described as the ability to a system to self heal after damage or an event.
- Reliability: is the probability that a system will work as designed.

## TODOs
- Read FAQs on ELBs
- Read AWS EC2 Autoscaling FAQs
    - https://aws.amazon.com/autoscaling/faqs/
- Take ELB and Autoscaling exam from whizlabs