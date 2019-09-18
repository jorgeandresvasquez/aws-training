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
- Waypoints
    - parameters:
        - ```waypoints```
    - Ordered list of milestones placed along the track center
    - Each waypoint is a pair[x, y] of coordinates in meters
- Track width
    - parameters:
        - ```waypoints```
    - width of the track in meters
- Distance from center line
    - parameters:
        - ```distance_from_center```
            - measures the displacement of the vehicle from the center of the track
        - ```is_left_from_center```
            - boolean describing whether the vehicle is to the left of the center line of the track
-  All wheels on track
    - parameters: 
        - ```all_wheels_on_track```
            - boolean (true/false) which is true if all 4 wheels of the vehicle are inside the track borders 
- Speed
    - parameters:
        - ```speed```
    - Observed speed of the vehicle measured in m/s
- Steering angle
    - parameters:
        - ```steering_angle```
    - steering angle of the vehicle measured in degrees
    - negative means turning to the right
    - positive means turning to the left
- Progress
    - percentage of track completed
- Steps
    - Number of steps completed.  A step corresponds to an action taken by the vehicle following the current policy.


## References
- https://github.com/aws-samples/aws-deepracer-workshops/blob/master/Workshops/2019-AWSSummits-AWSDeepRacerService/Lab1/Readme.md
- https://www.aws.training/Details/eLearning?id=32143
