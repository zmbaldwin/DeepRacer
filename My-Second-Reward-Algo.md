This is my second attempt at a ML reward algorithm.
I trained this at the same time as my first one and ran into simular issues.

Action Space:
20 Degrees and 4m/s
Giving the model 3 action options

Hyperparameters:
Increase batch to 128
Entropy: 0.02
Experence Episodes: 25
Loss type: Mean Squared
All others default

def reward_function(params):
    
    # Read input parameters
    distance_from_center = params['distance_from_center']
    track_width = params['track_width']
    steering = abs(params['steering_angle']) # Only need the absolute steering angle
    all_wheels_on_track = params['all_wheels_on_track']

    # Calculate 3 markers that are at varying distances away from the center line
    marker_1 = 0.1 * track_width
    marker_2 = 0.25 * track_width
    marker_3 = 0.5 * track_width

    # Give higher reward if the agent is closer to center line and vice versa
    if distance_from_center <= marker_1:
        reward = 1
    elif distance_from_center <= marker_2:
        reward = 0.5
    elif distance_from_center <= marker_3:
        reward = 0.1
    else:
        reward = 1e-3  # likely crashed/ close to off track

    # Give a high reward if no wheels go off the track and
    # the agent is somewhere in between the track borders
    if all_wheels_on_track and (0.5*track_width - distance_from_center) >= 0.05:
        reward = 2.0
    elif all_wheels_on_track and (0.5*track_width - distance_from_center) <= 0.05:
        reward = 0.5
    elif not all_wheels_on_track:
        Reward = -1.0


    # Steering penality threshold, change the number based on your action space setting
    ABS_STEERING_THRESHOLD = 20

    # Penalize reward if the agent is steering too much
    if steering > ABS_STEERING_THRESHOLD:
        reward *= 0.8
