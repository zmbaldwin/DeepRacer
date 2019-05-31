This is one of my reward functions is used at the Chicago AWS Summit.
It did not do to well.
However, I do no think it is because of the reward function itself. 
I think that i over trained the model. 
I trained this model for about 20 hours.
With the benefit of hind sight I see that there is such a thing as over training. 
Everytime I retrained it a would fiddle with the Hyperparameters without fully understanding the consiquences. 
We all have to start somewhere. This was my first model and the model I learned the most from.

Action Space:
22.5 degree and 4m/s 
This gave the model 3 action options

Hyperparameters:
By the last close all Hyperparameters were back to default execpt the episodes were at 25.

def reward_function(params):
    
    # Read input parameters
    all_wheels_on_track = params['all_wheels_on_track']
    distance_from_center = params['distance_from_center']
    track_width = params['track_width']
    steering = abs(params['steering_angle'])
    
    # Give a very low reward by default
    reward = 1e-3
    ABS_STEERING_THRESHOLD = 22.5

    # Give a high reward if no wheels go off the track and
    # the agent is somewhere in between the track borders
    if all_wheels_on_track and (0.5*track_width - distance_from_center) >= 0.05 and (steering < ABS_STEERING_THRESHOLD):
        reward = 5.0
    elif all_wheels_on_track and (0.5*track_width - distance_from_center) <= 0.05:
        reward = 0.5
    elif not all_wheels_on_track:
        reward = -5.0

    # Always return a float value
    return float(reward)
