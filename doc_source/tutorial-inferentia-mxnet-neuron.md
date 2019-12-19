# Using MXNet\-Neuron and the AWS Neuron Compiler<a name="tutorial-inferentia-mxnet-neuron"></a>

The MXNet\-Neuron compilation API provides a method to compile a model graph that you can run on an AWS Inferentia device\. 

 In this example, you use the API to compile a ResNet\-50 model and use it to run inference\. 

 For more information about the Neuron SDK, see the [AWS Neuron SDK documentation](https://github.com/aws/aws-neuron-sdk)\. 

**Topics**
+ [Prerequisites](#tutorial-inferentia-mxnet-neuron-prerequisites)
+ [Activate the Conda Environment](#tutorial-inferentia-mxnet-neuron-activate)
+ [Resnet50 Compilation](#tutorial-inferentia-mxnet-neuron-compilation)
+ [ResNet50 Inference](#tutorial-inferentia-mxnet-neuron-inference)

## Prerequisites<a name="tutorial-inferentia-mxnet-neuron-prerequisites"></a>

 Before using this tutorial, you should have completed the set up steps in [Using the DLAMI with AWS Neuron](tutorial-inferentia-using.md)\. You should also have a familiarity with deep learning and using the DLAMI\. 

## Activate the Conda Environment<a name="tutorial-inferentia-mxnet-neuron-activate"></a>

 Activate the MXNet\-Neuron conda environment using the following command: 

```
source activate aws_neuron_mxnet_p36
```

 Update the Neuron package using the following command: 

```
conda update mxnet-neuron
```

To exit the current conda environment, run: 

```
source deactivate
```

## Resnet50 Compilation<a name="tutorial-inferentia-mxnet-neuron-compilation"></a>

Create a Python script called **mxnet\_compile\_resnet50\.py** with the following content\. This script uses the MXNet\-Neuron compilation Python API to compile a ResNet\-50 model\. 

```
import mxnet as mx
import numpy as np

print("downloading...")
path='http://data.mxnet.io/models/imagenet/'
mx.test_utils.download(path+'resnet/50-layers/resnet-50-0000.params')
mx.test_utils.download(path+'resnet/50-layers/resnet-50-symbol.json')
print("download finished.")

sym, args, aux = mx.model.load_checkpoint('resnet-50', 0)

print("compile for inferentia using neuron... this will take a few minutes...")
inputs = { "data" : mx.nd.ones([1,3,224,224], name='data', dtype='float32') }

sym, args, aux = mx.contrib.neuron.compile(sym, args, aux, inputs)

print("save compiled model...")
mx.model.save_checkpoint("compiled_resnet50", 0, sym, args, aux)
```

 Compile the model using the following command: 

```
python mxnet_compile_resnet50.py
```

 Compilation will take a few minutes\. When compilation has finished, the following files will be in your current directory: 

```
resnet-50-0000.params
resnet-50-symbol.json
compiled_resnet50-0000.params
compiled_resnet50-symbol.json
```

## ResNet50 Inference<a name="tutorial-inferentia-mxnet-neuron-inference"></a>

Create a Python script called **mxnet\_infer\_resnet50\.py** with the following content\. This script downloads a sample image and uses it to run inference with the inference model with the compiled model\. 

```
import mxnet as mx
import numpy as np

path='http://data.mxnet.io/models/imagenet/'
mx.test_utils.download(path+'synset.txt')

fname = mx.test_utils.download('https://raw.githubusercontent.com/awslabs/mxnet-model-server/master/docs/images/kitten_small.jpg')
img = mx.image.imread(fname)

# convert into format (batch, RGB, width, height)
img = mx.image.imresize(img, 224, 224) 
# resize
img = img.transpose((2, 0, 1)) 
# Channel first
img = img.expand_dims(axis=0) 
# batchify
img = img.astype(dtype='float32')

sym, args, aux = mx.model.load_checkpoint('compiled_resnet50', 0)
softmax = mx.nd.random_normal(shape=(1,))
args['softmax_label'] = softmax
args['data'] = img
# Inferentia context
ctx = mx.neuron()

exe = sym.bind(ctx=ctx, args=args, aux_states=aux, grad_req='null')
with open('synset.txt', 'r') as f:
    labels = [l.rstrip() for l in f]

exe.forward(data=img)
prob = exe.outputs[0].asnumpy()
# print the top-5
prob = np.squeeze(prob)
a = np.argsort(prob)[::-1] 
for i in a[0:5]:
    print('probability=%f, class=%s' %(prob[i], labels[i]))
```

 Run inference with the compiled model using the following command: 

```
python mxnet_infer_resnet50.py
```

 Your output should look like the following: 

```
probability=0.642454, class=n02123045 tabby, tabby cat
probability=0.189407, class=n02123159 tiger cat
probability=0.100798, class=n02124075 Egyptian cat
probability=0.030649, class=n02127052 lynx, catamount
probability=0.016278, class=n02129604 tiger, Panthera tigris
```

**Next Step**  
[Using MXNet\-Neuron Model Serving](tutorial-inferentia-mxnet-neuron-serving.md)