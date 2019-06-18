# Odometry Calibration {#odometry_calibration status=ready}

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

\begin{align*}
    v_{\text{right}} &= (g + r) \cdot (v + \dfrac{1}{2} \omega l ) \\
    v_{\text{left}} &= (g - r) \cdot (v - \dfrac{1}{2} \omega l )
\end{align*}

where $v_{\text{right}}$ and $V_{\text{left}}$ are the velocities of the two motors, $g$ is called *gain*, $r$ is called *trim*, $v$ and $\omega$ are the desired linear and the angular velocity of the robot, and $l$ is the distance between the two
wheels. The gain parameter $g$ controls the maximum speed of the robot.

With $g > 1.0$, the vehicle goes faster given the same velocity command, and for $g < 1.0$ it goes slower. The trim parameter $r$ controls the balance between the two motors.

With $r > 0$, the right wheel will turn slightly more than the left wheel given the same velocity command; with $r < 0$, the left wheel will turn slightly more the right wheel.

TODO for Jacopo Tani: It might be helpful to add the differential equations that relate velocities and voltages of the motors. -AD
