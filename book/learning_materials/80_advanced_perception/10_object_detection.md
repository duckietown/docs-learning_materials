# Object detection {#object_detection status=draft}

Assigned: Nick and David

Assigned: Yun

## Background

Object detection helps autonomous vehicles detect different objects. The difference between object detection and classification is that detection algorithms not only output the class labels that the objects belong to, but also output the exact bounding boxes for the objects. 

<figure class="stretch">
<img src="2.png"/>
<figcaption>Figure 2.1 Object Detection</figcaption>
</figure>

Generally, object detection can be solved by features of machine learning approaches such as SIFT, HOG, Haar-like features etc. However, deep learning approaches perform much better lately and will be illustrated here. 

Region-CNN (R-CNN) is one of the state-of-the-art CNN-based deep learning object detection approaches. Based on this, there are fast R-CNN and faster R-CNN for the faster speed in object detection. Besides, mask R-CNN is good at object instance segmentation. On the other hand, there are also other approaches such as Single Shot MultiBox Detector (SSD) and You Only Look Once (YOLO), of which YOLO achieves the fastest performance on detection so that it can perform real-time object detection on videos.

## Region-CNN (R-CNN)

For object detection, what we need are bounding box size and location. Conventionally, for each image, there is a sliding window to search every position within the image shown in Figure 3. However, it is not efficient because different objects or even the same kind objects can have different sizes or aspect ratios. The different sizes and ratios affect effective window size, which can be extremly slow using CNN at each location for classification. 

<figure class="stretch">
<img src="3.png"/>
<figcaption>Figure 2.2 Sliding windows with different sizes and aspect ratios</figcaption>
</figure>

Instead, R-CNN uses selective search to generate about 2K region proposals which are bounding boxes for image classification. Then, for each bounding box, image classification is done by CNN. Finally, each bounding box can be refined using regression shown in the pipeline below:
 
 <figure class="stretch">
 <img src="4.png"/>
 <figcaption>Figure 2.3 Pipeline of R-CNN</figcaption>
 </figure>
 
 For the selective search:
 ** Generate initial sub-segmentation. Many candidate regions are generated. **
 ** Greedy algorithm is used to recursively combine similar regions into larger ones. **
 ** The generated regions are used to produce final candidate region proposals. **
  
<figure class="stretch">
<img src="5.png"/>
<figcaption>Figure 2.4 R-CNN classification</figcaption>
</figure>

As a result, 2000 candidate region proposals are wraped into a square and fed into a convolutional neural network that produces a 4096-dimensional feature vector as output. The CNN acts as a feature extractor and the output features are fed into a SVM to classify the whether or not there is an object in the proposed region. Besides, it also predicts the four offset values of a bounding box.

<figure class="stretch">
<img src="6.png"/>
<figcaption>Figure 2.5 R-CNN classification and detection</figcaption>
</figure>

## Fast R-CNN 

Since a huge time of training is required because you need to classify 2000 region proposals per image, the algorithm is slow and not efficient. As a result, Fast R-CNN is proposed by feeding the entire input image to the CNN for generating the feature map instead of proposed regions. Besides, RoI and softmax layer are used for feature extraction and classification.

<figure class="stretch">
<img src="7.png"/>
<figcaption>Figure 2.6 Fast R-CNN</figcaption>
</figure>

## Faster R-CNN 

However, R-CNN and Fast R-CNN both use selective search algorithm which is very time-consuming. As a result, a seperate network is used to predict region proposals replacing the selective search for efficiency. 

<figure class="stretch">
<img src="8.png"/>
<figcaption>Figure 2.7 Faster R-CNN</figcaption>
</figure>

<figure class="stretch">
<img src="9.png"/>
<figcaption>Figure 2.8 Test time and speed comparision</figcaption>
</figure>

Faster R-CNN is much faster than it’s predecessors and can be used for real-time object detection.

## YOLO— You Only Look Once

All the previous algorithms use region-based algorithm. Hoowever, YOLO is different and uses a single convolutional network to predict the bounding boxes and class probabilities for these boxes.

<figure class="stretch">
<img src="10.png"/>
<figcaption>Figure 2.9 YOLO</figcaption>
</figure>

YOLO takes and image and split it into an S*S grid and in each grid we take m bounding boxes. For each of the bounding box, the network outputs a class probability and offset values for bounding boxes. The bounding boxes whose class probability is above a threshold will be selected and used to locate object. YOLO is very fast of having 45 frames per second that is faster than other object detection algorithms. Hoowever, it struggles in detecting small objects due to the spatial constraints.

## YOLOv2

Due to the limitation of YOLO, an improving version of YOLO is proposed for better recall and localization while mantaining the classification accuracy. The structure is 19 convolutional and 5 maxpooling layers.

The improvements are made by using achors, so that it does not predict bounding box coordinates but offsets on priors. The box dimensions of priors are learnt by running k-means clustering. Besides, batch normalization is used. The input is replaced by a higher resolution input of 416* 416 instead of 224* 224. Besides, multi-scale training is implemented. 

Due to these changes, the recall has been improved while the speed is still fast, and it is great for detecting small objects.

## YOLOv3

The improvement is aimed at increasing accuracy in small objects by YOLOv3. There are 53 convolutional and no maxpool layers. The residual blocks, upsampling, and skipping connections which are latest computer vision machineries are used. And different scales of images are detected so that it does not miss small objects.  

<figure class="stretch">
<img src="11.png"/>
<figcaption>Figure 2.10 Feature pyramid network</figcaption>
</figure>

<figure class="stretch">
<img src="12.png"/>
<figcaption>Figure 2.11 Sppeed comparision</figcaption>
</figure>

YOLOv3 is slower than before but enough fast for real time detection with a good accuracy in detecting small objects. 

<figure class="stretch">
<img src="13.png"/>
<figcaption>Figure 2.12 Inference time</figcaption>
</figure>


