---
title: "Enable tensorflow gpu"
description: ""
category: "AI"
tags: [machinelearning,tensorflow]
---
{% include JB/setup %}

## How to setup tensorflow with GPU support on local pc 

## Requirements 

cpu: intel i7  
ram: 16G  
harddisk: ssd  
graphics: nvidia gtx 1060  
os: windows 10  

check that graphics card is supported  
[cuda](https://developer.nvidia.com/cuda-gpus)  
use [gpu-z](https://www.techpowerup.com/gpuz/) to check cuda compute version  
  

install latest driver for graphics card  
check windows device manager  
  
[NVIDIA® GPU drivers —CUDA 10.0 requires 410.x or higher.](https://www.tensorflow.org/install/gpu#software_requirements)  

[signup with nvidia developer program](https://developer.nvidia.com/)  
  

[install cuda](https://developer.nvidia.com/cuda-downloads?target_os=Windows)  
  

[install cudnn](https://developer.nvidia.com/compute/machine-learning/cudnn/)  
  

## Installation 

1. install [anaconda python](https://www.anaconda.com/distribution/) 
3. setup env

        ```
        conda create -n keras35 python=3.5
        conda activate keras35
        pip install tensorflow-gpu
        pip install --ignore-installed --upgrade tensorflow-gpu

        ```
4. run test program with cpu `tensorflow_test.py cpu 1000`
5. run test program with gpu `tensorflow_test.py gpu 1000`

test program available at:  
[tensorflow_test.py](https://github.com/xiemingzhi/tensorflowproject/blob/master/tensorflow_test.py)  

### todo enable tensorflow gpu in jupyter notebook

older version tf 1.13  
conda create -n tensorflowgpu113 pip python=3.5  
conda activate tensorflowgpu113  
pip install tensorflow-gpu==1.13.1


