# VO : The Problem {#visual-odometry-problem status=ready}

The VO problem is modeled as a sequence of transformations

<figure class="stretch">
<img src="figures/vo_scheme.png" style="width: 30em"/>
<figcaption>Figure 3.1 VO Problem Model </figcaption>
</figure>


### Input     

<br/>

**Image Sequence**

<br/>

* Monocular :

$$ I_{0:n}  = \{ I_0, I_1, …, I_n \}       $$

* Stereo    :

$$ I_{l, 0:n}  = \{ I_{l,0}, I_{l,1}, …, I_{l,n} \} $$
$$ I_{r, 0:n}  = \{ I_{r,0}, I_{r,1}, …, I_{r,n} \} $$  

### Output

<br/>

**Camera Poses**

Poses of the camera w.r.t. The initial frame at k = 0

$$ C_{0:n} = \{C_0, C_1, …, C_n\}  $$

Modelled by relative transformations $T_{1:n}$

$$                T_{1:n} = \{ T_{1,0}, T_{2,1}, …, T_{n,(n-1)}\}      $$

For simplicity, denote $T_{k, k-1}$ as $T_k$,

$$                T_{1:n} = \{ T_{1}, T_{2}, …, T_{n}\}      $$


The transformation matrix at each transformation $T_k$ is viewed as :

$$ T_{k, k-1} = T_{k} = \begin{bmatrix} \mathbf{R_{k, k-1}} & \mathbf{t_{k, k-1}} \\ 0 & 1 \end{bmatrix} $$


$\mathbf{R_{k, k-1}}$ = 3D Rotational Matrix

$\mathbf{t_{k, k-1}}$ = 3D Translation Vector              

Concatenate to recover full trajectory  :    
$$C_n = C_{n-1} T_n                                     $$




* In short : Estimate “relative motion” from image sequence
* Usually, the initial camera pose $ C_0 $ is considered to be the origin and these relative transformations are applied from there.
* This transformation matrix corresponds to 6-DoF pose parameters $ = $ $ \{t_x, t_y, t_z, \varphi, \theta, \psi \} $ where $t_x, t_y, t_z $ are the translation parameters along the 3 axes and $\varphi, \theta, \psi$ are the Euler angles indicating the rotation about the 3 axes (pitch, roll and yaw).
