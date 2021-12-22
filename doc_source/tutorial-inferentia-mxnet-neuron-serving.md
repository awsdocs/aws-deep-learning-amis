# Using MXNet\-Neuron Model Serving<a name="tutorial-inferentia-mxnet-neuron-serving"></a>

In this tutorial, you learn to use a pre\-trained MXNet model to perform real\-time image classification with Multi Model Server \(MMS\)\. MMS is a flexible and easy\-to\-use tool for serving deep learning models that are trained using any machine learning or deep learning framework\. This tutorial includes a compilation step using AWS Neuron and an implementation of MMS using MXNet\.

 For more information about the Neuron SDK, see the [AWS Neuron SDK documentation](https://awsdocs-neuron.readthedocs-hosted.com/en/latest/neuron-guide/neuron-frameworks/mxnet-neuron/index.html)\. 

**Topics**
+ [Prerequisites](#tutorial-inferentia-mxnet-neuron-serving-prerequisites)
+ [Activate the Conda Environment](#tutorial-inferentia-mxnet-neuron-serving-activate)
+ [Download the Example Code](#tutorial-inferentia-mxnet-neuron-serving-download)
+ [Compile the Model](#tutorial-inferentia-mxnet-neuron-serving-compile)
+ [Run Inference](#tutorial-inferentia-mxnet-neuron-serving-inference)

## Prerequisites<a name="tutorial-inferentia-mxnet-neuron-serving-prerequisites"></a>

 Before using this tutorial, you should have completed the set up steps in [Launching a DLAMI Instance with AWS Neuron](tutorial-inferentia-launching.md)\. You should also have a familiarity with deep learning and using the DLAMI\. 

## Activate the Conda Environment<a name="tutorial-inferentia-mxnet-neuron-serving-activate"></a>

 Activate the MXNet\-Neuron conda environment by using the following command: 

```
source activate aws_neuron_mxnet_p36
```

 To exit the current conda environment, run: 

```
source deactivate
```

## Download the Example Code<a name="tutorial-inferentia-mxnet-neuron-serving-download"></a>

 To run this example, download the example code using the following commands: 

```
git clone https://github.com/awslabs/multi-model-server
cd multi-model-server/examples/mxnet_vision
```

## Compile the Model<a name="tutorial-inferentia-mxnet-neuron-serving-compile"></a>

Create a Python script called `multi-model-server-compile.py` with the following content\. This script compiles the ResNet50 model to the Inferentia device target\. 

```
import mxnet as mx
from mxnet.contrib import neuron
import numpy as np

path='http://data.mxnet.io/models/imagenet/'
mx.test_utils.download(path+'resnet/50-layers/resnet-50-0000.params')
mx.test_utils.download(path+'resnet/50-layers/resnet-50-symbol.json')
mx.test_utils.download(path+'synset.txt')

nn_name = "resnet-50"

#Load a model
sym, args, auxs = mx.model.load_checkpoint(nn_name, 0)

#Define compilation parameters#  - input shape and dtype
inputs = {'data' : mx.nd.zeros([1,3,224,224], dtype='float32') }

# compile graph to inferentia target
csym, cargs, cauxs = neuron.compile(sym, args, auxs, inputs)

# save compiled model
mx.model.save_checkpoint(nn_name + "_compiled", 0, csym, cargs, cauxs)
```

 To compile the model, use the following command: 

```
python multi-model-server-compile.py
```

 Your output should look like the following: 

```
...
[21:18:40] src/nnvm/legacy_json_util.cc:209: Loading symbol saved by previous version v0.8.0. Attempting to upgrade...
[21:18:40] src/nnvm/legacy_json_util.cc:217: Symbol successfully upgraded!
[21:19:00] src/operator/subgraph/build_subgraph.cc:698: start to execute partition graph.
[21:19:00] src/nnvm/legacy_json_util.cc:209: Loading symbol saved by previous version v0.8.0. Attempting to upgrade...
[21:19:00] src/nnvm/legacy_json_util.cc:217: Symbol successfully upgraded!
```

 Create a file named `signature.json` with the following content to configure the input name and shape: 

```
{
  "inputs": [
    {
      "data_name": "data",
      "data_shape": [
        1,
        3,
        224,
        224
      ]
    }
  ]
}
```

Download the `synset.txt` file by using the following command\. This file is a list of names for ImageNet prediction classes\. 

```
curl -O https://s3.amazonaws.com/model-server/model_archive_1.0/examples/squeezenet_v1.1/synset.txt
```

Create a custom service class following the template in the `model_server_template` folder\. Copy the template into your current working directory by using the following command: 

```
cp -r ../model_service_template/* .
```

 Edit the `mxnet_model_service.py` module to replace the `mx.cpu()` context with the `mx.neuron()` context as follows\. You also need to comment out the unnecessary data copy for `model_input` because MXNet\-Neuron does not support the NDArray and Gluon APIs\. 

```
...
self.mxnet_ctx = mx.neuron() if gpu_id is None else mx.gpu(gpu_id)
...
#model_input = [item.as_in_context(self.mxnet_ctx) for item in model_input]
```

 Package the model with model\-archiver using the following commands: 

```
cd ~/multi-model-server/examples
model-archiver --force --model-name resnet-50_compiled --model-path mxnet_vision --handler mxnet_vision_service:handle
```

## Run Inference<a name="tutorial-inferentia-mxnet-neuron-serving-inference"></a>

Start the Multi Model Server and load the model that uses the RESTful API by using the following commands\. Ensure that neuron\-rtd is running with the default settings\. 

```
cd ~/multi-model-server/
multi-model-server --start --model-store examples > /dev/null # Pipe to log file if you want to keep a log of MMS
curl -v -X POST "http://localhost:8081/models?initial_workers=1&max_workers=4&synchronous=true&url=resnet-50_compiled.mar"
sleep 10 # allow sufficient time to load model
```

 Run inference using an example image with the following commands: 

```
curl -O https://raw.githubusercontent.com/awslabs/multi-model-server/master/docs/images/kitten_small.jpg
curl -X POST http://127.0.0.1:8080/predictions/resnet-50_compiled -T kitten_small.jpg
```

 Your output should look like the following: 

```
[
  {
    "probability": 0.6388034820556641,
    "class": "n02123045 tabby, tabby cat"
  },
  {
    "probability": 0.16900072991847992,
    "class": "n02123159 tiger cat"
  },
  {
    "probability": 0.12221276015043259,
    "class": "n02124075 Egyptian cat"
  },
  {
    "probability": 0.028706775978207588,
    "class": "n02127052 lynx, catamount"
  },
  {
    "probability": 0.01915954425930977,
    "class": "n02129604 tiger, Panthera tigris"
  }
]
```

 To cleanup after the test, issue a delete command via the RESTful API and stop the model server using the following commands: 

```
curl -X DELETE http://127.0.0.1:8081/models/resnet-50_compiled

multi-model-server --stop
```

 You should see the following output: 

```
{
  "status": "Model \"resnet-50_compiled\" unregistered"
}
Model server stopped.
Found 1 models and 1 NCGs.
Unloading 10001 (MODEL_STATUS_STARTED) :: success
Destroying NCG 1 :: success
```