# Visual Odometry Types {#visual-odometry-types: status=draft}

## VO vs V-SLAM

There are fundamental differences between the problems of Visual Odometry and Visual SLAM (Self-localization and Mapping) in terms of the scope and applicability.

Some noteworthy points in this regard are that :
* VO only aims to solve the **local consistency** of the trajectory 
* SLAM aims to solve the **global consistency** of the trajectory and of the map 
* VO can be used as a building block of SLAM 
* VO is SLAM before closing the loop.

## Types of Visual Odometry

#### Based on number of cameras  
* Monocular
* Stereo
	
#### Based on type of camera
* Perspective
* Spherical
* Omnidirectional (360° FOV)
* Time of Flight
* RGB-D

#### Based on when odometry is computed  
* Online
* Offline

#### Based on nature of approach
* Geometry-based
* Learning-based




