---
title: "ML scikit learn and tensorflow study"
description: ""
category: 
tags: [machinelearning]
---
{% include JB/setup %}

# scikit-learn wikipedia

Scikit-learn is a free software machine learning library for the Python programming language. It features various classification, regression and clustering algorithms including support vector machines, random forests, gradient boosting, k-means and DBSCAN, and is designed to interoperate with the Python numerical and scientific libraries NumPy and SciPy.  
Use docker:

```shell 
docker run -p 8888:8888 jupyter/scipy-notebook
check console for token 
browser http://192.168.99.100:8888/?token=<token_id>
open notebook 
from sklearn import datasets
digits = datasets.load_digits()
from sklearn import svm
clf = svm.SVC(gamma=0.001, C=100.)
clf.fit(digits.data[:-1], digits.target[:-1])
clf.predict(digits.data[-1:])

```

# tensorflow wikipedia 

TensorFlow is an open-source software library for dataflow programming across a range of tasks. It is a symbolic math library, and is also used for machine learning applications such as neural networks.  
Use docker:

```shell 
docker run -p 8888:8888 jupyter/tensorflow-notebook
check console for token 
browser http://192.168.99.100:8888/?token=<token_id>
open notebook 
import tensorflow as tf
mnist = tf.keras.datasets.mnist

(x_train, y_train),(x_test, y_test) = mnist.load_data()
x_train, x_test = x_train / 255.0, x_test / 255.0

model = tf.keras.models.Sequential([
  tf.keras.layers.Flatten(),
  tf.keras.layers.Dense(512, activation=tf.nn.relu),
  tf.keras.layers.Dropout(0.2),
  tf.keras.layers.Dense(10, activation=tf.nn.softmax)
])
model.compile(optimizer='adam',
              loss='sparse_categorical_crossentropy',
              metrics=['accuracy'])

model.fit(x_train, y_train, epochs=5)
model.evaluate(x_test, y_test)

```

# difference 

TensorFlow is more  low-level; basically, the Lego bricks that help you to implement machine learning algorithms whereas scikit-learn offers you off-the-shelf algorithms, e.g., algorithms for classification such as SVMs, Random Forests, Logistic Regression.

## use keras with tensorflow 

Keras is an open source neural network library written in Python. It is capable of running on top of TensorFlow, Microsoft Cognitive Toolkit or Theano.[1] Designed to enable fast experimentation with deep neural networks, it focuses on being user-friendly, modular, and extensible. 

# Free and open-source software machine learning algorithms

CNTK
Deeplearning4j
ELKI
H2O
Mahout
Mallet
MLPACK
MXNet
OpenNN
Orange
scikit-learn
Shogun
Spark MLlib
TensorFlow
Torch / PyTorch
Weka / MOA
Yooreeka
