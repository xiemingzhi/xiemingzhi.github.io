---
title: "Troubleshooting tensorflow model project"
description: ""
category: "AI"
tags: [machinelearning,tensorflow]
---
{% include JB/setup %}

**Problems running the [object detection tutorial](https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/installation.md)**

run object detection jupyter notebook  
```
cd object_detection
set PYTHONPATH=D:\ming\git\tensorflow-models;D:\ming\git\tensorflow-models\research;D:\ming\git\tensorflow-models\research\slim
jupyter notebook object_detection_tutorial.ipynb
```
problem  
```
AttributeError: module 'tensorflow._api.v1.compat' has no attribute 'v1'
```
troubleshoot  
conda list shows tensorflow 1.15  
jupyter notebook print(tf.__version__) shows 1.12 conda does not start jupyter with right env  
solution  
reinstall anaconda  
https://repo.anaconda.com/archive/Anaconda3-2019.10-Windows-x86_64.exe  

```
conda create -n tensorflow1 pip python=3.5
conda activate tensorflow1
conda install -c anaconda protobuf
python -m pip install --upgrade pip
pip install tensorflow-gpu==1.15
pip install --user Cython
pip install --user contextlib2
pip install --user pillow
pip install --user lxml
pip install --user jupyter
pip install --user matplotlib
```

---
problem  
jupyter notebook kernel error  
```
File "C:\Users\ming\AppData\Roaming\Python\Python37\site-packages\jupyter_core\paths.py", line 359, in win32_restrict_file_to_user
    import win32api
ImportError: DLL load failed: 找不到指定的程序。
```
solution 
//make sure to use python 3.5  
pip uninstall tensorflow-gpu  
//conda install tensorflow-gpu==1.14  
//search on http://anaconda.org  
//Package certifi conflicts for:  
//python=3.5 -> pip -> requests -> certifi[version='>=2016.09|>=2017.4.17']  
//tensorflow-gpu==1.14 -> tensorflow==1.14.0 -> python=3.6 -> pip -> requests -> certifi[version='>=2016.09|>=2016.9.26|>=2017.4.17']  
check C:\Users\ming\.conda\environments.txt  
conda create -n tensorflow1 pip python=3.5  
conda activate tensorflow1  
conda install -c anaconda protobuf  
//conda install tensorflow-gpu  
//python -m pip install --upgrade pip  
pip install tensorflow-gpu==1.15  
pip install --user Cython  
pip install --user contextlib2  
pip install --user pillow  
pip install --user lxml  
pip install --user jupyter  
pip install --user matplotlib  

set PYTHONPATH=D:\ming\git\tensorflow-models;D:\ming\git\tensorflow-models\research;D:\ming\git\tensorflow-models\research\slim  
set PYTHONPATH=D:\ming\git\models;D:\ming\git\models\research;D:\ming\git\models\research\slim  
jupyter notebook object_detection_tutorial.ipynb  

---
problem with anaconda 
```
 File "C:\Users\ming\AppData\Roaming\Python\Python37\site-packages\jupyter_core\paths.py", line 359, in win32_restrict_file_to_user
    import win32api
ImportError: DLL load failed: 找不到指定的程序。
```
Add \Anaconda3 and \Anaconda3\Script to Path in Environment variables.
add \Anaconda3\Library\bin 

(tensorflow1) C:\Users\ming>jupyter kernelspec list  
Available kernels:  
  python3    D:\Anaconda3\envs\tensorflow1\share\jupyter\kernels\python3  

(tensorflow1) C:\Users\ming>python -m ipykernel install --user  
Installed kernelspec python3 in C:\Users\ming\AppData\Roaming\jupyter\kernels\python3  

new install C:\Users\ming\Anaconda3  
delete  
C:\Users\ming\AppData\Roaming\jupyter  
C:\Users\ming\AppData\Roaming\Python   
C:\Users\ming  
.anaconda  
.conda  
.condarc  
.ipynb_checkpoints  
.ipython  
start base terminal   
(base) C:\Users\ming>jupyter kernelspec list  
Available kernels:  
  python3    C:\Users\ming\Anaconda3\share\jupyter\kernels\python3  

anaconda navigator create env tensorflow1   
install notebook   
(tensorflow1) C:\Users\ming>jupyter kernelspec list  
Available kernels:  
  python3    C:\Users\ming\Anaconda3\envs\tensorflow1\share\jupyter\kernels\python3  
  
---

problem  
kernel crashes on load label  
label_map_util.load_labelmap(PATH_TO_LABELS) 
solution  
check that object_detection package has been installed  
```
cd research
pip install . 
```

object_detection jupyter notebook uses tf2 so create tf2 environment  
        "!pip install -U --pre tensorflow==\"2.*\""  

conda create -n tensorflow2 pip python=3.5
conda activate tensorflow2  
conda install -c anaconda protobuf  
python -m pip install --upgrade pip  

pip install tensorflow-gpu==2.0
problems when running 
show_inference(detection_model, image_path)
```
UnknownError: 2 root error(s) found.
  (0) Unknown:  Failed to get convolution algorithm. This is probably because cuDNN failed to initialize, so try looking to see if a warning log message was printed above.
	 [[node FeatureExtractor/MobilenetV1/MobilenetV1/Conv2d_0/BatchNorm/batchnorm/mul_1 (defined at C:\Users\ming\Anaconda3\envs\tensorflow2\lib\site-packages\tensorflow_core\python\framework\ops.py:1751) ]]
```	 
Then, once inside the environment, install TensorFlow using CONDA rather than PIP:
conda install tensorflow-gpu
use navigator to install will download cudnn 

pip install --user Cython  
pip install --user contextlib2  
pip install --user pillow  
pip install --user lxml  
pip install --user matplotlib  

pip install --user jupyter  
use navigator to install 

cd research  
pip install .  

set PYTHONPATH=D:\ming\git\models;D:\ming\git\models\research;D:\ming\git\models\research\slim  
python -m ipykernel install --user  
Installed kernelspec python3 in C:\Users\ming\AppData\Roaming\jupyter\kernels\python3  
jupyter kernelspec list  
Available kernels:  
  python3    C:\Users\ming\AppData\Roaming\jupyter\kernels\python3  
kernel.json  
  "C:\\Users\\ming\\Anaconda3\\envs\\tensorflow2\\python.exe",  
this is okay it matches env python   
jupyter notebook object_detection_tutorial.ipynb  

---
conda install -c conda-forge pycocotools  
https://github.com/conda-forge/pycocotools-feedstock  
conda config --add channels conda-forge  
Once the conda-forge channel has been enabled, pycocotools can be installed with:  
conda install pycocotools  

---

kept getting  
```
UnknownError: 2 root error(s) found.
  (0) Unknown:  Failed to get convolution algorithm. This is probably because cuDNN failed to initialize, so try looking to see if a warning log message was printed above.
	 [[node FeatureExtractor/MobilenetV1/MobilenetV1/Conv2d_0/BatchNorm/batchnorm/mul_1 (defined at C:\Users\ming\Anaconda3\envs\tensorflow2\lib\site-packages\tensorflow_core\python\framework\ops.py:1751) ]]
```	 
check out https://github.com/tensorflow/models/tree/r1.13.0  
conda activate tensorflow1  
change to tf 1.13  
conda install tensorflow-gpu==1.13  
make sure  
python object_detection/builders/model_builder_test.py  
worked then  
jupyter notebook object_detection\object_detection_tutorial.ipynb  

if images do not appear edit D:\ming\git\models\research\object_detection\utils\visualization_utils.py
```
#import matplotlib; matplotlib.use('Agg')  # pylint: disable=multiple-statements
#import matplotlib.pyplot as plt  # pylint: disable=g-import-not-at-top
```

