# Future of SLAM {# future_of_slam status=draft}

Assigned: SLAM team


## The SLAM problem
Problem description
Definition
SLAM is about doing two parallel processes in a way that solving one needs to tackle the other. To make it more clear, consider a scenario, in Duckievania… Countduckula poisoned poor Yakkydoodle and throw it away to an unknown location. Yakkydoodle wakes up, and needs to find its way home. To do that, it needs to do two things. First, Yakkydoodle thinks, “what is around me?”. Once it understands this, it thinks “where am I?”. Only when it knows both these things, it can (even hope to) find its way back. 

The process of answering the “what is around me?” question is called mapping. And, the “where am I?” question is answered by a process called localization. TRANSITION Robots need to do both these processes, to solve any meaningful task in an unknown environment. 

To know your current position in the world,  you need to do localization. Which needs  a map and to build a map you need to localize yourself 
SLAM finds applications in all scenarios in which a prior map is not available and needs to be built. 
There are several variants of SLAM. We are going to focus on the most popular one: Visual SLAM. Visual SLAM is the process of performing SLAM using vision sensors as primary inputs. There are a variety of vision sensors around: monocular cameras, stereo cameras, depth cameras, and laser scanners.

## Visual SLAM 




robots needs to estimate their location with respect to objects in their environment
-> chicken and egg
Core idea of SLAM

## SLAM today

Visual SLAM

### Cameras typically used

### Examples of successes

## Issues with current solutions
### Robustness

### Scalability

### Representation and understanding

## The road to Spatial AI

### What is Spatial AI

### Differences with Geometric SLAM

### Recent works towards Spatial AI
