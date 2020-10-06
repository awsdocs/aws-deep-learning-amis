# Use Apache MXNet \(Incubating\) for Inference with a ResNet 50 Model<a name="tutorial-mxnet-inference-resnet50"></a>

**How to Use a Pre\-Trained Apache MXNet \(Incubating\) Model with the Symbol API for Image Inference with MXNet**

1. 
   + \(Option for Python 3\) \- Activate the Python 3 Apache MXNet \(Incubating\) environment:

     ```
     $ source activate mxnet_p36
     ```
   + \(Option for Python 2\) \- Activate the Python 2 Apache MXNet \(Incubating\) environment:

     ```
     $ source activate mxnet_p27
     ```

1. The remaining steps assume you are using the `mxnet_p36` environment\.

1. Use a your preferred text editor to create a script that has the following content\. This script will download the ResNet\-50 model files \(resnet\-50\-0000\.params and resnet\-50\-symbol\.json\) and labels list \(synset\.txt\), download a cat image to get a prediction result from the pre\-trained model, then look this up in the result in labels list, returning a prediction result\.

   ```
   import mxnet as mx
   import numpy as np
   
   path='http://data.mxnet.io/models/imagenet/'
   [mx.test_utils.download(path+'resnet/50-layers/resnet-50-0000.params'),
    mx.test_utils.download(path+'resnet/50-layers/resnet-50-symbol.json'),
    mx.test_utils.download(path+'synset.txt')]
   
   ctx = mx.cpu()
   
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
   
   exe.forward()
   prob = exe.outputs[0].asnumpy()
   # print the top-5
   prob = np.squeeze(prob)
   a = np.argsort(prob)[::-1]
   for i in a[0:5]:
       print('probability=%f, class=%s' %(prob[i], labels[i]))
   ```

1. Then run the script, and you should see a result as follows:

   ```
   probability=0.418679, class=n02119789 kit fox, Vulpes macrotis
   probability=0.293495, class=n02119022 red fox, Vulpes vulpes
   probability=0.029321, class=n02120505 grey fox, gray fox, Urocyon cinereoargenteus
   probability=0.026230, class=n02124075 Egyptian cat
   probability=0.022557, class=n02085620 Chihuahua
   ```