## What is Deep Racer?
- 1:18 scale model car
- Parts:
    - Camera
        - 4 mega pixel with MJPEG
        - HD
        - Used to see what is immediately in front of the car on the track
        - Very similar to DeepLens Camera
    - Small Intel computer
        - CPU
            - Dual Core Intel Atom Processor
    - Software
        - Ubuntu OS 16.04.3 LTS
    - Batteries
        - Computer battery
        - Wheels battery
    - Sensors:
        - Accelerometer
        - Gyroscope
            - Direction and Orientation
    - Suspension
    - USB port
        - plugin to load model
        - USB-A end in computer and USB-C end into the car
- deepracer.aws
    - Select wifi and type pwd
    - Enter the pwd at bottom of car
    - Calibration
        - Steer
        - Speed
        - Direction
- The model can be uploaded by the web interface
- We have to teach the car to drive
- Teaching process:
    - Reinforcement Learning
    - Output of process:
        - Model
            - Analogous to the brain of the car
            - Can be loaded onto the car
- Deep Racer League
    - Part promotional vehicle
    - Part crowdsourcing platform
    - Top parties will be invited to raise at Reinvent season finale

## Machine Learning
- It is really just math
- Parts:
    - Model
        - Responsible for estimating and predicting things
        - To create a model we need the following input:
            - Data 
            - Computation
            - Algorithm
                - Instructions on how to analyze the data
            - Feedback
                - Way to teal if a model is doing a good job
- Different Types:
    - Supervised ML
        - We feed example data to the algorithm and let it reverse engineer a math formula from samples
    - Unsupervised ML
        - Algorithms analyze data directly without any training step
    - Reinforcement Learning
        - We create a reward system that will incentivize the learning algorithm to repeat desired behaviour or reduce the undesired behaviour
        - Repeats scenarios over and over again trying to figure out how to maximize reward or minimize reward loss
        - Eventually reward process develops into a policy
            - Ex:  What the deepracer uses to decide how to maneuver around track
        - Reward function or system dimension examples:
            - Ex:  
                - Incentivize to stay in track center
                - Penalize it for going too slow
        - Trial and error process
            - Needs lots of iterations and data to build a good policy

## Training Process
- We have to train it using a Policy
- Markov Decision Process
    - Framework for reinforcement learning
    - Parts:
        - Agent:
            - In this case it is the car
        - Environment:
            - Track
        - State:
            - Current status of the car
        - Observation:
            - View from the onboard camera
        - Action:
            - Steer or just speed
        - Reward:
            - Driver for the reinforcement learning process
- Reward Function:
    - Snippet of code that is called with any Step or Action
    - Input:
        - State
        - Observations
    - Output:
        - Rewards
    - We can control actions
    - We can adjust hyper parameters
- Models:
    - Self-motivator
- Randomness:
    - Entropy
    - Different models may train differently due to this

## Hyperparameters
- We can choose different tracks which will provide different experiences for the training process
- We have our choice of `Action Space` where we can choose speed and steering
- We also have hyperparameters which are dials and knowbs to adjust the training process
- Similar to a sound engineer that adjusts to room, instruments, artist, etc.
- Typical cycle:
    - The agent (car) is in its environment (track)
    - The thing we're trying to train (policy) will take an image from the onbard camera
    - The policy will decide what action to do next
    - Once the car selects and action everything goes through the reward function to give us some sort of `award`
    - The training process keeps up with this reward as well as the observation, state and action taken and writes it into a little journal
        - This process is called an episode
        - Episodes repeat until we have a nice set of data on which to train
        - The episodes are tracked into a journal or experience buffer
- Hyperparameter elements:
    1.  Number of episodes
        - The more episodes the more data the policy process has to go on
        - More episodes also mean a longer duration for training
    2.  Gradient descent Batch size
        - Larger batch sizes mean more stable learning but it also slows down the learning process
    3.  Epoch
        - One pass over the batches
        - The more times the more stable the updates will be but will slow down the learning process
    4.  Discount factor
        - How far into the future the algorithm looks
        - 0.9 will look at 10 steps into the future
        - Looking too far into the future can be overwhelming
    5.  Loss function
        - Way to know if we're headed in the right direction
        - 2 types:
            1. Mean Square Error
                - Might be too aggressive
            2. Huber
                - Tends to be more subtle
    6.  Entropy
        - Trade-off between exploit and explore
        - Another word for randomness
    7. Learning rate
        - Amount of change
        - Tiny vs big steps
    7.  Time
        - How much should you train your model?
        - Recommendation is to train models in 1 hour increments

## Reward functions from Scott Pletcher
- https://github.com/scottpletcher/deepracer







        

