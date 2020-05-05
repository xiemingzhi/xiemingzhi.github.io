---
title: "Object detection Tensorflow serving with RESTful API client"
description: ""
category: "AI"
tags: [machinelearning,tensorflow]
---
{% include JB/setup %}

If you are using [tensorflow-serving](https://www.tensorflow.org/tfx/serving/api_rest) to serve your models and are getting errors when trying to call the RESTful apis:

```
(tensorflow1) D:\tf\serving\tensorflow_serving>python object_detection_client_rest.py
...
Type: Object is not of expected type: uint8
Traceback (most recent call last):
  File "object_detection_client_rest.py", line 49, in <module>
    main()
  File "object_detection_client_rest.py", line 33, in main
    response.raise_for_status()

```

`object_detection_client_rest.py` reads in image file converts to base64 and makes post request to the predict method.

```
  jpeg_bytes = base64.b64encode(dl_request.content).decode('utf-8')
...
  predict_request = '{"signature_name": "serving_default", "instances" : [{"b64": "%s"}]}' % jpeg_bytes
  headers = {"content-type": "application/json"}
...
  response = requests.post(SERVER_URL, data=predict_request, headers=headers)
...
```

Check that your model has input tensors that take in `DT_STRING`

```
>saved_model_cli show --dir object_detection/1 --all
MetaGraphDef with tag-set: 'serve' contains the following SignatureDefs:

signature_def['serving_default']:
  The given SavedModel SignatureDef contains the following input(s):
    inputs['inputs'] tensor_info:
        dtype: DT_UINT8
        shape: (-1, -1, -1, 3)
        name: image_tensor:0
  The given SavedModel SignatureDef contains the following output(s):
    outputs['detection_boxes'] tensor_info:
        dtype: DT_FLOAT
        shape: (-1, 100, 4)
        name: detection_boxes:0
    outputs['detection_classes'] tensor_info:
        dtype: DT_FLOAT
        shape: (-1, 100)
        name: detection_classes:0
    outputs['detection_scores'] tensor_info:
        dtype: DT_FLOAT
        shape: (-1, 100)
        name: detection_scores:0
    outputs['num_detections'] tensor_info:
        dtype: DT_FLOAT
        shape: (-1)
        name: num_detections:0
  Method name is: tensorflow/serving/predict

```
This model only takes `DT_UINT8` as input so only be used as gRPC client. 

Re-save the model by following the instructions [tensorflow serving guide](https://github.com/xiemingzhi/tensorflowproject/blob/master/tf/README.md)  
Edit [export_model.py](https://github.com/xiemingzhi/tensorflowproject/blob/master/tf/models/research/object_detection/export_model.py)  

```
object_detection.exporter.export_inference_graph(input_type='encoded_image_string_tensor',pipeline_config=pipeline_proto,trained_checkpoint_prefix=input_checkpoint,output_directory=output_directory)
```

Updated model metadata  

```
signature_def['serving_default']:
  The given SavedModel SignatureDef contains the following input(s):
    inputs['inputs'] tensor_info:
        dtype: DT_STRING
        shape: (-1)
        name: encoded_image_string_tensor:0
  The given SavedModel SignatureDef contains the following output(s):
    outputs['detection_boxes'] tensor_info:
        dtype: DT_FLOAT
        shape: (-1, 100, 4)
        name: detection_boxes:0
    outputs['detection_classes'] tensor_info:
        dtype: DT_FLOAT
        shape: (-1, 100)
        name: detection_classes:0
    outputs['detection_scores'] tensor_info:
        dtype: DT_FLOAT
        shape: (-1, 100)
        name: detection_scores:0
    outputs['num_detections'] tensor_info:
        dtype: DT_FLOAT
        shape: (-1)
        name: num_detections:0
  Method name is: tensorflow/serving/predict
```

Copy model to location where it is accessible from docker.  
Use docker to serve model.  

```
TESTDATA="/home/docker/users/tensorflow-serving/tensorflow_serving/servables/tensorflow/testdata"
echo $TESTDATA
docker run -t --name tensorflow_serving --rm -p 8500:8500 -p 8501:8501 \
    -v "$TESTDATA/export_model:/models/object_detection" \
    -e MODEL_NAME=object_detection \
    tensorflow/serving:1.13.0 &
```

Check model metadata 

```
http://192.168.99.100:8501/v1/models/object_detection/metadata
...
"serving_default": {
   "inputs": {
    "inputs": {
     "dtype": "DT_STRING",
     "tensor_shape": {
      "dim": [
       {
        "size": "-1",
        "name": ""
       }
      ],
      "unknown_rank": false
     },
     "name": "encoded_image_string_tensor:0"
    }
   },
   "outputs": {
    "detection_boxes": {
     "dtype": "DT_FLOAT",
     "tensor_shape": {
      "dim": [
       {
        "size": "-1",
        "name": ""
       },
       {
        "size": "100",
        "name": ""
       },
       {
        "size": "4",
        "name": ""
       }
      ],
      "unknown_rank": false
     },
     "name": "detection_boxes:0"
    },
    "detection_scores": {
     "dtype": "DT_FLOAT",
     "tensor_shape": {
      "dim": [
       {
        "size": "-1",
        "name": ""
       },
       {
        "size": "100",
        "name": ""
       }
      ],
      "unknown_rank": false
     },
     "name": "detection_scores:0"
    },
    "detection_classes": {
     "dtype": "DT_FLOAT",
     "tensor_shape": {
      "dim": [
       {
        "size": "-1",
        "name": ""
       },
       {
        "size": "100",
        "name": ""
       }
      ],
      "unknown_rank": false
     },
     "name": "detection_classes:0"
    },
    "num_detections": {
     "dtype": "DT_FLOAT",
     "tensor_shape": {
      "dim": [
       {
        "size": "-1",
        "name": ""
       }
      ],
      "unknown_rank": false
     },
     "name": "num_detections:0"
    }
   },
   "method_name": "tensorflow/serving/predict"
  },
...
```

Edit [object_detection_client_rest.py](https://github.com/xiemingzhi/tensorflowproject/blob/master/tf/serving/tensorflow_serving/object_detection_client_rest.py) 

```
  print('Prediction class: {}, avg latency: {} ms'.format(
      prediction['detection_classes'], (total_time*1000)/num_requests))

```

Execute and output 

```
conda activate tensorflow1
cd tf\serving\tensorflow_serving
set PYTHONPATH=d:\tf\models;d:\tf\models\research;d:\tf\models\research\slim;d:\tf\serving  
python object_detection_client_rest.py 
...
Prediction class: [85.0, 11.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0], avg latency: 804.6616 ms
```

`85` is clock in [mscoco_label_map.pbtxt](https://github.com/tensorflow/models/blob/master/research/object_detection/data/mscoco_label_map.pbtxt)


