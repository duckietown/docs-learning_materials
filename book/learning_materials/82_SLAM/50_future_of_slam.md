# Future of SLAM {# future_of_slam status=draft}

Assigned: SLAM team


## The SLAM problem
SLAM is a process which appears in a context where you have a robot in an unknown environment.
In many cases, a meaningful task would need for the robot to navigate and interact with the environment. Such actions often make use of a type of map of the environment as well as an estimation of the robot's position within the environment.

To do so first requires representing the environment around the robot through a _mapping_ process. As the robot moves around, it will also need to perform a process called _localization_.

TODO: 2 figures illustrating mapping and localization

These two processes depend on each other, and have to be performed simultaneously and continuously through the duration of the task, leading to _*S*imultaneous *L*ocalization *A*nd *M*apping_.

Seeing that the mapping process makes use the estimated position, and that the position is estimated with respect of the built map, this problem is hard and can be seen as a "chicken and egg" problem.

SLAM relies on the idea that it is necessary to make use of external information combined with internal states estimation to perform well. It involves making multiple observations on the environment and fusing all of them, to build a consistent global map and correct various estimation drifts and errors.

TODO: videos of duckiebot lane following with/without external information
## SLAM today
There are several variants of SLAM. We are going to focus on the most popular one: Visual SLAM. Visual SLAM is the process of performing SLAM using vision sensors as primary inputs.

### Cameras typically used
Visual SLAM usually relies on different types of cameras.
There are a variety of vision sensors around, which output different types of informations, here are a few examples :

*Monocular cameras* are the typical cameras you can find in a photography or smart phone camera. They create 2D images of the environment.


<figure class="flow-subfigures">  
    <figcaption>Monocular cameras.</figcaption>
    <figure>
        <figcaption>Example of a monocular camera.</figcaption>
        <img style='width:14em' src="figures/monocular_camera.png"/>
    </figure>
    <figure>  
        <figcaption>Example of a video produced by a monocular camera.</figcaption>
        <iframe style='width: 20em; height:auto' src="https://www.youtube.com/embed/_rF2eOOEzEw" frameborder="0" allowfullscreen="true"></iframe>
    </figure>
</figure>


*Stero cameras* are cameras which make use of two lenses, similarly to what humans do. They are able to deduce depth from the two images taken and thus output 3D images.


<figure class="flow-subfigures">  
    <figcaption>Stereo cameras.</figcaption>
    <figure>
        <figcaption>Example of a stereo camera.</figcaption>
        <img style='width:14em' src="figures/stereo_camera.png"/>
    </figure>
    <figure>  
        <figcaption>Example of a video produced by a stereo camera.</figcaption>
        <iframe style='width: 20em; height:auto' src="https://www.youtube.com/embed/AFH2yN3rM78" frameborder="0" allowfullscreen="true"></iframe>
    </figure>
</figure>

*RGB-D cameras* are cameras which usually make use of a depth sensor in addition to the lens. By doing so, they are also able to produce 3D images.

<figure class="flow-subfigures">  
    <figcaption>RGB-D cameras.</figcaption>
    <figure>
        <figcaption>Example of an RGB-D camera.</figcaption>
        <img style='width:14em' src="figures/rgbd_camera.png"/>
    </figure>
    <figure>  
        <figcaption>Example of a video produced by an RGB-D camera.</figcaption>
        <iframe style='width: 20em; height:auto' src="https://www.youtube.com/embed/3oKVSe5pZhY" frameborder="0" allowfullscreen="true"></iframe>
    </figure>
</figure>

*LIDAR sensors* make use of pulsed laser lights to measure free distances around the environment. They create a 3D representation of the environment but have no RGB information.

<figure class="flow-subfigures">  
    <figcaption>LIDAR sensors.</figcaption>
    <figure>
        <figcaption>Example of LIDAR modules.</figcaption>
        <img style='width:14em' src="figures/lidar.png"/>
    </figure>
    <figure>  
        <figcaption>Example of a video produced by LIDAR module.</figcaption>
        <iframe style='width: 20em; height:auto' src="https://www.youtube.com/embed/KxWrWPpSE8I" frameborder="0" allowfullscreen="true"></iframe>
    </figure>
</figure>

These sensors are the most popular in current SLAM systems, even though other types (event-based cameras) for example exist and might be useful in future SLAM systems.


### Examples of successes

## Issues with current solutions
We have seen in the previous part that in several cases, we had SLAM systems performing very well and achieving tasks efficiently.
These tasks were all very specific, in some restricted environment with some task specifications. However in general we argue that SLAM isn't fully solved, and present several aspects we believe are still not where they should be.

### Theoretical results
Today when we have a SLAM system, we rely on quantitative results on some experiments to evaluate the system. We rarely have theoretical guarantees on how it actually performs.
Usually, SLAM systems are designed to be used in unknown environments, meaning that we would have no guarantees on the performance in an actual use case. Some guarantees on minimal performance, or optimization reaching optimums would be important for real life large scale deployment.

### Robustness
We want SLAM systems to be robust. Being that most environments they are likely to operate in are dynamic, as illustrated in the figures below :

TODO: Image of same place winter vs summer

One same place can look very different visually depending on when it is seen. One dynamic effect is the change of winter, another one is the simple fact that some things can move. SLAM systems should therefore be able to rely on features which are more likely to be present and recognizable over time.
Currently, they mostly rely on geometric and visual features, which would completely fail to recognize the scenes shown here.

Not only would the perception module fail with dynamic environments, but also the back end.
In fact, in the case that an error in recognition is made (one place recognized as being the same when it is actually a different one), the optimization produces a very considerable error in the trajectory estimate.

In the video below, you can see the image from a camera, as well as two estimated trajectories. Both trajectories are estimated with a state of the art SLAM optimizer (g2o), used in many popular solutions. In the top trajectory, no data association is made, and the estimation is very good. However in the bottom trajectory, three random association errors are added over time. We can see that even one error affects badly the whole estimate, which quickly becomes completely wrong.

TODO: video of optimization failing


### Scalability
Scalability is extremely important for systems to be able to work over long durations and distances. Some tasks such as autonomous driving could require systems to meet those requirements.
Two issues arise in current SLAM solutions which prevent them from being scalable: *memory* and *computation*.

*Example of a problem:*
Imagine you have a truck driving to deliver the weekly bread crumb supplies to the cities of Duckingdom.
TODO: Image of the problem

A typical truck travels 4000km per week.
Assume your SLAM system requires storing an image every 10m, and that a stored RGB image has dimensions 640x480p.
This leads to the use of 400 000 images and 368.64GB of images per week of driving. This is an issue since today's SLAM algorithms need those images available in active memory and involved in computations, which is not doable.

### Representation and understanding
While robustness and scalability might improve over time thanks to the advancements in various SLAM tools (deep learning, computer power and memory, optimization techniques...), one issue will require rethinking some parts of current solutions.

You can see here how a modern SLAM system represents the environment:
TODO: Video of orbslam

Note how the SLAM map differs from what a human would see in an image. The SLAM system sees lines and points which appear salient visually, while the human would recognize objects and how they are arranged.
One core thing the SLAM system is missing is that it doesn't *understand* what it is seeing.
Not only could understanding help make the representation more compact (leading to a more scalable system), it would make the map more meaningful (and possibly make the system more robust), and make the systems more intelligent.

Here are examples of cases where having some kind of understanding of what is around can help SLAM:

TODO: Figures showing the images from the slides and legends describing what would help the SLAM system in the image


## The road to Spatial AI
As we have seen, current SLAM solutions have some issues, and SLAM still needs some work on. A recent paper from A. Davison (TODO cite paper properly) introduced a new term: "*Spatial AI*". This paper describes considerations on where SLAM should be heading, and what we should aim at having.

### What is Spatial AI
Even though "Spatial AI" is a new term, it is not far from being simply a rebranding of what is also called "Semantic SLAM".
TODO: Image of the wall crack
AI usually leads to building good representations. Similarly, the core idea of Spatial AI is to have the right spatial representations about the space in which the robot operates. Spatial AI superseeds SLAM in the sense that it is more general than simple "localization and mapping".

Here is an example of a situation where Spatial AI would be useful :

TODO: video of roomba + explanation.


### Differences with Geometric SLAM
On different levels Spatial AI differs with geometric SLAM.
We resume the main differences in the following table :

TODO: Table from slides. Maybe detail a bit more some points under ?

### Recent works towards Spatial AI

TODO: All videos from the slides + small explanations with each of them
