# Future of SLAM {# future_of_slam status=draft}

Assigned: SLAM team


## The SLAM problem
SLAM is a process which appears in a context where you have a robot in an unknown environment.
In many cases, a meaningful task would need for the robot to navigate and interact with the environment. Such actions often make use of a type of map of the environment and at the same time use this map to compute it's own location.

To do so first requires representing the environment around the robot through a _mapping_ process. As the robot moves around, it will also need to perform a process called _localization_.

<figure class="flow-subfigures">  
    <figcaption>An example of mapping and localization</figcaption>
    <figure>
        <figcaption>Example of Mapping.</figcaption>
        <img style='width:20em; height:15em' src="figures/mapping.png"/>
    </figure>
    <figure>
        <figcaption>Example of Localization.</figcaption>
        <img style='width:20em; height:15em' src="figures/localization.png"/>
    </figure>
  </figure>

These two processes depend on each other, and have to be performed simultaneously and continuously through the duration of the task, leading to _*S*imultaneous *L*ocalization *A*nd *M*apping_.

Seeing that the mapping process makes use the estimated position, and that the position is estimated with respect to the built map, this problem is hard and can be seen as a "chicken and egg" problem.

SLAM relies on the idea that it is necessary to make use of external information combined with internal states estimation to perform well. It involves making multiple observations on the environment and fusing all of them, to build a consistent global map and correct various estimation drifts and errors. Here are some examples of the lane following demo by duckiebot with/without using external information from the map:

<figure class="Lane follwing demo with/without external information from the map">  
    <figcaption>1. without external information.</figcaption>
    <figure>
        <figcaption>Example of lane following demo without external info.</figcaption>
        <iframe style='width:20em; height:auto' src="https://www.youtube.com/embed/n2ZHlNfFjUo" frameborder="0" allowfullscreen="true"></iframe>
    </figure>
    <figure>  
        <figcaption>2. lane following demo with external info.</figcaption>
        <iframe style='width:20em; height:auto' src="https://www.youtube.com/embed/guU9QsTT2vM" frameborder="0" allowfullscreen="true"></iframe>
    </figure>
</figure>


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

These sensors are the most popular in current SLAM systems, even though other types exist and might be useful in future SLAM systems.


### Examples of successes stories of SLAM

*Kuka Navigation solution* is an autonomous transportation solution mostly used in warehouses (for example in Amazon's). This is one of the success stories up to today in which mapping a fixed and size limited 2D indoor environment with sufficient accuracy (less than 10cm) and sufficient robustness (say, low failure rate) while interacting with several humans and robots in a very controlled environment works well.
<figure class="flow-subfigures">  
    <figcaption>Kuka Navigation solution</figcaption>
    <iframe style='width: 20em; height:auto' src="https://www.youtube.com/embed/kN9a7W_hnSQ" frameborder="0" allowfullscreen="true"></iframe>
</figure>

*Mars Exploration Rovers* aims at exploring the Martian surface and geology of planet Mars,  and similarly vision-base SLAM with slowly moving robots and visual inertial odometry can be considered mature research fields. In this example and also the previous one, the evironment is predictable, we don't have highly dynamic objects.
<figure class="flow-subfigures">  
    <figcaption>Mars Exploration Rovers</figcaption>
    <iframe style='width: 20em; height:auto' src="https://www.youtube.com/embed/APGswfxKqEw" frameborder="0" allowfullscreen="true"></iframe>
</figure>


*Roomba* is an autonomous robotic vacuum cleaner sold by iRobot. Roomba can navigate the floor area of an initially unknown home (with limited size) and clean it. In this case the environment is mostly static. You can see how well it performs in the following video:

<figure class="flow-subfigures">  
    <figcaption>Roombe robotic vaccum cleaner</figcaption>
    <iframe style='width: 20em; height:auto' src="https://www.youtube.com/embed/hZFlrYMrKCE" frameborder="0" allowfullscreen="true"></iframe>
</figure>


*Autonomous Cars* self driving cars are not an absolute success story; however, we have seen many significant progress on autonomous vehicles in recent years. There are 6 different levels of driving automation:
	*Level 0* in this level there is no sustained vehicle control, only an automated system issue warnings (we've already have this feature in our cars)
	*Level 1 ("hands on")* the automated system and the driver share the control of the vehicle, expamples are Cruise Control, Parking assistance and lane keeping assistance (We also achieved these feature in the recent years)
	*Level 2 ("hands off")* the automated system takes full control of the vehicle but the driver must monitor the driving and be ready to intervene immediately at any time (this is simlar to a full cruise control so we have this as well)
	*Level 3 ("eyes off")* the driver can safely turn their attention away from the driving tasks, the vehicle will handle situations that call for immediate reponse, like emergency breaking. However, the driver must still be prepared to intervene whithin some limited time.
	*Level 4 ("mind off")* is similar to level 3 but no driver attention is required for safety. But the self driving is supported only in limited spatial areas or under special circumstances
	*Level 5 ("steering wheel optional"):* no human intervention is required at all (similar to a robotic taxi)


The last 3 levels are also achieved at some degrees but still lots of works to do since people are not confident enough to rely on a self driving cars in all situations sepcially when they are in a crowded street and needs more attention.

<br/>
<figure class="flow-subfigures">  
    <figcaption>Autonomous Cars</figcaption>
    <iframe style='width: 20em; height:auto' src="https://www.youtube.com/embed/xsQvq4WlUYU" frameborder="0" allowfullscreen="true"></iframe>
</figure>



## Issues with current solutions
We have seen in the previous part that in several cases, we had SLAM systems performing very well and achieving tasks efficiently.
These tasks were all very specific, in some restricted environment with some task specifications. However in general we argue that SLAM isn't fully solved, and present several aspects we believe are still not where they should be.

### Theoretical results
Today when we have a SLAM system, we rely on quantitative results on some experiments to evaluate the system. We rarely have theoretical guarantees on how it actually performs.
Usually, SLAM systems are designed to be used in unknown environments, meaning that we would have no guarantees on the performance in an actual use case. Some guarantees on minimal performance, or optimization reaching optimums would be important for real life large scale deployment.

### Robustness
We want SLAM systems to be robust. Being that most environments they are likely to operate in are dynamic, as illustrated in the figures below :

<figure class="flow-subfigures">  
    <figcaption> The same scene in summer vs winter</figcaption>
    <figure>
        <figcaption>Summer vs Winter</figcaption>
        <img style='width:20em; height:auto' src="figures/winter_summer.png"/>
    </figure>
    <figure>
        <figcaption>A scene with many moving entities.</figcaption>
        <img style='width:20em; height:15em' src="figures/dynamic_scene.png"/>
    </figure>
</figure>


One same place can look very different visually depending on when it is seen. One dynamic effect is the change of winter, another one is the simple fact that some things can move. SLAM systems should therefore be able to rely on features which are more likely to be present and recognizable over time.
Currently, they mostly rely on geometric and visual features, which would completely fail to recognize the scenes shown here.

Not only would the perception module fail with dynamic environments, but also the back end.
In fact, in the case that an error in recognition is made (one place recognized as being the same when it is actually a different one), the optimization produces a very considerable error in the trajectory estimate.

In the video below, you can see the image from a camera, as well as two estimated trajectories. Both trajectories are estimated with a state of the art SLAM optimizer (g2o), used in many popular solutions. In the top trajectory, no data association is made, and the estimation is very good. However in the bottom trajectory, three random association errors are added over time. We can see that even
one error affects badly the whole estimate, which quickly becomes completely wrong.



<figure class="flow-subfigures">  
    <figcaption>A video of optimization failing</figcaption>
    <iframe style='width: 20em; height:auto' src="https://www.youtube.com/embed/UL6SkyZpdz4" frameborder="0" allowfullscreen="true"></iframe>
</figure>


### Scalability
Scalability is extremely important for systems to be able to work over long durations and distances. Some tasks such as autonomous driving could demand systems to meet those requirements.
Two issues arise in current SLAM solutions which prevent them from being scalable: *memory* and *computation*.

*Example of a problem:*
Imagine you have a truck driving to deliver the weekly bread crumb supplies to the cities of Duckingdom.


<figure class="flow-subfigures">  
    <figcaption> The memory and computation problem example</figcaption>
    <img style='width:20em; height:auto' src="figures/bread_example.png"/>
</figure>


A typical truck travels 4000km per week.
Assume your SLAM system requires storing an image every 10m, and that a stored RGB image has dimensions 640x480p.
This leads to the use of 400 000 images and 368.64GB of images per week of driving. This is an issue since today's SLAM algorithms need those images available in active memory and involved in computations, which is not doable.

### Representation and understanding
While robustness and scalability might improve over time thanks to the advancements in various SLAM tools (deep learning, computer power and memory, optimization techniques...), one issue will require rethinking some parts of current solutions.

You can see here how a modern SLAM system represents the environment:

<figure class="flow-subfigures">  
    <figcaption>ORB SLAM</figcaption>
    <iframe style='width: 20em; height:auto' src="https://www.youtube.com/embed/ufvPS5wJAx0" frameborder="0" allowfullscreen="true"></iframe>
</figure>

Note how the SLAM map differs from what a human would see in an image. The SLAM system sees lines and points which appear salient visually, while the human would recognize objects and how they are arranged.
One core thing the SLAM system is missing is that it doesn't *understand* what it is seeing.
Not only could understanding help make the representation more compact (leading to a more scalable system), it would make the map more meaningful (and possibly make the system more robust), and make the systems more intelligent.

Here are examples of cases where having some kind of understanding of what is around can help SLAM:

<figure class="flow-subfigures">  
    <figcaption> Examples of cases where prior knowledge would help SLAM</figcaption>
    <figure>
        <figcaption> A scene looking visually different.</figcaption>
        <img style='width:20em; height:auto' src="figures/winter_summer.png"/>
    </figure>
    <figure>
        <figcaption> A bridge you maybe shouldn't cross.</figcaption>
        <img style='width:20em; height:auto' src="figures/bridge.png"/>
    </figure>
    <figure>
        <figcaption> A door you know will open.</figcaption>
        <img style='width:20em; height:auto' src="figures/auto_door.png"/>
    </figure>
    <figure>
        <figcaption> Someone you should listen to.</figcaption>
        <img style='width:20em; height:auto' src="figures/policeman.png"/>
    </figure>
</figure>

In the above figures, if the robot's already had some prior knowldege indicating the bridge is not safe enough, decided to not pass the bridge or in the left figure if you could recognize the material of the objects such as the glass wall, would avoid the robot to collide with the wall.


## The road to Spatial AI
As we have seen, current SLAM solutions have some issues, and SLAM still needs some work on. A recent paper <cite>FutureMapping by Andrew Davison, 2018</cite> introduced a new term: "*Spatial AI*". This paper describes considerations on where SLAM should be heading, and what we should aim at having.

### What is Spatial AI
Even though "Spatial AI" is a new term, it is not far from being simply a rebranding of what is also called "Semantic SLAM".

<figure class="flow-subfigures">  
    <figcaption> An example to show how Spatial AI evolved</figcaption>
    <img style='width:20em; height:auto' src="figures/wallcrack.png"/>
</figure>


AI usually leads to building good representations. Similarly, the core idea of Spatial AI is to have the right spatial representations about the space in which the robot operates. Spatial AI superseeds SLAM in the sense that it is more general than simple "localization and mapping".

### Differences with Geometric SLAM
On different levels Spatial AI differs with geometric SLAM.
We resume the main differences in the following table:

<table style="width:100%" border="1|0">
  <tr>
    <th>   </th>
    <th>Geometric SLAM</th>
    <th>Spatial AI</th>
  </tr>
  <tr>
    <td>Representation</td>
    <td> </td>
    <td> </td>
  </tr>
 <tr>
    <td> </td>
    <td>Map: bunch of geometric entities(points,lines,planes, etc)</td>
    <td>Map: semantic categories, objects, agents</td>
  </tr>
  <tr>
    <td> </td>
    <td>Task-agnostic</td>
    <td>Task-driven</td>
  </tr>
  <tr>
    <td> </td>
    <td>Not very interpretable</td>
    <td>Interpretability is key </td>
  </tr>
  <tr>
     <td>co-Design</td>
    <td>Sensors, Algorithms</td>
    <td>Sensors, Algorithms, Processors</td>
 </tr>
</table>

### Recent works towards Spatial AI
Making a robot understand what it sees is one of the key factors in its performance in the environment. By combining object detection with SLAM techniques we could get some prior knowledge over objects, and possibly localize camera position. For example by using Bayesian Deep Learning for object detection, to better undersand the uncertainty of a deep network's predictions.

Following video shows how semantic information can help SLAM to perform better:
<figure class="flow-subfigures">  
    <figcaption>Semantic helps SLAM</figcaption>
    <iframe style='width: 20em; height:auto' src="https://www.youtube.com/embed/ebIrBn_nc-k" frameborder="0" allowfullscreen="true"></iframe>
</figure>

Most recently, SLAM capabilities have been leveraged to show a good result in object detection and recognition task as you can see in the following video:
<figure class="flow-subfigures">  
    <figcaption>SLAM helps semantic</figcaption>
    <iframe style='width: 20em; height:auto' src="https://www.youtube.com/embed/m6sStUk3UVk" frameborder="0" allowfullscreen="true"></iframe>
</figure>

Also there are situations that SLAM and semantic can help each other:

<figure class="flow-subfigures">  
    <figcaption>SLAM and Semantic helps each other!</figcaption>
    <iframe style='width: 20em; height:auto' src="https://www.youtube.com/embed/0zHMMQPswgY" frameborder="0" allowfullscreen="true"></iframe>
</figure>
A higher level of representations, including objects and solid shapes, will play a key role in the future of SLAM, encodes the fact that real objects are 3-dimensional rather that 1D and this allows associating physical notions such volume and mass to object which is an important factor for robots to interact with their environment.  

<figure class="flow-subfigures">  
    <figcaption>Object level map</figcaption>
    <iframe style='width: 20em; height:auto' src="https://www.youtube.com/embed/tmrAh1CqCRo" frameborder="0" allowfullscreen="true"></iframe>
</figure>

Below is a video of Fusion++ work, an online object-level SLAM system that builds a persistent and accurate 3D graph map of arbitrary reconstructed objects. The work demonstrates an object-oriented online SLAM system with a focus on indoor scene understanding using RGB-D Data.

<figure class="flow-subfigures">  
    <figcaption>Current art in object-SLAM</figcaption>
    <iframe style='width: 20em; height:auto' src="https://www.youtube.com/embed/2luKNC03x4k" frameborder="0" allowfullscreen="true"></iframe>
</figure>

## Our Project: Line based SLAM in Duckietown

The project is a graph based SLAM with semantic lines. Lines are recognized as part of the road, we have 3 different type of lines, *yellow lines* in the center of the road, *white lines* located on the sides of the road and *red lines*  which is a stop line. Then we make use them as a prior knowledge on duckietown.

<figure class="flow-subfigures">  
    <figcaption> Different types of lines in Duckietown</figcaption>
    <img style='width:20em; height:auto' src="figures/lines.png"/>
</figure>

We use a line detection module using our line segment detector and compute a descriptor for each line then match those descriptors across images. use that to compute camera motion and also map those lines in 3D space. For that, we estimate the homography matrix to constuct lanes and computing cmera motion by measuring rotation and translation components. we're assuming the place recognition problem is already solved and we're using that to trigger global consistent optimization and integrate prior knowledge.
