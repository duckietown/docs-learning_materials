# Representations  {#representations status=draft}

Assigned: Matt Walter

<div class="requirements" markdown="1">

Required Reading: The following assumes working knowledge of 2D and 3D Cartesian coordinate systems, reference frames, and coordinate transformations. If you are not familiar with these topics, please see the following preliminary chapters.

See: [Coordinate systems](#coordinate_systems)

See: [Reference frames](#reference_frames)

See: [Coordinate transformations](#transformations).

</div>


<!--
**Discuss**:
* Introduction to the notion of *state* as a sufficient statistic that represents the agent (robot) and environment.
* Describe qualities: sufficient statistic; compact (i.e., not conveying unnecessary information); and readily interpretable.
* Define notion of *static* and *dynamic* states.
* Provide examples of robot and environment states.
-->

Robots are embodied autonomous agents that interact with a physical environment.

As you read through this book, you will find that shared representations of the agent (i.e., the robot) and the environment in which it operates are fundamental to a robot's ability to sense, plan, and act---the capabilities that are key to making a robot a robot.

The *state* $\state_t \in \statesp$ is a representation that consists of a compilation of all knowledge about the robot and its environment that is *sufficient* both to perform a particular task as well as to predict the future. Of course, "predict the future" is vague, and we can not expect the state to include knowledge to predict *everything* about the future, but rather what is relevant in the context of the task.

\begin{definition}   \label{def:state}
    The state $\state_t \in \statesp$ is a representation of the robot and the environment that is sufficient to predict the future in the context of the task being performed.
\end{definition}

Comment: I understand that explicitly referencing the task as relevant to the notion of state is odd, but I want to convey that the representation that we choose for the state does depend upon the task.

Now, let's be a bit more formal with regards to what we mean by a state being "sufficient" to predict the future. We are specifically referring to a state that exhibits the *Markov property*, i.e., that it provides a complete summary of the past (again, in the context of the task). Mathematically, we say that a state is *Markov* if the future state is independent of the past given the present state (technically, this corresponds to first-order Markov):

\begin{equation}
    p(x_{t+1} \vert x_t, a_t, x_{t-1}, a_{t-1}, \ldots, x_0, a_0) = p(x_{t+1} \vert x_t, a_t)
\end{equation}

A state exhibits the Markov property if and only if the above holds.

Comment: I usually say that "state" is all that is needed to predict the future.
The agent only needs to keep track of a smaller thing than the state to act optimally.
There isn't a good name to use; but it should be distinct from "state".

Comment: I think that this oversimplifies things. What is it about the future that is being predicted? Certainly, the states used by autonomous systems aren't sufficient to predict everything about the future (e.g., a self-driving car can't predict whether a street light is going to come on, but that probably doesn't matter).

Knowledge about the robot and its environment is often extracted from the robot's multimodal sensor streams, such as wheel encoders and cameras. Consequently, one might choose to formulate the state as the collection of all of the measurements that the robot acquires over time. Indeed, the use of low-level sensor measurements as the representation of the state has a long history in robotics and artificial intelligence and has received renewed attention of-late, notably in the context of deep learning approaches to visuomotor policy learning.

However, while measurement history is a sufficient representation of the robot and its operating environment, it serves as a challenging definition of state.

First, raw measurements are redundant, both within a single observation and across successive measurements. For example, one doesn't need to reason over all pixels in an image, let alone all pixels within successive images to understand the location of a street sign.

Second, raw measurements contain a large amount of information that is not necessary for a given task. For example, the pixel intensities that capture the complex diffusion light due to clouds convey information that is not useful for self-driving cars. Requiring algorithms that deal with state to reason over these pixels would be unnecessarily burdensome.

Third, measurement history is very inefficient: its size grows linearly with time as the robot makes new observations, and it may be computationally intractable to access and process such a large amount of data.

This motivates the desire for a *minimal* representation that expresses knowledge that is both necessary and sufficient for the robot to perform a given task. More concretely, we will consider parameterized (symbolic) formulations of the state and will prefer representations that involve as few parameters as possible, subject to the state being Markov and the constraints imposed by the task.

Metric spaces (namely, Euclidean spaces) constitute the most commonly adopted state space in robotics, be it through the use of feature-based parameterizations of the state or gridmap representation. However, it is not uncommon to define the state of the robot and its environment in terms of a topological space or a hybrid metric-topological space.

Importantly, the exteroceptive and proprioceptive sensor measurements from which the robot's perception algorithms infer the state are noisy, the models that describe the motion of the robot and environment are error-prone, and many aspects of the state are not directly observable (e.g., your Duckiebot may not be able to "see" some of the other Duckiebots on the road due to occlusions or the cameras limited field-of-view). As a result, robots must reason over probabilistic models of the state, commonly referred to as the *belief*, in order to effectively mitigate this uncertainty.

## Robot Representations

TODO: Discuss conventions

The state of the robot typically includes its *pose* $\pose_t$, which specifies the position and orientation of a coordinate frame affixed to the robot relative to a fixed global coordinate frame commonly referred to as the "world frame". The pose then defines the rigid-body [rigid-body transformation](#transformations) between the two reference frames.

For rigid-body robots that operate in a plane ($\reals^2$), the pose $\pose \in \SEtwo$ consists of the Cartesian coordinates $(x,y)$ that specify the robot's position and the angle $\theta$ that defines the robot's orientation (yaw). For robots in $\reals^3$, including some ground platforms, aerial vehicles, and underwater vehicles, the pose $\pose \in \SEthree$ consists of three Cartesian coordinates $(x, y, z)$ that encode the robot's position, and three Euler angles $\phi, \theta, \psi$ that specify the robot's orientation.

<div figure-id="fig:robot_pose_2d" figure-caption="The pose $\pose_t$ of a robot operating in a two-dimensional world consists of the robots $(x,y)$ position and orientation $\theta$ relative to a fixed reference frame.">
  <img src="robot_pose_2d.pdf" style='width: 15em; height:auto'/>
</div>

In addition to the robot's pose, it is often useful to include its linear and angular velocities as elements of the state, resulting in an additional set of three or six parameters for robots that operate in two dimensions and three dimensions, respectively.

<!--
Define the notion of:

* *pose* for mobile robots;
* *configuration* for manipulators
* robot and joint velocities
-->

TODO: Discuss specific robot state representation for Duckietown.

## Environment Representations

The two most commonly used metric representations of the robot's environment are occupancy grid models and feature-based representations.

Occupancy grid representations discretize the world into a set of grid cells. Associated with each cell are its Cartesian coordinates in a global reference frame, $(x,y)$ in $\reals^2$ and $(x,y,z)$ in $\reals^3$, and a binary label that indicates whether the cell is occupied, where a value of one denotes that the cell is occupied.

Comment: pictures for these two? -AC

Feature-based models constitute the second commonly used environment representation. Feature-based representations model the environment as a collection of landmarks, and parameterize each in terms of the landmark's  position ($(x,y)$ or $(x,y,z)$ in $\reals^2$ and $\reals^3$, respectively) and possibly its orientation.


Discuss:

* Difference between topological and metric environment representations;
* Details of topological representation;
* Common metric representations, notably feature-based maps and gridmaps;

### Duckietown Environment Representation

TODO: Discuss specific environment representation for Duckietown.
