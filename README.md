# Path-Planning

In this project, the goal is to design a path planner that is able to create smooth, safe paths for the car to follow along a 3 lane highway with traffic. The path planner keeps the car inside its lane, avoids hitting other cars, and passes slower moving traffic all by using localization, sensor fusion, and map data.

- I was able to drive the car in the simulator for 34.67 miles without any incident.

![best_without_incident](https://user-images.githubusercontent.com/25946127/35612461-b39ecb70-061d-11e8-9e02-963730b855b8.jpg)
![best_without_incident_2](https://user-images.githubusercontent.com/25946127/35612462-b3c69b3c-061d-11e8-9800-7ae887b88f8b.jpg)

- During this drive, the car  
i. did not exceed the max speed of 50mph.  
ii. did not exceed a total acceleration of 10 m/s^2 and a jerk of 10 m/s^3.  
iii. did not come into contact with any other cars on the road.  
iv. did not spend more than 3 seconds outside lane lines during lane changes and always stayed on the right side of the road.  
v. changed the lanes optimally to avoid slower traffic.  

Code Description: All the minor details in the code are written within the code as comments. Here, the overall concept of the code is explained.

a) Lines 259 - 309: Identifying other cars' positions and adjusting our lane to avoid them formed the core of this code. Lines 262-271 detail how the sensor fusion data is used to identify the lane of the neighbor cars based on their Frent co-ordinate 'd' value. Lines 273-277 is extracting their velocity and Frenet distance 's'.
Lines 280-287 required considerable finetuning to decide the range around our car that defines whether a car is to its right/left/ahead of it. All cars within 25m ahead and 20m behind were considered 'neighbors'. The lower 20m behind helped easily switch lanes and overtake cars during slower traffic. 
At the same time, any value below 20m could cause collisions with cars speeding up from behind.
Lines 289-309 describe our car's actions (lane change/slowing down/speeding up) based on the presence of cars around it. Mostly self-explanatory. It took some experimentation to get to the right max acceleration (a_max) value such that car doesn't experience any jerks or go beyond the permitted max acceleration.

b) Lines 319-340: This part of the code gets the last two waypoints from the previous path (if the previous path has more than 2 waypoints) and uses them to plan the current path to maintain smooth transition between each set of waypoints.

c) Lines 344-397 : This part of the code is the path generation for the car using Spline tool recommended in the Udacity lessons. This tool was used to interpolate the path between the known waypoints upto 30m ahead. The velocity is adjusted for every frame (0.02 seconds) to make sure it doesn't exceed the v_max but at the same time accelerate and go as fast as possible within the speed limit.
