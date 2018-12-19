# Object classification {#object_classification status=draft}

Assigned: Nick and David

Assigned: Yun

Object classification is the critical task in comoputer vision applications. It is the task of classifying objects from different object categories. It is useful for duckiebot to classify the objects in the recieved images and it can be helpful in tasks such as object detection and tracking. In duckietown, there are many informative objects such as traffic signs, duckiebots, duckies, house models, etc. 

Traditional ways of object classification extract features or descriptors for different classes first and then use classifiers to classify objects. Typical features used are HOG (Histogram of Oriented Gradients), SIFT (Scal-invariant Feature Transform), Haar-like, SURF (Speeded up Robust Features), etc. Nowadays, with the development of CNNs, the accuracy of object classification improved astoundingly.

An object classification algorithm takes images as input and output what objects it contains. For simplification, HOG plus two-class (binary) classifiers are explained because it has the best performance in traditional algorithms. 

## Steps in object classification
<figure class="stretch">
<img src="1.png"/>
</figure>






