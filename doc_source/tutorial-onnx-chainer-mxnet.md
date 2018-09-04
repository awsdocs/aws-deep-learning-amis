# Chainer to ONNX to MXNet Tutorial<a name="tutorial-onnx-chainer-mxnet"></a>

## ONNX Overview<a name="tutorial-onnx-overview"></a>

The Open Neural Network Exchange \([ONNX](http://onnx.ai/)\) is an open format used to represent deep learning models\. ONNX is supported by Amazon Web Services, Microsoft, Facebook, and several other partners\. You can design, train, and deploy deep learning models with any framework you choose\. The benefit of ONNX models is that they can be moved between frameworks with ease\.

This tutorial shows you how to use the Deep Learning AMI with Conda with ONNX\. By following these steps, you can train a model or load a pre\-trained model from one framework, export this model to ONNX, and then import the model in another framework\.

## ONNX Prerequisites<a name="tutorial-onnx-prereq"></a>

To use this ONNX tutorial, you must have access to a Deep Learning AMI with Conda version 12 or later\. For more information about how to get started with a Deep Learning AMI with Conda, see [Deep Learning AMI with Conda](overview-conda.md)\.

Launch a terminal session with your Deep Learning AMI with Conda to begin the following tutorial\.

## Convert a Chainer Model to ONNX, then Load the Model into MXNet<a name="tutorial-onnx-chainer-mxnet-detail"></a>

First, activate the Chainer environment:

```
$ source activate chainer_p36
```

Create a new file with your text editor, and use the following program in a script to fetch a model from Chainer's model zoo, then export it to the ONNX format\.

```
import numpy as np
import chainer
import chainercv.links as L
import onnx_chainer

# Fetch a vgg16 model
model = L.VGG16(pretrained_model='imagenet')
# Export the model to a .onnx file
out = onnx_chainer.export(model, x, filename='vgg16.onnx')

# Check that the newly created model is valid and meets ONNX specification.
import onnx
model_proto = onnx.load("vgg16.onnx")
onnx.checker.check_model(model_proto)
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
sym, arg, aux = onnx_mxnet.import_model("vgg16.onnx")
print("Loaded vgg16.onnx!") 
print(sym.get_internals())
```

After you run this script, MXNet will have loaded the model\.

## ONNX Tutorials<a name="tutorial-onnx-footer"></a>
+ [Chainer to ONNX to CNTK Tutorial](tutorial-onnx-chainer-cntk.md)
+ [Chainer to ONNX to MXNet Tutorial](#tutorial-onnx-chainer-mxnet)
+ [PyTorch to ONNX to MXNet Tutorial](tutorial-onnx-pytorch-mxnet.md)
+ [PyTorch to ONNX to CNTK Tutorial](tutorial-onnx-pytorch-cntk.md)