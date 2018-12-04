# Use Apache MXNet for Inference with an ONNX Model<a name="tutorial-mxnet-inference-onnx"></a>

**How to Use an ONNX Model for Image Inference with MXNet**

1. 
   + \(Option for Python 3\) \- Activate the Python 3 MXNet environment:

     ```
     $ source activate mxnet_p36
     ```
   + \(Option for Python 2\) \- Activate the Python 2 MXNet environment:

     ```
     $ source activate mxnet_p27
     ```

1. The remaining steps assume you are using the `mxnet_p36` environment\.

1. Download a picture of a husky\.

   ```
   $ curl -O https://upload.wikimedia.org/wikipedia/commons/b/b5/Siberian_Husky_bi-eyed_Flickr.jpg
   ```

1. Download a list of classes will work with this model\.

   ```
   $ curl -O https://gist.githubusercontent.com/yrevar/6135f1bd8dcf2e0cc683/raw/d133d61a09d7e5a3b36b8c111a8dd5c4b5d560ee/imagenet1000_clsid_to_human.pkl
   ```

1. Use a your preferred text editor to create a script that has the following content\. This script will use the image of the husky, get a prediction result from the pre\-trained model, then look this up in the file of classes, returning a prediction result\.

   ```
   import mxnet as mx
   import numpy as np
   from collections import namedtuple
   from PIL import Image
   import pickle
   
   # Preprocess the image
   img = Image.open("Siberian_Husky_bi-eyed_Flickr.jpg")
   img = img.resize((224,224))
   rgb_img = np.asarray(img, dtype=np.float32) - 128
   bgr_img = rgb_img[..., [2,1,0]]
   img_data = np.ascontiguousarray(np.rollaxis(bgr_img,2))
   img_data = img_data[np.newaxis, :, :, :].astype(np.float32)
   
   # Define the model's input
   data_names = ['data']
   Batch = namedtuple('Batch', data_names)
   
   # Set the context to cpu or gpu
   ctx = mx.cpu()
   
   # Load the model
   sym, arg, aux = onnx_mxnet.import_model("vgg16.onnx")
   mod = mx.mod.Module(symbol=sym, data_names=data_names, context=ctx, label_names=None)
   mod.bind(for_training=False, data_shapes=[(data_names[0],img_data.shape)], label_shapes=None)
   mod.set_params(arg_params=arg, aux_params=aux, allow_missing=True, allow_extra=True)
   
   # Run inference on the image
   mod.forward(Batch([mx.nd.array(img_data)]))
   predictions = mod.get_outputs()[0].asnumpy()
   top_class = np.argmax(predictions)
   print(top_class)
   labels_dict = pickle.load(open("imagenet1000_clsid_to_human.pkl", "rb"))
   print(labels_dict[top_class])
   ```

1. Then run the script, and you should see a result as follows:

   ```
   248
   Eskimo dog, husky
   ```