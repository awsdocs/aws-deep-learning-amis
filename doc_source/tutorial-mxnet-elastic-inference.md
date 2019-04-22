# Use Elastic Inference with MXNet<a name="tutorial-mxnet-elastic-inference"></a>

## About Elastic Inference<a name="tutorial-mxnet-elastic-inference-overview"></a>

[Elastic Inference](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/elastic-inference.html) \(EI\) is available only on instances that were launched with an Elastic Inference Accelerator\.

Review [Selecting the Instance Type for DLAMI](instance-select.md) to select your desired instance type, and also review [Elastic Inference Prerequisites](ei-prerequisites.md) for the instructions related to Elastic Inference\. For detailed instructions on how to launch a DLAMI with an Elastic Inference Accelerator, see the [Elastic Inference](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/elastic-inference.html) documentation\.

To use an existing MXNet inference script, make one change in the code\. Wherever you set the context to bind your model, such as `mx.cpu()` or `mx.gpu()`, update this to use `mx.eia()` instead\. The following is an example that has this one modification, compared to the tutorial, [Use Elastic Inference with MXNet](#tutorial-mxnet-elastic-inference), where the example runs on the local CPU\. 

### Activate the MXNet EI Environment<a name="tutorial-mxnet-elastic-inference-activate"></a>

1. Log in to your Deep Learning AMI with Conda console to begin the following tutorial\. Depending on your version of Python, do one of the following:
   + Option for Python 3 \- Activate the Python 3 MXNet EI environment\.

     ```
     $ source activate amazonei_mxnet_p36
     ```
   + Option for Python 2 \- Activate the Python 2 MXNet EI environment\.

     ```
     $ source activate amazonei_mxnet_p27
     ```

1. Verify that you've properly set up your instance with EI\.

   ```
   $ python ~/anaconda3/bin/EISetupValidator.py
   ```

   If your instance is not properly set up with an accelerator, running any of the examples in this section will result in the following error:

   ```
   Error: Failed to query accelerator metadata.
   Failed to detect any accelerator
   ```

   For detailed instructions on how to launch a DLAMI with an Elastic Inference Accelerator, see the [Elastic Inference](https://docs.aws.amazon.com/dlami/latest/devguide/ei-prerequisites.html) documentation\.

1. The remaining parts of this guide assume you are using the `amazonei_mxnet_p36` environment\.

**Note**  
If you are switching between MXNet or TensorFlow Elastic Inference environments, you must Stop and then Start your instance to reattach the Elastic Inference Accelerator\. Rebooting is not sufficient since the process requires a complete shutdown\.

### Using EI with MXNet<a name="ei-mxnet"></a>

The EI\-enabled version of MXNet lets you use EI seamlessly with few changes to your MXNet code\. You can use EI with any of the following MXNet APIs:
+ MXNet Python Symbol API
+ MXNet Python Module API

### Use EI with the MXNet Symbol API<a name="ei-mxnet-symbol"></a>

Pass `mx.eia()` as the context in a call to either the `simple_bind()` or the `bind()` methods\. For information about the MXNet Symbol API, see [https://mxnet\.incubator\.apache\.org/api/python/symbol/symbol\.html](https://mxnet.incubator.apache.org/api/python/symbol/symbol.html)\.

The EI environment is not compiled with CUDA, so GPU context is not supported\. Check for this in your code to make sure that you don't accidentally use the GPU context\. For example, NDArray computations should be handled by the `mx.cpu()` context when using EI\. You use the `mx.eia()` context only with the bind call\. The following example calls the `simple_bind()` method with the `mx.eia()` context\. 

```
import mxnet as mx

data = mx.sym.var('data', shape=(1,))
sym = mx.sym.exp(data)

# Pass mx.eia() as context during simple bind operation

executor = sym.simple_bind(ctx=mx.eia(), grad_req='null')
for i in range(10):

  # Forward call is performed on remote accelerator
  executor.forward(data=mx.nd.ones((1,)))
  print('Inference %d, output = %s' % (i, executor.outputs[0]))
```

The following example calls the `bind()` method\.

```
import mxnet as mx
a = mx.sym.Variable('a')
b = mx.sym.Variable('b')
c = 2 * a + b
# Even for execution of inference workloads on eia,
# context for input ndarrays to be mx.cpu()
a_data = mx.nd.array([1,2], ctx=mx.cpu())
b_data = mx.nd.array([2,3], ctx=mx.cpu())
# Then in the bind call, use the mx.eia() context
e = c.bind(mx.eia(), {'a': a_data, 'b': b_data})

# Forward call is performed on remote accelerator
e.forward()
print('1st Inference, output = %s' % (executor.outputs[0]))
# Subsequent calls can pass new data in a forward call
e.forward(a=mx.nd.ones((2,)), b=mx.nd.ones((2,)))
print('2nd Inference, output = %s' % (executor.outputs[0]))
```

### Use EI with the Symbol API and a ResNet\-50 Model<a name="tutorial-mxnet-elastic-inference-symbol-resnet50"></a>

The following example calls the `bind()` method on a pre\-trained real model \(Resnet\-50\) from the Symbol API\.

1. Use your preferred text editor to create a script called `mxnet_resnet50.py` that has the following content\. This script downloads the ResNet\-50 model files \(resnet\-50\-0000\.params and resnet\-50\-symbol\.json\)\. The script also downloads labels list \(synset\.txt\)\. Download a cat image to get a prediction result from the pre\-trained model, then look this up in the result in labels list, returning a prediction result\.

   ```
   import mxnet as mx
   import numpy as np
   
   path='http://data.mxnet.io/models/imagenet/'
   [mx.test_utils.download(path+'resnet/50-layers/resnet-50-0000.params'),
   mx.test_utils.download(path+'resnet/50-layers/resnet-50-symbol.json'),
   mx.test_utils.download(path+'synset.txt')]
   
   ctx = mx.eia()
   
   with open('synset.txt', 'r') as f:
     labels = [l.rstrip() for l in f]
   
   sym, args, aux = mx.model.load_checkpoint('resnet-50', 0)
   
   fname = mx.test_utils.download('https://github.com/dmlc/web-data/blob/master/mxnet/doc/tutorials/python/predict_image/cat.jpg?raw=true')
   img = mx.image.imread(fname)
   # convert into format (batch, RGB, width, height)
   img = mx.image.imresize(img, 224, 224) # resize
   img = img.transpose((2, 0, 1)) # Channel first
   img = img.expand_dims(axis=0) # batchify
   img = img.astype(dtype='float32')
   args['data'] = img
   
   softmax = mx.nd.random_normal(shape=(1,))
   args['softmax_label'] = softmax
   
   exe = sym.bind(ctx=ctx, args=args, aux_states=aux, grad_req='null')
   
   exe.forward(data=img)
   prob = exe.outputs[0].asnumpy()
   # print the top-5
   prob = np.squeeze(prob)
   a = np.argsort(prob)[::-1]
   for i in a[0:5]:
     print('probability=%f, class=%s' %(prob[i], labels[i]))
   ```

1. Then run the script, and you should see something similar to the following output\. MXNet will optimize the model graph for Elastic Inference, load it on Elastic Inference Accelerator, and then run inference against it:

   ```
   (amazonei_mxnet_p36) ubuntu@ip-172-31-42-83:~$ python mxnet_resnet50.py
     [23:12:03] src/nnvm/legacy_json_util.cc:209: Loading symbol saved by previous version v0.8.0. Attempting to upgrade...
     [23:12:03] src/nnvm/legacy_json_util.cc:217: Symbol successfully upgraded!
     Using Amazon Elastic Inference Client Library Version: 1.2.8
     Number of Elastic Inference Accelerators Available: 1
     Elastic Inference Accelerator ID: eia-95ae5a472b2241769656dbb5d344a80e
     Elastic Inference Accelerator Type: eia1.large
   
     probability=0.418679, class=n02119789 kit fox, Vulpes macrotis
     probability=0.293495, class=n02119022 red fox, Vulpes vulpes
     probability=0.029321, class=n02120505 grey fox, gray fox, Urocyon cinereoargenteus
     probability=0.026230, class=n02124075 Egyptian cat
     probability=0.022557, class=n02085620 Chihuahua
   ```

### Use EI with the Module API and a ResNet\-152 Model<a name="tutorial-mxnet-elastic-inference-module-resnet152"></a>

The following example uses EI with the Module API on a pre\-trained real model \(Resnet\-152\)\.

1. The following script downloads two ResNet\-152 model files \(resnet\-152\-0000\.params and resnet\-152\-symbol\.json\) and a labels list \(synset\.txt\)\. It also downloads a cat image to get a prediction result from the pre\-trained model, then looks this up in the result in labels list, returning a prediction result\. Use your preferred text editor to create a script using the following content\.

   ```
   import mxnet as mx
   import numpy as np
   from collections import namedtuple
   Batch = namedtuple('Batch', ['data'])
   
   path='http://data.mxnet.io/models/imagenet/'
   [mx.test_utils.download(path+'resnet/152-layers/resnet-152-0000.params'),
   mx.test_utils.download(path+'resnet/152-layers/resnet-152-symbol.json'),
   mx.test_utils.download(path+'synset.txt')]
   
   ctx = mx.eia()
   
   sym, arg_params, aux_params = mx.model.load_checkpoint('resnet-152', 0)
   mod = mx.mod.Module(symbol=sym, context=ctx, label_names=None)
   mod.bind(for_training=False, data_shapes=[('data', (1,3,224,224))],
        label_shapes=mod._label_shapes)
   mod.set_params(arg_params, aux_params, allow_missing=True)
   with open('synset.txt', 'r') as f:
     labels = [l.rstrip() for l in f]
   
   fname = mx.test_utils.download('https://github.com/dmlc/web-data/blob/master/mxnet/doc/tutorials/python/predict_image/cat.jpg?raw=true')
   img = mx.image.imread(fname)
   
   # convert into format (batch, RGB, width, height)
   img = mx.image.imresize(img, 224, 224) # resize
   img = img.transpose((2, 0, 1)) # Channel first
   img = img.expand_dims(axis=0) # batchify
   
   mod.forward(Batch([img]))
   prob = mod.get_outputs()[0].asnumpy()
   # print the top-5
   prob = np.squeeze(prob)
   a = np.argsort(prob)[::-1]
   for i in a[0:5]:
     print('probability=%f, class=%s' %(prob[i], labels[i]))
   ```

   Save this script as test\.py

1. Then run the script, and you should see a result similar to the Symbol API ResNet\-50 example output\.

### Considerations When Using EI\-enabled MXNet<a name="ei-mxnet-considerations"></a>
+ MXNet EI is built with MKLDNN, so all operations when using the `mx.cpu()` context are supported\. MXNet EI doesn't support the `mx.gpu()` context, which means that no operations can be performed on an attached GPU\. Any attempt to use a GPU context throws an error\.
+ EI is not currently supported for either MXNet Imperative mode or the MXNet Gluon API, however, Gluon models can be [hybridized as symbolic and exported](https://gluon.mxnet.io/chapter07_distributed-learning/hybridize.html#Getting-the-best-of-both-worlds-with-MXNet-Gluon%E2%80%99s-HybridBlocks)\.
+ You cannot allocate memory for NDArray on the remote accelerator by writing something like this: `x = mx.nd.array([[1, 2], [3, 4]], ctx=mx.eia())`\. This throws an error\. Instead you should: use `x = mx.nd.array([[1, 2], [3, 4]], ctx=mx.cpu())`\. MXNet automatically transfers your data to the accelerator as necessary, so there is no need to worry about that\. Sample error message:

  ```
  >>> mx.nd.array([1,2],ctx=mx.eia())
  Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
    File "/home/ec2-user/.local/lib/python3.6/site-packages/mxnet/ndarray/utils.py", line 146, in array
      return _array(source_array, ctx=ctx, dtype=dtype)
    File "/home/ec2-user/.local/lib/python3.6/site-packages/mxnet/ndarray/ndarray.py", line 2434, in array
      arr = empty(source_array.shape, ctx, dtype)
    File "/home/ec2-user/.local/lib/python3.6/site-packages/mxnet/ndarray/ndarray.py", line 3820, in empty
      return NDArray(handle=_new_alloc_handle(shape, ctx, False, dtype))
    File "/home/ec2-user/.local/lib/python3.6/site-packages/mxnet/ndarray/ndarray.py", line 139, in _new_alloc_handle
      ctypes.byref(hdl)))
    File "/home/ec2-user/.local/lib/python3.6/site-packages/mxnet/base.py", line 252, in check_call
      raise MXNetError(py_str(_LIB.MXGetLastError()))
  mxnet.base.MXNetError: [21:51:47] src/storage/storage.cc:145: Unimplemented device 6
  
  Stack trace returned 10 entries:
  [bt] (0) /home/ec2-user/.local/lib/python3.6/site-packages/mxnet/libmxnet.so(dmlc::StackTrace()+0x44) [0x7fc9ac6a8b34]
  [bt] (1) /home/ec2-user/.local/lib/python3.6/site-packages/mxnet/libmxnet.so(dmlc::LogMessageFatal::~LogMessageFatal()+0x21) [0x7fc9ac6a8f11]
  [bt] (2) /home/ec2-user/.local/lib/python3.6/site-packages/mxnet/libmxnet.so(+0x366e697) [0x7fc9af789697]
  [bt] (3) /home/ec2-user/.local/lib/python3.6/site-packages/mxnet/libmxnet.so(+0x367031f) [0x7fc9af78b31f]
  [bt] (4) /home/ec2-user/.local/lib/python3.6/site-packages/mxnet/libmxnet.so(mxnet::StorageImpl::Alloc(mxnet::Storage::Handle*)+0x3f) [0x7fc9af78c22f]
  [bt] (5) /home/ec2-user/.local/lib/python3.6/site-packages/mxnet/libmxnet.so(+0x595cf9) [0x7fc9ac6b0cf9]
  [bt] (6) /home/ec2-user/.local/lib/python3.6/site-packages/mxnet/libmxnet.so(mxnet::NDArray::NDArray(nnvm::TShape const, mxnet::Context, bool, int)+0x7fa) [0x7fc9ac6bc76a]
  [bt] (7) /home/ec2-user/.local/lib/python3.6/site-packages/mxnet/libmxnet.so(MXNDArrayCreateEx+0x17d) [0x7fc9aef87e7d]
  [bt] (8) /usr/lib64/libffi.so.6(ffi_call_unix64+0x4c) [0x7fca559bbcec]
  [bt] (9) /usr/lib64/libffi.so.6(ffi_call+0x1f5) [0x7fca559bb615]
  ```
+ When you use either the Symbol API or the Module API, do not call the `backward()` method or call `bind()` with `for_training=True`\. This throws an error\. Because the default value of `for_training` is `True`, make sure you set `for_training=False` manually\. Sample error using test\.py:

  ```
  Traceback (most recent call last):
    File "test.py", line 16, in <module>
      label_shapes=mod._label_shapes)
    File "/home/ec2-user/.local/lib/python3.6/site-packages/mxnet/module/module.py", line 402, in bind
      raise ValueError("for training cannot be set to true with EIA context")
  ValueError: for training cannot be set to true with EIA context
  ```
+ Because training is not allowed, there is no point of initializing an optimizer for inference\.
+ `group2ctxs` is not supported for model parallelism\. If you try to create the following `Module` object, you get the error `Maximum one EI device can be attached to an instance`: `mod = mx.mod.Module(c, context=[mx.eia(0), mx.eia(1)], data_names=['a', 'b'], label_names=None, group2ctxs={'dev1': mx.eia(2), 'dev2': mx.eia(3)})` Sample error: 

  ```
  Traceback (most recent call last):
    File "test.py", line 14, in <module>
      mod = mx.mod.Module(symbol=sym, context=ctx, label_names=None, group2ctxs={'dev1': mx.eia(2), 'dev2':mx.eia(3)})
    File "/home/ec2-user/.local/lib/python3.6/site-packages/mxnet/module/module.py", line 82, in __init__
      raise ValueError("Maximum one EIA device can be attached to an instance")
  ValueError: Maximum one EIA device can be attached to an instance
  ```
+ Multiple contexts are not supported\. That is, you can't use data parallelism\. Only one EI device can be attached to an instance\. Sample error:

  ```
  Traceback (most recent call last):
    File "test.py", line 14, in <module>
      mod = mx.mod.Module(symbol=sym, context=ctx, label_names=None)
    File "/home/ec2-user/.local/lib/python3.6/site-packages/mxnet/module/module.py", line 82, in __init__
      raise ValueError("Maximum one EIA device can be attached to an instance")
  ValueError: Maximum one EIA device can be attached to an instance
  ```
+ A model trained on `MXNet version <= MXNet_EIA version` works but, a model trained on `MXNet version >= MXNet_EIA` version results in undefined behavior\.
+ Different sizes of EI accelerators have different amounts of GPU memory\. If your model requires more GPU memory than is available in your accelerator, you get a message that looks like the log below\. If you run into this message, you should use a larger accelerator size\. Stop and restart your instance\.

  ```
  [Tue Nov 27 00:13:27 2018, 717139us] [Execution Engine][MXNet][3] Failed - Last Error:
  EI Error Code: [12, 5, 13]
  EI Error Description: Internal error
  EI Request ID: MX-A830D1C2-D6F7-4264-8B9C-7D6402E26C06 -- EI Accelerator ID: eia-2e1e793895b040f989d2309c4b9f1e66
  EI Client Version: 1.2.8
  Traceback (most recent call last):
  File "test.py", line 30, in module
  prob = mod.get_outputs()[0].asnumpy()
  File "/home/ec2-user/anaconda3/envs/amazonei_mxnet_p36/lib/python3.6/site-packages/mxnet/ndarray/ndarray.py", line 1972, in asnumpy
  ctypes.c_size_t(data.size)))
  File "/home/ec2-user/anaconda3/envs/amazonei_mxnet_p36/lib/python3.6/site-packages/mxnet/base.py", line 252, in check_call
  raise MXNetError(py_str(_LIB.MXGetLastError()))
  mxnet.base.MXNetError: [00:13:27] src/operator/subgraph/eia/eia_subgraph_op.cc:147: Last Error:
  EI Error Code: [12, 5, 13]
  EI Error Description: Internal error
  EI Request ID: MX-A830D1C2-D6F7-4264-8B9C-7D6402E26C06 -- EI Accelerator ID: eia-2e1e793895b040f989d2309c4b9f1e66
  EI Client Version: 1.2.8
  ```
+ Calling `reshape` explicitly by using either the Module API or the Symbol API, or implicitly using different shapes for input `ndarrays` in different forward passes can lead to OOM errors\. Before being reshaped, the model is not cleaned up on the accelerator until the session is destroyed\.

### More Models and Resources<a name="tutorial-mxnet-elastic-inference-more"></a>

Here are some more pre\-trained models and examples to try with EI\. 

1. [MXNet ImageNet Models](http://data.mxnet.io/models/imagenet/) \- Many of the ImageNet\-based models found here can be dropped into the previous examples by updating the `path` variable on line 6\.

1. [MXNet Model Zoo](https://mxnet.incubator.apache.org/api/python/gluon/model_zoo.html) \- these Gluon models can be exported to the Symbol format and used with EI\.

1. [Open Neural Network Exchange \(ONNX\) Models with MXNet](https://mxnet.incubator.apache.org/api/python/contrib/onnx.html) \- MXNet natively supports the ONNX model format, so you can use EI with ONNX models that were exported from other frameworks\.

For more tutorials and examples, see the framework's official Python documentation, the [Python API for MXNet](https://mxnet.incubator.apache.org/api/python/index.html), or the [Apache MXNet](https://mxnet.incubator.apache.org/) website\.