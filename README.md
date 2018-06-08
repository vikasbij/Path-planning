# CarND-Path-Planning-Project
Self-Driving Car Engineer Nanodegree Program

### Introduction

The goal of this project is to build a path planner that creates smooth, safe trajectories for the car to follow. The highway track has other vehicles, all going different speeds, but approximately obeying the 50 MPH speed limit.
The car transmits its location, along with its sensor fusion data, which estimates the location of all the vehicles on the same side of the road.
The path planner outputs a list of x and y global map coordinates. Each pair of x and y coordinates is a point, and all of the points together form a trajectory. We can use any number of points that we want, but the x list should be the same length as the y list. Every 20 ms the car moves to the next point on the list. The car's new rotation becomes the line between the previous waypoint and the car's new location.

Here is the data provided from the Simulator to the C++ Program

#### Main car's localization Data (No Noise)

["x"] The car's x position in map coordinates

["y"] The car's y position in map coordinates

["s"] The car's s position in frenet coordinates

["d"] The car's d position in frenet coordinates

["yaw"] The car's yaw angle in the map

["speed"] The car's speed in MPH

#### Previous path data given to the Planner

//Note: Return the previous list but with processed points removed, can be a nice tool to show how far along
the path has processed since last time. 

["previous_path_x"] The previous list of x points previously given to the simulator

["previous_path_y"] The previous list of y points previously given to the simulator

#### Previous path's end s and d values 

["end_path_s"] The previous list's last point's frenet s value

["end_path_d"] The previous list's last point's frenet d value

#### Sensor Fusion Data, a list of all other car's attributes on the same side of the road. (No Noise)

["sensor_fusion"] A 2d vector of cars and then that car's [car's unique ID, car's x position in map coordinates, car's y position in map coordinates, car's x velocity in m/s, car's y velocity in m/s, car's s position in frenet coordinates, car's d position in frenet coordinates. 

## Details

1. The car uses a perfect controller and will visit every (x,y) point it recieves in the list every .02 seconds. The units for the (x,y) points are in meters and the spacing of the points determines the speed of the car. The vector going from a point to the next point in the list dictates the angle of the car. Acceleration both in the tangential and normal directions is measured along with the jerk, the rate of change of total Acceleration. The (x,y) point paths that the planner recieves should not have a total acceleration that goes over 10 m/s^2, also the jerk should not go over 50 m/s^3. (NOTE: As this is BETA, these requirements might change. Also currently jerk is over a .02 second interval, it would probably be better to average total acceleration over 1 second and measure jerk from that.

2. There will be some latency between the simulator running and the path planner returning a path, with optimized code usually its not very long maybe just 1-3 time steps. During this delay the simulator will continue using points that it was last given, because of this its a good idea to store the last points you have used so you can have a smooth transition. previous_path_x, and previous_path_y can be helpful for this transition since they show the last points given to the simulator controller with the processed points already removed. You would either return a path that extends this previous path or make sure to create a new path that has a smooth transition with this last path.

##Rubric Points explaination:

The code compiled correctly as per the reference taken form the Project Walkthrough and Q&A I used the  spline.h file into the src folder and included it in the code which was later used for smoothening the generated waypoints. 

After implementing the code complete code i was able to run the simulator car for a minimum of 4.32 miles eventhough I tested it for 10 miles and it ran successfully without any incident.

The car  drove within the legal speed limits and never reached beyond the speed limit of 50 mph.The car moved with a collision and without any incident such as exceeded max acceleration and jerk.

The car stays in its lane, except for the time between lanes switching.

##Model Documentation:

We are getting the required parameters such car's location, velocity, yaw rate, speed, frenet coordinates and sensor fusion data from the simulator, using which we are taking into account several features using which we computing several computational values.we have used spline.h for polynomial fitting and smoothening (I have reffered to Project Walkthrough and Q&A and used  spline.h)


##Prediction and behaviour planning
The recieved data i.e car's location, velocity, yaw rate, etc is used to predict the behaviour of the other vehicles. As prediction we are predicting where the other vehicle will be in a future time and using that prediton we are planning  path and poistion of our simulator car

As a behaviour planning i have implemented a basic logic just like human drivers while changing lanes the things that we keep in mind are weather there is any slow speed vehicle in front of me if yes then i should try to switch lanes and get pass that slow moving vehicle for that the necessary thing is that there should not be any vehicle moving in the other lanes or they must be at an adiquate distance so that there is no collision.

So, considering these steps only i have implemented the following steps:

1. If there is a vehicle 30m ahead of my car i should speed down my vehicle.
2. Check for vehicles in the other lanes.
3. If in the other lanes there is a vehicle 30 m ahead and behind my current location then i should not change the lane as it may result in a collision.
4. If there is no vehicle then choose the fastest moving lane.

##Trajectory Generation

To compute the best trajectory I used car speed, the speed of surrounding cars, current lane, intended lane and previous points. For making trajectory smoother we add immediate two points from previous trajectory. If there are no previous points then we use yaw rate and current car coordinates to compute previous points. Then we add 3 points at 30, 60, 90 meters to the trajectory. To make mathematics bit easier we shift all the points car coordinate system and after processing, we convert them back to map coordinate system before passing those to the car.(Reference taken from project walkthrough and Q&A)