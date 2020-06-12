# Using PyTorch\-Neuron and the AWS Neuron Compiler<a name="tutorial-inferentia-pytorch-neuron"></a>

The PyTorch\-Neuron compilation API provides a method to compile a model graph that you can run on an AWS Inferentia device\. 

A trained model must be compiled to an Inferentia target before it can be deployed on Inf1 instances\. The following tutorial compiles the torchvision ResNet50 model and exports it as a saved TorchScript module\. This model is then used to run inference\.

For convenience, this tutorial uses an Inf1 instance for both compilation and inference\. In practice, you may compile your model using another instance type, such as the c5 instance family\. You must then deploy your compiled model to the Inf1 inference server\. For more information, see the [AWS Neuron PyTorch SDK Documentation](https://github.com/aws/aws-neuron-sdk/blob/master/docs/pytorch-neuron/tutorial-compile-infer.md)\.

**Topics**
+ [Prerequisites](#tutorial-inferentia-pytorch-neuron-prerequisites)
+ [Activate the Conda Environment](#tutorial-inferentia-pytorch-neuron-activate)
+ [Resnet50 Compilation](#tutorial-inferentia-pytorch-neuron-compilation)
+ [ResNet50 Inference](#tutorial-inferentia-pytorch-neuron-inference)

## Prerequisites<a name="tutorial-inferentia-pytorch-neuron-prerequisites"></a>

Before using this tutorial, you should have completed the set up steps in [Using the DLAMI with AWS Neuron](tutorial-inferentia-using.md)\. You should also have a familiarity with deep learning and using the DLAMI\. 

## Activate the Conda Environment<a name="tutorial-inferentia-pytorch-neuron-activate"></a>

Activate the PyTorch\-Neuron conda environment using the following command: 

```
source activate aws_neuron_pytorch_p36
```

To exit the current conda environment, run: 

```
source deactivate
```

## Resnet50 Compilation<a name="tutorial-inferentia-pytorch-neuron-compilation"></a>

Create a Python script called **pytorch\_trace\_resnet50\.py** with the following content\. This script uses the PyTorch\-Neuron compilation Python API to compile a ResNet\-50 model\. 

```
import torch
import numpy as np
import os
import torch_neuron
from torchvision import models

image = torch.zeros([1, 3, 224, 224], dtype=torch.float32)

## Load a pretrained ResNet50 model
model = models.resnet50(pretrained=True)

## Tell the model we are using it for evaluation (not training)
model.eval()
model_neuron = torch.neuron.trace(model, example_inputs=[image])

## Export to saved model
model_neuron.save("resnet50_neuron.pt")
```

Run the compilation script\.

```
python pytorch_trace_resnet50.py
```

Compilation will take a few minutes\. When compilation has finished, the compiled model is saved as `resnet50_neuron.pt` in the local directory\.

## ResNet50 Inference<a name="tutorial-inferentia-pytorch-neuron-inference"></a>

Create a Python script called **pytorch\_infer\_resnet50\.py** with the following content\. This script downloads a sample image and uses it to run inference with the compiled model\. 

```
import os
import time
import torch
import torch_neuron
import json
import numpy as np

from urllib import request

from torchvision import models, transforms, datasets

## Create an image directory containing a small kitten
os.makedirs("./torch_neuron_test/images", exist_ok=True)
request.urlretrieve("https://raw.githubusercontent.com/awslabs/mxnet-model-server/master/docs/images/kitten_small.jpg",
                    "./torch_neuron_test/images/kitten_small.jpg")


## Fetch labels to output the top classifications
request.urlretrieve("https://s3.amazonaws.com/deep-learning-models/image-models/imagenet_class_index.json","imagenet_class_index.json")
idx2label = []

with open("imagenet_class_index.json", "r") as read_file:
    class_idx = json.load(read_file)
    idx2label = [class_idx[str(k)][1] for k in range(len(class_idx))]

## Import a sample image and normalize it into a tensor
normalize = transforms.Normalize(
    mean=[0.485, 0.456, 0.406],
    std=[0.229, 0.224, 0.225])

eval_dataset = datasets.ImageFolder(
    os.path.dirname("./torch_neuron_test/"),
    transforms.Compose([
    transforms.Resize([224, 224]),
    transforms.ToTensor(),
    normalize,
    ])
)

image, _ = eval_dataset[0]
image = torch.tensor(image.numpy()[np.newaxis, ...])

## Load model
model_neuron = torch.jit.load( 'resnet50_neuron.pt' )

## Predict
results = model_neuron( image )

# Get the top 5 results
top5_idx = results[0].sort()[1][-5:]

# Lookup and print the top 5 labels
top5_labels = [idx2label[idx] for idx in top5_idx]

print("Top 5 labels:\n {}".format(top5_labels) )
```

Run inference with the compiled model using the following command: 

```
python pytorch_infer_resnet50.py
```

Your output should look like the following: 

```
Top 5 labels:
 ['tiger', 'lynx', 'tiger_cat', 'Egyptian_cat', 'tabby']
```