# Odometry Calibration {#odometry_calibration status=draft}

Assigned: Jacopo Tani

## Introduction

The motors used on the Duckiebots are called "Voltage-controlled motors".

This means that the velocity of each motor is directly proportional to the voltage it is subject to. Even though we use the same model of motor for left and right wheel, they are not exactly the same. In particular, every motor responds to a given voltage signal in a slightly different way. Similarly, the wheels that we are using look "identical", but they might be slightly different.

If you drive the Duckiebot around using the joystick, you might notice that it doesn’t really go in a straight line when you command it to. This is due to those small differences between the motors and the wheels explained above.

Different motors can cause the left wheel and right wheel to travel at different speed even though the motors received the same command signal. Similarly, different wheels travel different distances even though the motors made the same rotation.

We can counter this behavior by *calibrating* the wheels. A calibrated Duckiebot
sends two different signals to left and right motor such that the robot moves in
a straight line when you command it to.

The relationship between linear and angular velocity of the robot and the velocities
of left and right motors are:
$$
\begin{align*}
    v_{\text{right}} &= (g + r) \cdot (v + \dfrac{1}{2} \omega l ) \\
    v_{\text{left}} &= (g - r) \cdot (v - \dfrac{1}{2} \omega l )
\end{align*}
$$
where $v_{\text{right}}$ and $V_{\text{left}}$ are the velocities of the two motors, $g$ is called *gain*, $r$ is called *trim*, $v$ and $\omega$ are the desired linear and the angular velocity of the robot, and $l$ is the distance between the two
wheels. The gain parameter $g$ controls the maximum speed of the robot.

With $g > 1.0$, the vehicle goes faster given the same velocity command, and for $g < 1.0$ it goes slower. The trim parameter $r$ controls the balance between the two motors.

With $r > 0$, the right wheel will turn slightly more than the left wheel given the same velocity command; with $r < 0$, the left wheel will turn slightly more the right wheel.

TODO for Jacopo Tani: It might be helpful to add the differential equations that relate velocities and voltages of the motors. -AD

<!-- From old google docs file: https://docs.google.com/document/d/1FmuOvPtKPMSIfay0wuxPQXESbmxYewl3gwJmhLU-4KY/edit#

Motivation
You might have noticed that your vehicle doesn’t really go in a straight line when you command it to. Also, the vehicle might not go at the velocity you are commanding it to drive at.

This is due to the fact a slight difference between the motors and the wheels can cause the left wheel and right wheel to travel slightly different distance even though they have made the same rotation. Also, the system has no encoders, so we are commanding open loop voltages without ensuring the desired wheel velocity is met.

We can counter this behavior by calibrating the “gain” and “trim”  on the commands that are sent to the wheels. This tutorial will walk you through the calibration process and also introduce the use of parameter server in ROS.
Method
We want to be able to specify a velocity(speed) and angular velocity(rotation) to control where the car goes. However the motors can only be controlled via rotation rate commands. Our robots use differential drive. For details see https://chess.eecs.berkeley.edu/eecs149/documentation/differentialDrive.pdf

Software Implementation
The inverse_kinematics_node under dagu_car pkg is in charge of translating a desired velocity(speed) and angular velocity(rotation) command, also called Twist2D, to motor voltages. It also adjusting the wheel voltage commands by a gain and trim value. The relationship between the velocities and the output voltages are defined as:

right_wheel_voltage = (gain + trim) * (linearVelocity + angularVelocity * 0.5 * baseline)
left_wheel_voltage = (gain - trim) * (linearVelocity - angularVelocity * 0.5 * baseline)

The baseline is the distance between the two wheels. Note that if the gain = 1.0 and trim = 0.0, the wheel’s voltages are exactly the same as the linear velocity + or - angular velocity times half the baseline length.


With gain > 1.0, the vehicle goes faster given the same velocity command, and for gain <1.0 it would go slower.

With trim > 0, the right wheel will turn slightly more than the left wheel given the same velocity command; with trim < 0, the left wheel will turn slightly more the right wheel.
Calibration Instructions
Make sure that your vehicle is on and connected to the wifi.

On your Duckiebot, launch the joystick

duckiebot: $ roslaunch duckietown_demos joystick.launch veh:=${VEHICLE_NAME}

Changing the trim to 0.01:

duckiebot: $ rosservice call /${VEHICLE_NAME}/inverse_kinematics_node/set_trim -- 0.01

Or Changing the trim in a negative way, e.g. to -0.01:

duckiebot: $ rosservice call /${VEHICLE_NAME}/inverse_kinematics_node/set_trim -- -0.01

Keep setting the trim until the duckiebot goes straight

Then start setting the gain:

duckiebot: $ rosservice call /${VEHICLE_NAME}/inverse_kinematics_node/set_gain -- 1.1

When you are all done, save the parameters by running:

duckiebot: $ rosservice call /${VEHICLE_NAME}/inverse_kinematics_node/save_calibration

If you do this the first time, you will see how it creates a new ${VEHICLE_NAME}.yaml file for your duckiebot in the folder:
duckietown/config/baseline/calibration/kinematics
 which you can add and commit to the git repo.

Testing instructions
See operation manual instructions for details
Demo instructions
You can connect to your robot and just run “make demo-joystick” to play around with driving straight and  changing the trim/gain
Performance Evaluation
The trim performance is tested by setting a reasonable distance (3 tiles length) and evaluating whether or not the robot can stay within the lane when it traverse the entire distance. The velocity is tested similarly by evaluating whether the robot traverses far enough or too far when driving for a specific amount of time.


-->
