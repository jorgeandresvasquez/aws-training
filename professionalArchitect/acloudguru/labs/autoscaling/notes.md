## Problem Statement
- You are consulting with a pharmaceutical company new to AWS.  Every night, they have a series of calculations they run against data gathered by the day's research.
- Some days they have lots of data and some days they only have a little data to process.  They have some knowledge about scalability on AWS but are skeptical that it will work for them.  
- Since they are scientists, they want objective proof the scaling works.  Design and conduct an experiment to show how scaling works in the most cost-effective way.

## Tasks
1. Design an experiment to demonstrate austoscaling to the scientists using cost-effective methods
2. Build the scenario that simulates heavy computational load to trigger scale out

## Solution Steps
- Tip:  Use the `stress` command line linux utility to generate artificial load on a linux EC2 instance and force the system to scale:  http://manpages.ubuntu.com/manpages/eoan/man1/stress.1.html
    - Ex:  ```$ stress --cpu 8 --timeout 15s``` (will run for 15 seconds)
        - Use ```top```command to monitor the resources and validate the command outcome
1. Create a launch configuration for all EC2 instances in the ASG
    - Use the following as start-up script:
    ```bash
    #! /bin/bash
    yum update -y
    yum install stress -y 
    /usr/bin/stress --cpu 4 --timeout 2m
    ```
    - Check option:  `Enable Cloudwatch detailed monitoring`
2.  Create an ASG using the previous launch configuration
    - Check option:  `Enable Cloudwatch detailed monitoring`
    - Use a dynamic scaling poliy to adjust between 1 and 3 instances
        - 60% Average CPU Utilization
        - Instances need 60 seconds to warm up after scaling