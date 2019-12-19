# Using TensorFlow\-Neuron and the AWS Neuron Compiler<a name="tutorial-inferentia-tf-neuron"></a>

 This tutorial shows how to use the AWS Neuron compiler to compile the Keras ResNet\-50 model and export it as a saved model in SavedModel format\. This format is a typical TensorFlow model interchangeable format\. You also learn how to run inference on an Inf1 instance with example input\.  

 For more information about the Neuron SDK, see the [AWS Neuron SDK documentation](https://github.com/aws/aws-neuron-sdk)\. 

**Topics**
+ [Prerequisites](#tutorial-inferentia-tf-neuron-prerequisites)
+ [Activate the Conda environment](#tutorial-inferentia-tf-neuron-activate)
+ [Resnet50 Compilation](#tutorial-inferentia-tf-neuron-compilation)
+ [ResNet50 Inference](#tutorial-inferentia-tf-neuron-inference)

## Prerequisites<a name="tutorial-inferentia-tf-neuron-prerequisites"></a>

 Before using this tutorial, you should have completed the set up steps in [Using the DLAMI with AWS Neuron](tutorial-inferentia-using.md)\. You should also have a familiarity with deep learning and using the DLAMI\. 

## Activate the Conda environment<a name="tutorial-inferentia-tf-neuron-activate"></a>

 Activate the TensorFlow\-Neuron conda environment using the following command: 

```
source activate aws_neuron_tensorflow_p36
```

 Update the Neuron package using the following command: 

```
conda update tensorflow-neuron
```

 To exit the current conda environment, run the following command: 

```
source deactivate
```

## Resnet50 Compilation<a name="tutorial-inferentia-tf-neuron-compilation"></a>

Create a Python script called **tensorflow\_compile\_resnet50\.py** that has the following content\. This Python script compiles the Keras ResNet50 model and exports it as a saved model\. 

```
import os
import time
import shutil
import tensorflow as tf
import tensorflow.neuron as tfn
import tensorflow.compat.v1.keras as keras
from tensorflow.keras.applications.resnet50 import ResNet50
from tensorflow.keras.applications.resnet50 import preprocess_input

# Create a workspace
WORKSPACE = './ws_resnet50'
os.makedirs(WORKSPACE, exist_ok=True)

# Prepare export directory (old one removed)
model_dir = os.path.join(WORKSPACE, 'resnet50')
compiled_model_dir = os.path.join(WORKSPACE, 'resnet50_neuron')
shutil.rmtree(model_dir, ignore_errors=True)
shutil.rmtree(compiled_model_dir, ignore_errors=True)

# Instantiate Keras ResNet50 model
keras.backend.set_learning_phase(0)
model = ResNet50(weights='imagenet')

# Export SavedModel
tf.saved_model.simple_save(
 session            = keras.backend.get_session(),
 export_dir         = model_dir,
 inputs             = {'input': model.inputs[0]},
 outputs            = {'output': model.outputs[0]})

# Compile using Neuron
tfn.saved_model.compile(model_dir, compiled_model_dir)

# Prepare SavedModel for uploading to Inf1 instance
shutil.make_archive(compiled_model_dir, 'zip', WORKSPACE, 'resnet50_neuron')
```

 Compile the model using the following command: 

```
python tensorflow_compile_resnet50.py
```

The compilation process will take a few minutes\. When it completes, your output should look like the following: 

```
...
INFO:tensorflow:fusing subgraph neuron_op_d6f098c01c780733 with neuron-cc
INFO:tensorflow:Number of operations in TensorFlow session: 4638
INFO:tensorflow:Number of operations after tf.neuron optimizations: 556
INFO:tensorflow:Number of operations placed on Neuron runtime: 554
INFO:tensorflow:Successfully converted ./ws_resnet50/resnet50 to ./ws_resnet50/resnet50_neuron
...
```

 ​ 

 After compilation, the saved model is zipped at **ws\_resnet50/resnet50\_neuron\.zip**\. Unzip the model and download the sample image for inference using the following commands: 

```
unzip ws_resnet50/resnet50_neuron.zip -d .
curl -O https://raw.githubusercontent.com/awslabs/mxnet-model-server/master/docs/images/kitten_small.jpg
```

## ResNet50 Inference<a name="tutorial-inferentia-tf-neuron-inference"></a>

Create a Python script called **tensorflow\_infer\_resnet50\.py**  that has the following content\. This script runs inference on the downloaded model using a previously compiled inference model\. 

```
import os
import numpy as np
import tensorflow as tf
from tensorflow.keras.preprocessing import image
from tensorflow.keras.applications import resnet50

# Create input from image
img_sgl = image.load_img('kitten_small.jpg', target_size=(224, 224))
img_arr = image.img_to_array(img_sgl)
img_arr2 = np.expand_dims(img_arr, axis=0)
img_arr3 = resnet50.preprocess_input(img_arr2)
# Load model
COMPILED_MODEL_DIR = './resnet50_neuron/'
predictor_inferentia = tf.contrib.predictor.from_saved_model(COMPILED_MODEL_DIR)
# Run inference
model_feed_dict={'input': img_arr3}
infa_rslts = predictor_inferentia(model_feed_dict);
# Display results
print(resnet50.decode_predictions(infa_rslts["output"], top=5)[0])
```

 Run inference on the model using the following command: 

```
python tensorflow_infer_resnet50.py
```

 Your output should look like the following: 

```
...
[('n02123045', 'tabby', 0.6918919), ('n02127052', 'lynx', 0.12770271), ('n02123159', 'tiger_cat', 0.08277027), ('n02124075', 'Egyptian_cat', 0.06418919), ('n02128757', 'snow_leopard', 0.009290541)]
```

**Next Step**  
[Using AWS Neuron TensorFlow Serving](tutorial-inferentia-tf-neuron-serving.md)