This is the fourth place algorithm that I ran at the Chicago AWS Summit. 
All day I had been runnig complicated models that had trained for hours on end. 
By the time my last run of the day came around I was in risk of losing my top 10 finish.
So, I went back to basics and swung for the fences with the last model on my USB stick.
It was a first gen model that had only trained for two hours. 
When it started around the track it was doing well. 
I picked up the pace and ended with a 10.930!!
Good enough for four place and my own DeepRacer. 

Action items:
20 degrees and 3m/s 
This gave the car three actions

I increased the batch size to 128, the entropy to 0.05 and the learing rate to 0.00003.

I lowered the learing rate to allow for smooth clean learning instead of a rapid caotic learing process.

Reward Algo
def reward_function(params):
   
    # Read input parameters
    all_wheels_on_track = params['all_wheels_on_track']
    distance_from_center = params['distance_from_center']
    track_width = params['track_width']
    marker = 0.5 * track_width
    
    # Give a very low reward by default
    reward = 1e-3

    # Give a high reward if no wheels go off the track and
    # the agent is somewhere in between the track borders
    if all_wheels_on_track:
        reward = 2.0
    elif distance_from_center <= marker:
        reward = 1.0
    else:
        reward = 1e-3

    # Always return a float value
    return float(reward)
