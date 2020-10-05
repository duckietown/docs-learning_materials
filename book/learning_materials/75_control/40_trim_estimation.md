# Online Wheel "Trim" Estimation  {#trim_estimation status=beta}

In this section, a solution is proposed to specifically address the issue with a badly calibrated ``trim''. That is, generating the ability to dynamically adjust for an incorrect trim calibration online. Wheel "trim" refers to one wheel spinning faster than the other when they are both sent the same command, due to manufacturing discrepancies. This is perhaps a simpler approach to increasing controller robustness, as it takes advantage of a model. Consider a controller which outputs a commanded angular velocity, denoted $\omega^{\mathrm{com}}$, which is distinct form the actual vehicle angular velocity $\omega$. A commanded angular velocity leads to commanded right and left wheel velocities, denoted $\dot{\phi}_R^{\mathrm{com}}, \dot{\phi}_L^{\mathrm{com}}$ respectively. Now, suppose that these wheel velocities differ from actual wheel velocities $\dot{\phi}_R, \dot{\phi}_L$ by

$$\dot{\phi}_R = (1 + k^{\mathrm{trim}}) \dot{\phi}^{\mathrm{com}}_R,$$
$$\dot{\phi}_L = (1 - k^{\mathrm{trim}}) \dot{\phi}^{\mathrm{com}}_L.$$


That is, the parameter $k^{\mathrm{trim}}$ creates a discrepancy in a wheel's angular velocity relative to the other. If the commanded angular velocity and forward velocity are defined to be

$$ \omega^{\mathrm{com}} = 0.5 \frac{R}{L}(\dot{\phi}_R^{\mathrm{com}} - \dot{\phi}_L^{\mathrm{com}}),$$
$$ v^{\mathrm{com}} = 0.5 R(\dot{\phi}_R^{\mathrm{com}} + \dot{\phi}_L^{\mathrm{com}}),$$

respectively, then the actual vehicle angular velocity becomes

\begin{align}
    \omega &= 0.5 \frac{R}{L}(\dot{\phi}_R - \dot{\phi}_L) \\
        &= 0.5 \frac{R}{L}\left((1 + k^{\mathrm{trim}}) \dot{\phi}^{\mathrm{com}}_R - (1 - k^{\mathrm{trim}}) \dot{\phi}^{\mathrm{com}}_L\right)\\
        &= \omega^{\mathrm{com}} + 0.5 \frac{R k^{\mathrm{trim}}}{L}(\dot{\phi}^{\mathrm{com}}_R + \dot{\phi}^{\mathrm{com}}_L) \\
        &= \omega^{\mathrm{com}} + \frac{v^{\mathrm{com}}}{L}k^{\mathrm{trim}}.
\end{align}

Notice how this trim factor $k^{\mathrm{trim}}$ simply produces a bias on the angular velocity. Recall the linearized dynamics of the duckiebot, which after substituting the expression above for $\omega^{\mathrm{com}}$, now becomes 

$$ \left[\begin{array}{c}  \dot{d} \\ \dot{\phi} \end{array}\right] = \left[\begin{array}{cc} 0 & v^{\mathrm{com}} \\ 0 & 0 \end{array}\right] \left[\begin{array}{c} d \\ \phi \end{array}\right] + \left[\begin{array}{c} 0 \\ 1 \end{array}\right] \omega^{\mathrm{com}}  + \left[\begin{array}{c} 0 \\ v^{\mathrm{com}} / L \end{array}\right] k^{\mathrm{trim}},$$ 

which can be written as

$$\dot{\mathbf{x}} = \mathbf{A}\mathbf{x} + \mathbf{B}\omega^{\mathrm{com}} + \mathbf{L}k^{\mathrm{trim}}.$$

This form makes this problem an ideal candidate for a *disturbance observer*. That is, we seek to estimate $k^{\mathrm{trim}}$ dynamically based on the assumption that we are able to measure $\mathbf{x}$ (from a state estimator). To do this, construct an augmented state space system

$$ \left[\begin{array}{c} \dot{\mathbf{x}} \\ \dot{k}^{\mathrm{trim}} \end{array}\right] = \left[\begin{array}{cc} \mathbf{A} & \mathbf{L} \\ \mathbf{0} & \mathbf{0} \end{array}\right] \left[\begin{array}{c} \mathbf{x} \\ k^{\mathrm{trim}} \end{array}\right] + \left[\begin{array}{c} \mathbf{B} \\ \mathbf{0} \end{array}\right] \omega^{\mathrm{com}}$$

$$\mathbf{y} = \left[\begin{array}{cc} \mathbf{1} & \mathbf{0} \end{array}\right] \left[\begin{array}{c} \mathbf{x} \\ k^{\mathrm{trim}} \end{array}\right],$$

otherwise denoted as 

$$\dot{\mathbf{x}}_a  = \mathbf{A}_a \mathbf{x}_a + \mathbf{B}_a \omega^{\mathrm{com}}$$  
$$\mathbf{y} = \mathbf{C}_a \mathbf{x}_a$$

where we have assumed that we can "measure" the state of the duckiebot $\mathbf{y} = \mathbf{x}$ using some state estimator. With this system, one can design a classical observer that provides an estimate of $\mathbf{x}_a$, denoted $\hat{\mathbf{x}}_a$. It can be shown that the system above is observable. We define an observer with the following dynamics,

$$\dot{\hat{\mathbf{x}}}_a = \mathbf{A}_a \hat{\mathbf{x}}_a + \mathbf{B}_a \omega^{\mathrm{com}} + \mathbf{K}^{\mathrm{obs}}(\mathbf{y} - \mathbf{C}_a \hat{\mathbf{x}}_a),$$

and it is then possible to choose values for the observer gain matrix $\mathbf{K}^{\mathrm{obs}}$ using pole placement such that the estimation error $\mathbf{e} = \mathbf{x}_a - \hat{\mathbf{x}}_a$ asymptotically approaches zero. After estimator convergence, one simply extracts $\hat{k}^{\mathrm{trim}}$ from $\hat{\mathbf{x}}_a$ and subtracts the bias from the control command.

## Discretization
To actually implement the observer dynamics given above, you need to discretize the continous-time dynamics. There are multiple methods to discretize dynamic models, the simplest being an *Euler discretization*. Let $\Delta t$ be the time that has elapsed between two time steps, then the discretized observer dynamics are simply

$$ \hat{\mathbf{x}}_{a_{k+1}} = \hat{\mathbf{x}}_{a_k} + \Delta t ( \dot{\hat{\mathbf{x}}}_{a_k})$$

where $\dot{\hat{\mathbf{x}}}_{a_k}$ is computed by evaluating the continous time expression above at time step $k$.