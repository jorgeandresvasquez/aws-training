## AWS Reference on ML
- https://d2k9g1efyej86q.cloudfront.net

## Parameters of Reward functions
- Position on track
    - parameters:
        - ```x```
        - ```y```
    - x, y coordinates measured from the lower-left corner of the environment
- Heading
    - parameters:  
        - ```heading```
    - orientation of the vehicle in degrees measured counter-clockwise from the X-axis
    - Range:   -180:180
- Waypoints
    - parameters:
        - ```waypoints```
    - Ordered list of milestones placed along the track center
    - Each waypoint is a pair[x, y] of coordinates in meters
    - [[float, float], ... ]
- Closest Waypoints
    - parameters:
        - ```closest_waypoints```
    - Indices of the 2 nearest waypoints
    - [int, int]
- Track width
    - parameters:
        - ```track_width```
    - width of the track in meters
- Distance from center line
    - parameters:
        - ```distance_from_center```
            - measures the displacement of the vehicle from the center of the track to the vehicle center
- Is Left of Center
    - parameters:
        - ```is_left_from_center```
            - boolean describing whether the vehicle is to the left of the center line of the track (true) or to the right (false)
-  All wheels on track
    - parameters: 
        - ```all_wheels_on_track```
            - boolean (true/false) which is true if all 4 wheels of the vehicle are inside the track borders 
- Speed
    - parameters:
        - ```speed```
    - Observed speed of the vehicle measured in m/s
    - range: 0:8
- Steering angle
    - parameters:
        - ```steering_angle```
    - steering angle of the vehicle measured in degrees from the center line of the vehicle
    - negative means turning to the right
    - positive means turning to the left
    - range:  -30:30
- Progress
    - parameters:
        - ```progress```
    - percentage of track completed
- Steps
    - parameters:
        - ```steps```
    - Number of steps completed.  A step corresponds to an action taken by the vehicle following the current policy.

## Reward function output
- The reward_function outputs the immediate reward (reward) at the specified step and the output is a number in the range of [-100000.0 : 100000.0].

## Notes from Fintech AWS DeepRacer Webinar _ Prep Work 
- Basics of meeting:    
    - Stephen Richardson
    - October 22-23
    - Success of the booth as well as deep racer
    - No need to attend on the 2nd session
    - 2 hours/ hands-on
    - Presenter:  Prem Ranga
    - Awareness of prep work required
- Intro to AWS RL and DeepRacer
    - Prem Ranga
        - Sr. Solution Architect for AWS
        - Simple Beer Service DEV
        - Aligned to AI/ML
- AWS DeepRacer Origin
    - AWS Sagemaker
    - How can we put RL in the hands of all developers?
    - Deep Lens is one aspect of the car, the car uses deep lens.
        - inference at the edge
    - Robocar rally
        - Autonomous car
            - used supervised learning
- 3 main components of deepracer
    - 1/18 autonomous race car
    - Virtual simulator, to train and evaluate
    - Global racing league
        - virtual leagues
        - Practice here to see how your model is doing  
            - make sure that the models perform well
- AWS Deepracer problem formulation
    - Model
    - Agent
        - The model gets deployed on the car
        - Car
    - Action
        - Steering
        - Speed
    - State
        - Snapshot of the track that the agent sees at any instant
        - Converted into gray scale
        - Sent to CPU where model is running
        - Picks action with highest probability
    - Environment
        - Track
    - Goal
- Machine Learnig Overview
    - Supervised Learning
    - Unsupervised Learning
        - Different algorithms than SL
        - Algorithms for pattern finding
        - Ex:  Fraud detection in credit cards
    - Reinforcement Learning
        - We reward the agent if it is doing well and penalize it if not
        - Ex:  Puppy is rewarded for positive behaviour

- Reward Function
    - Incentivizes particular behaviors and is at the core of RL
    - Python code
        - If it does this it will get rewarded otherwise it won't
    - Ex:  100 is max reward

- RL vs other approaches for robotic racing

- Once training is completed the Reward function is no longer applied

- Algorithm and Learning Framework
    - Clip PPO
    - Proximal Policy Optimization algorithm (PPO)
    - AWS DeepRacer integrates with Amazon SageMaker to make some popular deep-learning frameworks, such as TensorFlow, readily available in the AWS DeepRacer console

- Car
    - Gyroscope is not presently used

- Tips:
    - Look at Cloudwatch Logs
    - Max of 4 hours
        - Don't overtrain the model
    - Environment influences
        - Light of space
        - shadow
        - Drives with real car before racing
    - World record:
        - 7.6 seconds (Tokyo, Japan)
        - 7.6 seconds (USA)

- AWS DeepRacer League Logistics
    - Race Day
        - Track Boss
        - Pit Boss
            - Sometimes one per team
        - Time Keeper
        - Fueler
            - Helps with battery charging
            - Compute models
                - 4-5 hours
            - Recommend to have backup for batteries and recharge them immediately
            - Voltage meter to see how much is left per battery
    - 10-12 AWS staff to support teams  
    - Make sure to have a USB Flash Drive
    - Multiple models can be loaded in a car
    - Every team is provided 4 minutes
    - For every lap you can put back 4 times after leaving the track
    - Complete as many laps within 4 minutes
    - Score is the lap with the fastest score
    - 6 races per day for 2 days

- Last minute tweaks
    - TBD

- Hyperparameters
    - Parameters for the neural network itself
    - Tuned for reinvent track already

- Track
    - Dimensions:
        - 7.92m x 5.18m

- Reward
    - Objective is to maximize accumulated reward

- Questions:
    - Team structure ideal?
    - What does deepracer do under the hood?
    - Console simulator video seems to lag very often...
    - Training differs by track

- Cost
    - M4 instance
    - Estimated cost of $3.36

- Labs
    - https://github.com/aws-samples/aws-deepracer-workshops/tree/master/Workshops/2019-AWSSummits-AWSDeepRacerService/Lab0_Create_resources
    - https://github.com/aws-samples/aws-deepracer-workshops/tree/master/Workshops/2019-AWSSummits-AWSDeepRacerService/Lab1

- Action refresh rate
    - 10-12 hz
    
- Iterations
    - At each iteration you can introduce one or a few more sophisticated treatments to the reward function to handle previous ignored variables or you can
systematically adjust hyperparameters until the result converges.
    - As a general rule, train your model to be as robust as possible so that you can apply it to as many environments as possible. 

## Logs
- Command to download the logs locally:
    - ```aws logs get-log-events --log-group-name /aws/robomaker/SimulationJobs --log-stream-name sim-j40ggtmd2lx7/2019-09-26T16-14-45.887Z_3c32aff9-c53c-48a5-b94c-673723e3f68e/SimulationApplicationLogs```
- Simulation Trace Log format:
    - SIM_TRACE_LOG: episode, step, x-coordinate, y-coordinate, heading, steering_angle, speed, action_taken, reward, job_completed, all_wheels_on_track, progress,  closest_waypoint_index, track_length, time.time()

## Workflow
1. This all starts with creating a model in the AWS DeepRacer console. You use the console for model configuration. Once built, the simulator is used to train and then evaluate your model. The simulator is a great tool: you can see in real time how your model is responding to the settings you’ve chosen.

2. After your model is trained, you can evaluate its performance in the simulator, which yields the percentage of the lap complete for each of your runs, plus the overall time it took to complete each run.

3. If you like what you’ve done with your trained and evaluated model, you can clone it and start to tweak your configurations for increased performance.

4. Once you have a model that’s performing to meet your expectations, deploy it to the car and get started racing on the physical track, or compete with your model via virtual track events. 

## Findings
- 70 waypoints
- The waypoints output is always the exact same
- The current position does not correspond necessarily to a waypoint
- Plotting of waypoints in reinvent 2018 track:  `deepRacer/awsWorkshopsRepo/aws-deepracer-workshops/Workshops/2019-AWSSummits-AWSDeepRacerService/Lab1/img/reinventtrack_waypoints.png`
- A new episode starts whenever any of the following happens:
    - `all_wheels_on_track` is false
    - no progress over a couple of steps
- Each episode is made up of many steps which start with 0 at the beginning of each episode
- `action_taken` in the logs represent an index from the `model_metadata.json` (action space)

## Reinforcement Learning
- Main Components of an RL model:
    1. Agent
    2. Action
        - Types:
            - Continuous
                - Ex:  Fluctuating speed
            - Discrete
                - Ex:  Going left, going right, standing still
    3. Environment
    4. State
        - An action of the agent causes it to move from original state to a new state
        - Partial State:
            - Ex:  The agent sometimes cannot capture its complete positioning in regards to a track
    5. Reward
        - Scaler value
- Overtime the agent learns which actions to take in order to maximize rewards
- A `task` is the looping process between state in environment-->agent takes action-->Moves to new state-->environment gives a reward-->agent takes another action-->....
    - A task can be continuous (keeps going without end) or discrete (ends after certain time, often called an `episode`)
- The goal or RL algorithms is to maximize the sum of rewards
    - Discount factor
        - Helps the model determine the extent to which future rewards should contribute to the overall sum of expected rewards
        - Ex:  0, the agent will want to maximize the rewards in the short term, only cares about 1 obstacle at a time
        - Ex:  1, the agent cares about looking further ahead, learning which actions maximize the rewards over the course of the whole game
- Policy:
    - Mapping between the action and the states
    - Often takes the form of a lookup table
        - if this state take this action, if that state takes this other action
    - 2 ways to model a policy function:
        1. Stochastically
            - Includes a probability of taking an acion given a particular state
        2. Deterministically
            - Much more direct mapping between state and action
            - Usually denoted by the sympol:  pi
    - The goal of the training of RL is go learn the most effective policies to maximize rewards
    - Deep RL
        - Policy function is a neural network that will determine the actions to take
    - Convolutional Neural Network (CNN)
    - Value Function (V)
    - AWS Deep Racer uses the PPO (Proximal Policy Optimization) algorithm to train its models
        - Advantages
            - Very efficient
            - Easy to use
            - Stable
        - Uses 2 neural networks during its training:
            1. Policy Network
                - aka:  actor network
                - Decides which action to take given an image as input
            2. Value Network
                - aka:  critic network
                - estimates the cummulative results we are likely to get given the image as an input

## Parts of DeepRacer in AWS
- Sagemaker
    - Used to train the RL models
- Robomaker
    - Used to create a virtual environment for the agent to interact with
    - Uses gazebo to create the simulation
        - Gazebo is a physics agent
        - Simulates the laws of physics for each component
            - wheels
            - chassis
            - camera
                - field of view
            - collision
            - friction
- Sagemaker + Robomaker + Redis + S3
- The more training you do the better the model gets
- Hyperparameters
    - External to the model and set by you before training
    - You can use trial and error
- Parameters
    - Internal to the model and can be learned from training
- Reward Function
    - Setup the rewards
    - Primary code used to incentivize optimal actions. 
    - It’s a mechanism used by the environment to let the agent know how it’s doing.


## Terms
- episode:  is a period in which the vehicle starts from a given starting point and ends up completing the track or going off the track. It embodies a sequence of experiences. Different episodes can have different lengths.

## References
- https://github.com/aws-samples/aws-deepracer-workshops/blob/master/Workshops/2019-AWSSummits-AWSDeepRacerService/Lab1/Readme.md
- https://www.aws.training/Details/eLearning?id=32143pwd
- https://medium.com/proud2becloud/deepracer-our-journey-to-the-top-ten-257ff69922e
    - contains sample reward functions
- https://docs.aws.amazon.com/deepracer/latest/developerguide/deepracer-reward-function-input.html
- https://towardsdatascience.com/learning-to-drive-smoothly-in-minutes-450a7cdb35f4
- https://www.codementor.io/james_aka_yale/a-gentle-introduction-to-neural-networks-for-machine-learning-hkijvz7lp
- https://github.com/aws-samples/aws-deepracer-workshops/tree/master/Workshops/2019-AWSSummits-AWSDeepRacerService/Lab1
- https://www.aws.training/Details/eLearning?id=32143
    - Free Training on AWS
- https://docs.aws.amazon.com/deepracer/latest/developerguide/images/deepracer-track-guideline.png
- https://www.linkedin.com/pulse/first-experience-reinforcement-learning-aws-deepracer-heiko-lange/
    - Best article summarizing deepracer
- https://github.com/breadcentric/aws-deepracer-workshops/tree/enhance-log-analysis/log-analysis
- https://codelikeamother.uk/using-jupyter-notebook-for-analysing-deepracer-s-logs

## TODOs
- Read on Neural Networks





