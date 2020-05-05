---
title: "How to serve your tensorflow model"
description: ""
category: "AI"
tags: [machinelearning,tensorflow]
---
{% include JB/setup %}

Make [object detection](https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/installation.md) model works  

Prerequistes  
1. Install anaconda/python
2. Install tensorflow/tensorflow-gpu
3. git clone https://github.com/tensorflow/models.git

## Install dependencies 

```
open anaconda cmd
cd tensorflow-models\research 
pip install --user Cython
Installing collected packages: Cython
  The scripts cygdb.exe, cython.exe and cythonize.exe are installed in 'C:\Users\ming\AppData\Roaming\Python\Python35\Scripts' which is not on PATH.
  Consider adding this directory to PATH or, if you prefer to suppress this warning, use --no-warn-script-location.
Successfully installed Cython-0.29.14
pip install --user Cython
pip install --user contextlib2
pip install --user pillow
pip install --user lxml
pip install --user jupyter
pip install --user matplotlib
```

## Run protoc 

cd tensorflow-models\research  
protoc object_detection/protos/*.proto --python_out=.  
(keras35) D:\ming\git\tensorflow-models\research>protoc .\object_detection\protos\*.proto --python_out=.  
.\object_detection\protos\*.proto: No such file or directory  
(keras35) D:\ming\git\tensorflow-models\research>protoc --version  
libprotoc 3.6.1  

//does not work  
//protoc object_detection\protos --python_out=.  
//protoc --python_out=. .\object_detection\protos\*.proto  
//protoc object_detection\protos\*.proto --python_out=. --proto_path=.  
//FOR %i IN (object_detection\protos\*.proto) DO protoc object_detection\protos\%i --python_out=.  

//working  
```
for /f %i in ('dir /b object_detection\protos\*.proto') do protoc --python_out=. object_detection\protos\%i
```

## Add path 

export PYTHONPATH=$PYTHONPATH:`pwd`:`pwd`/slim
```
set PYTHONPATH=%PYTHONPATH%;D:\ming\git\tensorflow-models\research;D:\ming\git\tensorflow-models\research\slim
echo %PYTHONPATH%
```

## Run test 

Problems executing model builder test  
```
python object_detection/builders/model_builder_test.py
...
  File "D:\ming\git\tensorflow-models\research\object_detection\predictors\convolutional_box_predictor.py", line 23, in <module>
    slim = tf.contrib.slim
AttributeError: module 'tensorflow' has no attribute 'contrib'
```
See:  
[https://github.com/googlecolab/colabtools/issues/602]()  
[https://github.com/tensorflow/models.git]() not ready for tf2 use tf1 

## Create environment for tensorflow 1.x 

```
conda create -n tensorflow1 pip python=3.5
conda activate tensorflow1
python -m pip install --upgrade pip
//pip install --ignore-installed --upgrade tensorflow-gpu
pip install tensorflow-gpu==1.15

cd models\research 
pip install --user Cython
pip install --user contextlib2
pip install --user pillow
pip install --user lxml
pip install --user jupyter
pip install --user matplotlib
```

## Run test for tensorflow 1.x

```
python object_detection/builders/model_builder_test.py
...
----------------------------------------------------------------------
Ran 17 tests in 0.168s

OK (skipped=1)
```

### todo train model 

### todo evaluate model 


## troubleshooting 

  File "C:\Users\ming\Anaconda3\envs\tensorflow1\lib\site-packages\object_detection\models\faster_rcnn_inception_resnet_v2_feature_extractor.py", line 28, in <module>
    from nets import inception_resnet_v2
ModuleNotFoundError: No module named 'nets'
set PYTHONPATH=D:\ming\git\models;D:\ming\git\models\research;D:\ming\git\models\research\slim  
