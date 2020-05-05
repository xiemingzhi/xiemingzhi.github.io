---
title: "Image classification Object detection information"
description: ""
category: "AI"
tags: [machinelearning,tensorflow]
---
{% include JB/setup %}

Just listing some key information regarding image classification and object detection.  

## Data sets 

[https://www.cs.toronto.edu/~kriz/cifar.html]()  
The CIFAR-10 dataset (Canadian Institute For Advanced Research) is a collection of images that are commonly used to train machine learning and computer vision algorithms. It is one of the most widely used datasets for machine learning research. The CIFAR-10 dataset contains 60,000 32x32 color images in 10 different classes. The 10 different classes represent airplanes, cars, birds, cats, deer, dogs, frogs, horses, ships, and trucks. There are 6,000 images of each class.  
Various kinds of convolutional neural networks tend to be the best at recognizing the images in CIFAR-10.  

[http://www.image-net.org/]()  
The ImageNet project is a large visual database designed for use in visual object recognition software research. More than 14 million images have been hand-annotated by the project to indicate what objects are pictured and in at least one million of the images, bounding boxes are also provided. ImageNet contains more than 20,000 categories with a typical category, such as "balloon" or "strawberry", consisting of several hundred images.  
On 30 September 2012, a convolutional neural network (CNN) called AlexNet achieved a top-5 error of 15.3% in the ImageNet 2012 Challenge.  

[http://cocodataset.org/#explore]()  
Microsoft Common Objects in Context (COCO)	complex everyday scenes of common objects in their natural context.	Object highlighting, labeling, and classification into 91 object types.	2,500,000	Labeled images, text	Object recognition	2015  

[https://storage.googleapis.com/openimages/web/download.html]()  
Open Images	A Large set of images listed as having CC BY 2.0 license with image-level labels and bounding boxes spanning thousands of classes.	Image-level labels, Bounding boxes	9,178,275	Images, text	Classification, Object recognition	2017  


## Models 

Convolutional Neural Networks (CNNs) is the most popular neural network model being used for image classification problem. The big idea behind CNNs is that a local understanding of an image is good enough. The practical benefit is that having fewer parameters greatly improves the time it takes to learn as well as reduces the amount of data required to train the model. Instead of a fully connected network of weights from each pixel, a CNN has just enough weights to look at a small patch of the image. It’s like reading a book by using a magnifying glass; eventually, you read the whole page, but you look at only a small patch of the page at any given time.  
A convolution is a weighted sum of the pixel values of the image, as the window slides across the whole image. Turns out, this convolution process throughout an image with a weight matrix produces another image (of the same size, depending on the convention). Convolving is the process of applying a convolution.  

The Inception Network is an improvement on CNN.  
Salient parts in the image can have extremely large variation in size. The area occupied by the dog is different in each image.  
It performs convolution on an input, with 3 different sizes of filters (1x1, 3x3, 5x5). Additionally, max pooling is also performed. The outputs are concatenated and sent to the next inception module.  
Though adding an extra operation may seem counterintuitive, 1x1 convolutions are far more cheaper than 5x5 convolutions, and the reduced number of input channels also help.  

A residual neural network (ResNet) is an artificial neural network (ANN) of a kind that builds on constructs known from pyramidal cells in the cerebral cortex. Residual neural networks do this by utilizing skip connections, or short-cuts to jump over some layers. Typical ResNet models are implemented with double- or triple- layer skips that contain nonlinearities (ReLU) and batch normalization in between.  

[https://github.com/matterport/Mask_RCNN]()  
Mask R-CNN for object detection and instance segmentation on Keras and TensorFlow  

[https://github.com/experiencor/keras-yolo2]()  
Easy training on custom dataset. Various backends (MobileNet and SqueezeNet) supported. A YOLO demo to detect raccoon run entirely in brower is accessible at https://git.io/vF7vI (not on Windows).  

[https://github.com/tensorflow/models/tree/master/research/slim/nets/mobilenet]()  
MobileNets are small, low-latency, low-power models parameterized to meet the resource constraints of a variety of use cases. They can be built upon for classification, detection, embeddings and segmentation similar to how other popular large scale models, such as Inception, are used. MobileNets can be run efficiently on mobile devices with TensorFlow Mobile.  


