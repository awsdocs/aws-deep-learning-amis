# Using AWS Neuron TensorFlow Serving<a name="tutorial-inferentia-tf-neuron-serving"></a>

This tutorial shows how to construct a graph and add an AWS Neuron compilation step before exporting the saved model to use with TensorFlow Serving\. TensorFlow Serving is a serving system that allows you to scale\-up inference across a network\. Neuron TensorFlow Serving uses the same API as normal TensorFlow Serving\. The only difference is that a saved model must be compiled for AWS Inferentia and the entry point is a different binary named `tensorflow_model_server_neuron`\. The binary is found at `/usr/local/bin/tensorflow_model_server_neuron` and is pre\-installed in the DLAMI\. 

 For more information about the Neuron SDK, see the [AWS Neuron SDK documentation](https://github.com/aws/aws-neuron-sdk)\. 

**Topics**
+ [Prerequisites](#tutorial-inferentia-tf-neuron--serving-prerequisites)
+ [Activate the Conda environment](#tutorial-inferentia-tf-neuron-serving-activate)
+ [Compile and Export the Saved Model](#tutorial-inferentia-tf-neuron-serving-compile)
+ [Serving the Saved Model](#tutorial-inferentia-tf-neuron-serving-serving)
+ [Generate inference requests to the model server](#tutorial-inferentia-tf-neuron-serving-inference)

## Prerequisites<a name="tutorial-inferentia-tf-neuron--serving-prerequisites"></a>

 Before using this tutorial, you should have completed the set up steps in [Using the DLAMI with AWS Neuron](tutorial-inferentia-using.md)\. You should also have a familiarity with deep learning and using the DLAMI\. 

## Activate the Conda environment<a name="tutorial-inferentia-tf-neuron-serving-activate"></a>

 Activate the TensorFlow\-Neuron conda environment using the following command: 

```
source activate aws_neuron_tensorflow_p36
```

 Update the Neuron package using the following command: 

```
conda update tensorflow-neuron
```

 If you need to exit the current conda environment, run: 

```
source deactivate
```

## Compile and Export the Saved Model<a name="tutorial-inferentia-tf-neuron-serving-compile"></a>

Create a Python script called `tensorflow-model-server-compile.py` with the following content\. This script constructs a graph and compiles it using Neuron\. It then exports the compiled graph as a saved model\.  

```
import tensorflow as tf
import os

tf.keras.backend.set_learning_phase(0)
model = tf.keras.applications.ResNet50(weights='imagenet')
sess = tf.keras.backend.get_session()
inputs = {'input': model.inputs[0]}
outputs = {'output': model.outputs[0]}

# save the model using tf.saved_model.simple_save
modeldir = "./resnet50/1"
tf.saved_model.simple_save(sess, modeldir, inputs, outputs)

# compile the model for Inferentia
neuron_modeldir = os.path.join(os.path.expanduser('~'), 'resnet50_inf1', '1')
tf.neuron.saved_model.compile(modeldir, neuron_modeldir, batch_size=1)
```

 Compile the model using the following command: 

```
python tensorflow-model-server-compile.py
```

 Your output should look like the following: 

```
...
INFO:tensorflow:fusing subgraph neuron_op_d6f098c01c780733 with neuron-cc
INFO:tensorflow:Number of operations in TensorFlow session: 4638
INFO:tensorflow:Number of operations after tf.neuron optimizations: 556
INFO:tensorflow:Number of operations placed on Neuron runtime: 554
INFO:tensorflow:Successfully converted ./resnet50/1 to /home/ubuntu/resnet50_inf1/1
```

## Serving the Saved Model<a name="tutorial-inferentia-tf-neuron-serving-serving"></a>

Once the model has been compiled, you can use the following command to serve the saved model with the tensorflow\_model\_server\_neuron binary: 

```
tensorflow_model_server_neuron --model_name=resnet50_inf1 \
    --model_base_path=$HOME/resnet50_inf1/ --port=8500 &
```

 Your output should look like the following\. The compiled model is staged in the Inferentia device’s DRAM by the server to prepare for inference\. 

```
...
2019-11-22 01:20:32.075856: I external/org_tensorflow/tensorflow/cc/saved_model/loader.cc:311] SavedModel load for tags { serve }; Status: success. Took 40764 microseconds.
2019-11-22 01:20:32.075888: I tensorflow_serving/servables/tensorflow/saved_model_warmup.cc:105] No warmup data file found at /home/ubuntu/resnet50_inf1/1/assets.extra/tf_serving_warmup_requests
2019-11-22 01:20:32.075950: I tensorflow_serving/core/loader_harness.cc:87] Successfully loaded servable version {name: resnet50_inf1 version: 1}
2019-11-22 01:20:32.077859: I tensorflow_serving/model_servers/server.cc:353] Running gRPC ModelServer at 0.0.0.0:8500 ...
```

## Generate inference requests to the model server<a name="tutorial-inferentia-tf-neuron-serving-inference"></a>

Create a Python script called `tensorflow-model-server-infer.py` with the following content\. This script runs inference via gRPC, which is service framework\. 

```
import numpy as np
import grpc
import tensorflow as tf
from tensorflow.keras.preprocessing import image
from tensorflow.keras.applications.resnet50 import preprocess_input
from tensorflow_serving.apis import predict_pb2
from tensorflow_serving.apis import prediction_service_pb2_grpc
from tensorflow.keras.applications.resnet50 import decode_predictions

if __name__ == '__main__':
    channel = grpc.insecure_channel('localhost:8500')
    stub = prediction_service_pb2_grpc.PredictionServiceStub(channel)
    img_file = tf.keras.utils.get_file(
        "./kitten_small.jpg",
        "https://raw.githubusercontent.com/awslabs/mxnet-model-server/master/docs/images/kitten_small.jpg")
    img = image.load_img(img_file, target_size=(224, 224))
    img_array = preprocess_input(image.img_to_array(img)[None, ...])
    request = predict_pb2.PredictRequest()
    request.model_spec.name = 'resnet50_inf1'
    request.inputs['input'].CopyFrom(
        tf.contrib.util.make_tensor_proto(img_array, shape=img_array.shape))
    result = stub.Predict(request)
    prediction = tf.make_ndarray(result.outputs['output'])
    print(decode_predictions(prediction))
```

 Run inference on the model by using gRPC with the following command: 

```
python tensorflow-model-server-infer.py
```

 Your output should look like the following: 

```
[[('n02123045', 'tabby', 0.6918919), ('n02127052', 'lynx', 0.12770271), ('n02123159', 'tiger_cat', 0.08277027), ('n02124075', 'Egyptian_cat', 0.06418919), ('n02128757', 'snow_leopard', 0.009290541)]]
```