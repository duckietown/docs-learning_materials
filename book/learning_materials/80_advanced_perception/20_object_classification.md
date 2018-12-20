# Object classification {#object_classification status=draft}

Assigned: Nick and David

Assigned: Yun

Object classification is the critical task in comoputer vision applications. It is the task of classifying objects from different object categories. It is useful for duckiebot to classify the objects in the recieved images and it can be helpful in tasks such as object detection and tracking. In duckietown, there are many informative objects such as traffic signs, duckiebots, duckies, house models, etc. 

Traditional ways of object classification extract features or descriptors for different classes first and then use classifiers to classify objects. Typical features used are Histogram of Oriented Gradients (HOG), Scale-Invariant Feature Transform (SIFT), Haar-like features, Speeded Up Robust Feature (SURF) etc. Nowadays, with the development of CNNs, the accuracy of object classification improved astoundingly.

An object classification algorithm takes images as input and output what objects it contains. For simplification, HOG plus two-class (binary) classifiers are explained because it has the best performance in traditional algorithms. 

## Steps in object classification

The main pipeline is shown as following:
<figure class="stretch">
<img src="1.png"/>
<figcaption>Figure1.1 Pipeline of object classification</figcaption>
</figure>

## 1.1. Preprocessing

An input image is usually pre-processed to normalize contrast and brightness effects. Often it is done by subtracting the mean of image intensities and divided by the standard deviation. When dealing with color images, a transformation in color space such as from RGB to LAB color space might be helpful. Besides, the input images are cropped and resized to a fixed size because fixed sized images should be performed in the next step of feature extraction.

## 1.2. Feature Extraction

Some well-known features are Haar-like features, Histogram of Oriented Gradients (HOG), Scale-Invariant Feature Transform (SIFT), Speeded Up Robust Feature (SURF) etc.

## 1.3. Learning Algorithm For Classification

After converting an image into a feature vector, a classification algorithm will take this feature vector as input and output a class label (e.g. duckiebot or background).

The general principle is that learning algorithms treat feature vectors as points in higher dimensional space and try to find planes or surfaces that partition the higher dimensional spaces. As a result, the examples of same class are on the same one side of the plane or surface.

Support Vector Machine (SVM) is one of the most popular supervised binary classification algorithm. It tries to find the hyperplane that can maximize the margin of two classes. It is also widly used combined with HOG for better accuracy in object classification. 













