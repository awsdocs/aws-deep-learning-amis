# PyTorch to ONNX to MXNet Tutorial<a name="tutorial-onnx-pytorch-mxnet"></a>

## ONNX Overview<a name="tutorial-onnx-overview"></a>

The Open Neural Network Exchange \([ONNX](http://onnx.ai/)\) is an open format used to represent deep learning models\. ONNX is supported by Amazon Web Services, Microsoft, Facebook, and several other partners\. You can design, train, and deploy deep learning models with any framework you choose\. The benefit of ONNX models is that they can be moved between frameworks with ease\.

This tutorial shows you how to use the Deep Learning AMI with Conda with ONNX\. By following these steps, you can train a model or load a pre\-trained model from one framework, export this model to ONNX, and then import the model in another framework\.

## ONNX Prerequisites<a name="tutorial-onnx-prereq"></a>

To use this ONNX tutorial, you must have access to a Deep Learning AMI with Conda version 12 or later\. For more information about how to get started with a Deep Learning AMI with Conda, see [Deep Learning AMI with Conda](overview-conda.md)\.

Launch a terminal session with your Deep Learning AMI with Conda to begin the following tutorial\.

## Convert a PyTorch Model to ONNX, then Load the Model into MXNet<a name="tutorial-onnx-pytorch-mxnet-detail"></a>

First, activate the PyTorch environment:

```
$ source activate pytorch_p36
```

Create a new file with your text editor, and use the following program in a script to train a mock model in PyTorch, then export it to the ONNX format\.

```
# Build a Mock Model in PyTorch with a convolution and a reduceMean layer
import torch
import torch.nn as nn
import torch.nn.functional as F
import torch.optim as optim
from torchvision import datasets, transforms
from torch.autograd import Variable
import torch.onnx as torch_onnx

class Model(nn.Module):
    def __init__(self):
        super(Model, self).__init__()
        self.conv = nn.Conv2d(in_channels=3, out_channels=32, kernel_size=(3,3), stride=1, padding=0, bias=False)

    def forward(self, inputs):
        x = self.conv(inputs)
        #x = x.view(x.size()[0], x.size()[1], -1)
        return torch.mean(x, dim=2)

# Use this an input trace to serialize the model
input_shape = (3, 100, 100)
model_onnx_path = "torch_model.onnx"
model = Model()
model.train(False)

# Export the model to an ONNX file
dummy_input = Variable(torch.randn(1, *input_shape))
output = torch_onnx.export(model, 
                          dummy_input, 
                          model_onnx_path, 
                          verbose=False)
print("Export of torch_model.onnx complete!")
```

After you run this script, you will see the newly created \.onnx file in the same directory\. Now, switch to the MXNet Conda environment to load the model with MXNet\.

Next, activate the MXNet environment:

```
$ source deactivate
$ source activate mxnet_p36
```

Create a new file with your text editor, and use the following program in a script to open ONNX format file in MXNet\.

```
import mxnet as mx
from mxnet.contrib import onnx as onnx_mxnet
import numpy as np

# Import the ONNX model into MXNet's symbolic interface
sym, arg, aux = onnx_mxnet.import_model("torch_model.onnx")
print("Loaded torch_model.onnx!")
print(sym.get_internals())
```

After you run this script, MXNet will have loaded the model, and will print some basic model information\.

## ONNX Tutorials<a name="tutorial-onnx-footer"></a>
+ [Chainer to ONNX to CNTK Tutorial](tutorial-onnx-chainer-cntk.md)
+ [Chainer to ONNX to MXNet Tutorial](tutorial-onnx-chainer-mxnet.md)
+ [PyTorch to ONNX to MXNet Tutorial](#tutorial-onnx-pytorch-mxnet)
+ [PyTorch to ONNX to CNTK Tutorial](tutorial-onnx-pytorch-cntk.md)