# Apache MXNet to ONNX to CNTK Tutorial<a name="tutorial-onnx-mxnet-cntk"></a>

## ONNX Overview<a name="tutorial-onnx-overview"></a>

The Open Neural Network Exchange \([ONNX](http://onnx.ai/)\) is an open format used to represent deep learning models\. ONNX is supported by Amazon Web Services, Microsoft, Facebook, and several other partners\. You can design, train, and deploy deep learning models with any framework you choose\. The benefit of ONNX models is that they can be moved between frameworks with ease\.

This tutorial shows you how to use the Deep Learning AMI with Conda with ONNX\. By following these steps, you can train a model or load a pre\-trained model from one framework, export this model to ONNX, and then import the model in another framework\.

## ONNX Prerequisites<a name="tutorial-onnx-prereq"></a>

To use this ONNX tutorial, you must have access to a Deep Learning AMI with Conda version 12 or later\. For more information about how to get started with a Deep Learning AMI with Conda, see [Deep Learning AMI with Conda](overview-conda.md)\.

**Important**  
These examples use functions that might require up to 8 GB of memory \(or more\)\. Be sure to choose an instance type with enough memory\.

Launch a terminal session with your Deep Learning AMI with Conda to begin the following tutorial\.

## Convert an Apache MXNet \(incubating\) Model to ONNX, then Load the Model into CNTK<a name="tutorial-onnx-mxnet-cntk-detail"></a>

**How to Export a Model from Apache MXNet \(incubating\)**

You can install the latest MXNet build into either or both of the MXNet Conda environments on your Deep Learning AMI with Conda\.

1. 
   + \(Option for Python 3\) \- Activate the Python 3 MXNet environment:

     ```
     $ source activate mxnet_p36
     ```
   + \(Option for Python 2\) \- Activate the Python 2 MXNet environment:

     ```
     $ source activate mxnet_p27
     ```

1. The remaining steps assume that you are using the `mxnet_p36` environment\.

1. Download the model files\.

   ```
   curl -O https://s3.amazonaws.com/onnx-mxnet/model-zoo/vgg16/vgg16-symbol.json
   curl -O https://s3.amazonaws.com/onnx-mxnet/model-zoo/vgg16/vgg16-0000.params
   ```

1. To export the model files from MXNet to the ONNX format, create a new file with your text editor and use the following program in a script\.

   ```
   import numpy as np
   import mxnet as mx
   from mxnet.contrib import onnx as onnx_mxnet
   converted_onnx_filename='vgg16.onnx'
   
   # Export MXNet model to ONNX format via MXNet's export_model API
   converted_onnx_filename=onnx_mxnet.export_model('vgg16-symbol.json', 'vgg16-0000.params', [(1,3,224,224)], np.float32, converted_onnx_filename)
   
   # Check that the newly created model is valid and meets ONNX specification.
   import onnx
   model_proto = onnx.load(converted_onnx_filename)
   onnx.checker.check_model(model_proto)
   ```

   You may see some warning messages, but you can safely ignore those for now\. After you run this script, you will see the newly created \.onnx file in the same directory\. 

1. Now that you have an ONNX file you can try running inference with it with the following example:
   + [Use CNTK for Inference with an ONNX Model](tutorial-cntk-inference-onnx.md)

## ONNX Tutorials<a name="tutorial-onnx-footer"></a>
+ [Apache MXNet to ONNX to CNTK Tutorial](#tutorial-onnx-mxnet-cntk)
+ [Chainer to ONNX to CNTK Tutorial](tutorial-onnx-chainer-cntk.md)
+ [Chainer to ONNX to MXNet Tutorial](tutorial-onnx-chainer-mxnet.md)
+ [PyTorch to ONNX to MXNet Tutorial](tutorial-onnx-pytorch-mxnet.md)
+ [PyTorch to ONNX to CNTK Tutorial](tutorial-onnx-pytorch-cntk.md)