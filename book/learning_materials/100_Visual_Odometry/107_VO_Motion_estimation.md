# Geometric Motion Estimation {#vo-motion-estimation status=beta}

The methods described below assume the perspective camera model for vision. But, these can be extended to other camera models depending on the underlying technique.

### 2D-to-2D


The geometric relations between two images $I_k$ and $I_{k-1}$ of a calibrated camera are described by the essential matrix $\mathbf{E}$.

$\mathbf{E}$ contains the camera motion parameters up to an unknown scale factor for the translation in the following form:

$$ \mathbf{E_k} = \mathbf{\hat{t}_k R_k} $$

where



$$ \mathbf{\hat{t}_k} =  \begin{bmatrix} 0 & -t_z & t_y \\ t_z & 0 & -t_x \\ -t_y & t_x & 0\end{bmatrix}$$

<center>
<figure>
    <img style='width:25em' src="figures/epipolar.png"/>
    <figcaption>Epipolar Constraint</figcaption>
</figure>
</center>

<br/>

The idea is to compute the essential matrix ($\mathbf{E}$) from the 2-D-to-2-D feature correspondences, extract and rotation matrix ($\mathbf{R}$) and translation matrix ($\mathbf{\hat{t}}$) or translation vector ($\mathbf{t}$).

The key property of the 2-D-to-2-D motion estimation is the epipolar constraint, which determines the line on which the corresponding feature point $\mathbf{\tilde{p}}'$ and $\mathbf{\tilde{p}}$ lies in the other image as in the figure.

<br/>

This constraint can be formulated mathematically as:

<br/>

$$ \mathbf{\tilde{p}'^TE\tilde{p}} = 0$$
$$ \mathbf{\begin{bmatrix} \tilde{u}' & \tilde{v}' & 1 \end{bmatrix} E \begin{bmatrix} \tilde{u} & \tilde{v} & 1 \end{bmatrix}^T} = 0$$

<br/>

This can be expressed as the linear system :

$$ \mathbf{A.E} = 0$$
where,$$ \mathbf{A} = \begin{bmatrix} \tilde{u}\tilde{u}' &  \tilde{u}'\tilde{v} & \tilde{u}' &  \tilde{u}\tilde{v}' & \tilde{v}\tilde{v}' & \tilde{v}' &  \tilde{u} & \tilde{v} &1\end{bmatrix}  $$

$$ \mathbf{E} = \begin{bmatrix} e_1 & e_2 & e_3 & e_4 & e_5 & e_6 & e_7 & e_8 & e_9\end{bmatrix}^T  $$

The rotation and transformation matrices can be obtained from the solution of the above linear system.

$$ \mathbf{USV^T} = svd(\mathbf{A})   $$
$$ \mathbf{R} = \mathbf{U (\pm W^T) V^T}$$

$$ \mathbf{\hat{t}} = \mathbf{U (\pm W) S U^T}$$
$$ \mathbf{W^T} = \begin{bmatrix} 0 & \pm 1 & 0 \\ \mp 1 & 0 & 0 \\ 0 & 0 & 1\end{bmatrix} $$

The relative scale for translation can be computed by triangulation of 3-D point pairs $\mathbf{X_{k-1}}$ and $\mathbf{X_{k}}$ from two subsequent images.
$$ r = \frac{|| \mathbf{X_{k-1,i}} - \mathbf{X_{k-1,j}} ||}{|| \mathbf{X_{k,i}} - \mathbf{X_{k,j}} ||} $$

<br/>
This factor $r$ is multiplied with the translation vector/matrix for the final transformation matrix.
<br/>

*Commonly used algorithms:*
<br/>

  - Longuet-Higgins 8-point algorithm
  - Nister’s 5-point algorithm

### 3D-to-3D

In this case, the arithmetic mean of all the 3D feature points $ (\mathbf{\bar{X}}$) of the image are computed and a similar linear system is solved to obtain the solution using :

$$ \mathbf{USV^T} = svd((\mathbf{X_{k-1}} -\mathbf{\bar{X}_{k-1}})(\mathbf{X_{k}} - \mathbf{\bar{X}_{k})^T})$$
$$ \mathbf{R_k} = \mathbf{VU^T} $$
$$ \mathbf{t_k} = \mathbf{\bar{X_k}} - \mathbf{R_k}\bar{\mathbf{X}}_{k-1}  $$

*Commonly used algorithm:*
  <br/>

  -  Least-squares fitting of two 3-d point sets

<br/>
### 3D-to-2D

The optimization problem to be solved can be stated as :

$$ \underset{T_k}{\operatorname{arg min}} \sum_{i}{|| \mathbf{p_k^i} - \mathbf{\hat{p}^i_{k-1}} ||}^2 $$


*Commonly used algorithm:*
<br/>

  -  Perspective from n points (PnP) (resection)

<br/>

An instance of the P3P problem can be represented as :
<br/>

$$ \mathbf{A.P} = 0$$
where,$$ \mathbf{A} = \begin{bmatrix} 0 & 0 & 0 & 0 & -x & -y & -z & -1 & x\tilde{v} &  y\tilde{v} & z\tilde{v} & \tilde{v} \\ x & y & z & 1 &  0 & 0 & 0 & 0 & -x\tilde{u} &  -y\tilde{u} & -z\tilde{u} & -\tilde{u} \end{bmatrix}  $$

$$ \mathbf{P} = \begin{bmatrix} \mathbf{P_k^1} & \mathbf{P_k^2} & \mathbf{P_k^3} \end{bmatrix}^T $$
$$ \mathbf{P_k^j} = j^{th}row(\mathbf{P_k}) $$
$$ \mathbf{P_k} = \mathbf{[R_k|t_k]} $$

$\mathbf{R_k}$ and $\mathbf{t_k}$ can be computed after computing $ \mathbf{P_k}  $ using SVD to compute the null-space of $\mathbf{A}$, similar to the other methods.
