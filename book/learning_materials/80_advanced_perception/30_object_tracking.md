# Object tracking {#object_tracking status=draft}

Assigned: Nick and David

Assigned: Ruixiang

Object tracking is the process of is the process of locating a moving object (or multiple objects) in a sequence of frames. Visual object tracking is an essential computer vision problem with many real-world applications including autonomous vehicles, robotics, motion-based recognition, video indexing, surveillance and security and human-computer interaction.

In this tutorial we focus on the class of online trackers for which a bounding box only in the initial frame is provided. There are a variety of algorithms for online tracking, we  briefly introduce two groups of tracking algorithms here.

##Tracking Using Template Matching
Trackers in this group perform a matching of the representation of the target model built form the previous frames. Normalized Cross-Correlation (**NCC**) is a most basic concept of tracking to perform direct target matching by normalized cross-correlation which uses the intensity values in the initial target bounding box as template. Lucas-Kanade Tracker (**KLT**): Ahead of his time by a wide margin, the tracker finds the affine-transformed match between the target bounding box and candidate windows around the previous location. The affine transformation is computed by incremental image alignment based on spatiotemporal derivatives and warping capable of dealing with scale, rotation and translation. The location of the target is determined by mapping the target position in the previous frame to the location in the current frame using computed affine transformation.

There're other trackers in this group which extend the idea of tracking with template matching by adding constraints over the matching process. Adaptive Coupled-layer Tracking (**ACT**) is a recent tracker that aims for rapid and significant appearance changes by sparse optimization in two layers. Tracking by Monte Carlo (**TMC**) sampling is another method aims to track targets for which the object shape
changes drastically over time by sparse optimization over patch pairs.

##Tracking Using Discriminative Classification
Trackers in this category build the model on the distinction of the target foreground against the background. Tracking-by-detection builds a classifier to distinguish target pixels from the background pixels, and updates the classifier by new samples coming in. Foreground-Background Tracker (**FBT**) is a simple approach to the incremental discriminative classifier is where a linear discriminant classifier is trained on Gabor texture feature vectors from the target region against feature vectors derived from the local background, surrounding the target. The target is searched in a window centered at the previous location. The highest classification
score determines the new position of the target in FBT. Tracking, Learning and Detection (**TLD**) aims at using labeled and unlabeled examples for discriminative classifier learning. The method is applied to tracking by combining the results of a detector and an optical flow tracker. Some trackers also extend discriminative classiaction methods by adding constraints, such as **STRuck**: Structured output tracking with kernels, 

##Tracking with OpenCV
To get more hands-on experience on object tracking, we can use the OpenCV library to implement our own tracking algorithm. OpenCV 3 comes with a new tracking API that contains implementations of many single object tracking algorithms. Many resources are available online, please refer to the [simple tutorial](https://www.learnopencv.com/object-tracking-using-opencv-cpp-python/) for more details.

##Reference
1. Smeulders, Arnold WM, et al. "Visual tracking: An experimental survey." IEEE transactions on pattern analysis and machine intelligence 36.7 (2014): 1442-1468.
2. Yilmaz, Alper, Omar Javed, and Mubarak Shah. "Object tracking: A survey." Acm computing surveys (CSUR) 38.4 (2006): 13.
3. 
