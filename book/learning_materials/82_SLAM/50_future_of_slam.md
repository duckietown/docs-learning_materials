# Future of SLAM {# future_of_slam status=draft}

Assigned: SLAM team


## The SLAM problem
SLAM is a process which appears in a context where you have a robot in an unknown environment.
In many cases, a meaningful task would need for the robot to navigate and interact with the environment. Such actions often make use of a type of map of the environment as well as an estimation of the robot's position within the environment.

To do so first requires representing the environment around the robot through a _mapping_ process. As the robot moves around, it will also need to perform a process called _localization_.

These two processes depend on each other, and have to be performed simultaneously and continuously through the duration of the task, leading to _*S*imultaneous *L*ocalization *A*nd *M*apping_.

Seeing that the mapping process makes use the estimated position, and that the position is estimated with respect of the built map, this problem is hard and can be seen as a "chicken and egg" problem.

SLAM relies on the idea that it is necessary to make use of external information combined with internal states estimation to perform well. It involves making multiple observations on the environment and fusing all of them, to build a consistent global map and correct various estimation drifts and errors.

TODO: videos of duckiebot lane following with/without external information
## SLAM today
There are several variants of SLAM. We are going to focus on the most popular one: Visual SLAM. Visual SLAM is the process of performing SLAM using vision sensors as primary inputs.

### Map types ???

### Cameras typically used
There are a variety of vision sensors around, which output different types of informations.

: monocular cameras, stereo cameras, depth cameras, and laser scanners.
### Examples of successes

## Issues with current solutions
### Robustness

### Scalability

### Representation and understanding

## The road to Spatial AI

### What is Spatial AI

### Differences with Geometric SLAM

### Recent works towards Spatial AI
