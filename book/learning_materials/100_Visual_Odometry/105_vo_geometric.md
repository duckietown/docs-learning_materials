# Geometric Approaches to VO {#vo-geometric-approaches status=beta}

The geometric approaches to VO consider the geometry of the scene. These take advantage of the visual geometry in the camera-world model. They can be further sub-classified into the following

<br/>

<br/>

1. **Feature-based Methods (Sparse)(Indirect) :**

      <br/>

      <br/>

      Specific features (landmarks) are extracted from the images for motion estimation. These features across time instants are matched to compute the motion parameters using geometry. This is explained in detail in [next section](#vo-feature-based-pipeline).

      <br/>

<br/>      

<br/>      

2. **Appearance-based Methods (Dense)(Direct) :**

      <br/>

      <br/>

      The entire image as a pixel matrix is considered and optimized for the photometric error. These methods are also referred to as Correspondence-Free Methods.

      <br/>

      Motion estimation is done using one of the following methods :

      <br/>

      <br/>

      * **Optical flow method**
        <br/>
        It individually tracks pixels (all or subs-region of image). It assumes small motion between frames and, therefore, is not suitable for VO applications since motion error accumulates quickly.

        <br/>

        <br/>

      * **Featureless motion-estimation methods**
        <br/>
        All the pixels in the two images are used to compute the relative motion using a harmonic Fourier transform. This method works well with low-texture images but it is not used due to
          - Very high computational expense
          - less accuracy

        <br/>      

<br/>

<br/>            

3. **Hybrid Methods (Semi-dense)(Semi-direct) :**

      <br/>

      <br/>

      A hybrid of the first two stated methods.
      <br/>

      For more details, take a look at [SVO: Fast Semi-Direct Monocular Visual Odometry](https://www.ifi.uzh.ch/dam/jcr:e9b12a61-5dc8-48d2-a5f6-bd8ab49d1986/ICRA14_Forster.pdf)


<br/>

<br/>

<figure class="stretch">
<img src="figures/vo_geometric.png" style="width: 30em"/>
<figcaption>Figure 4.1 VO geometric approaches </figcaption>
</figure>
