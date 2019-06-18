
# Learning Approaches to VO {#vo-learning-approaches status=ready}

* Learning can be used in any stage of the odometry pipeline such as:     
    - features detection
    - estimating feature correspondences
    - homography estimation

<br/>

* In general, for the motion estimation step, the problem is framed to use the labelled data of image sequences for training  a *regression* or *classification* model.

<br/>

#### **Key Advantages**

<br/>

* It does not require the camera calibration parameters to be known explicitly.

* It can estimate translation to the correct scale and are robust against similar kind of noises with which it is trained.
<br/>

#### **Some approaches**

<br/>

Several machine learning approaches have been tried and deep learning approaches are currently picking up pace. standard methods might evolve over time.

<br/>

  * Gaussian process of ego-motion from optical flow : [semi-parametric models](https://journals.sagepub.com/doi/abs/10.1177/0278364912472245)
  * Using only CNNs for pose prediction : [Learning visual odometry with a convolutional network](https://www.iro.umontreal.ca/~memisevr/pubs/VISAPP2015.pdf)
  * [DeepVO](https://www.cs.ox.ac.uk/files/9026/DeepVO.pdf) : A CNN for learning learning geometrical features and an RNN for modelling sequence of poses as seen in the figure.
  * [UnDeepVO](https://arxiv.org/pdf/1709.06841.pdf): Monocular Visual Odometry through Unsupervised Deep
Learning

<br/>

<div class="fig:deep-vo">
<figure class="stretch">
<img src="figures/deep_vo.png" style="width: 35em"/>
<figcaption> Deep VO Neural Network Model </figcaption>
</figure>
</div>
