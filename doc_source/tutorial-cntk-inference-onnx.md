# Use CNTK for Inference with an ONNX Model<a name="tutorial-cntk-inference-onnx"></a>

**How to Use an ONNX Model for Inference with CNTK**

1. 
   + \(Option for Python 3\) \- Activate the Python 3 CNTK environment:

     ```
     $ source activate cntk_p36
     ```
   + \(Option for Python 2\) \- Activate the Python 2 CNTK environment:

     ```
     $ source activate cntk_p27
     ```

1. The remaining steps assume you are using the `cntk_p36` environment\.

1. Create a new file with your text editor, and use the following program in a script to open ONNX format file in CNTK\.

   ```
   import cntk as C
   # Import the Chainer model into CNTK via CNTK's import API
   z = C.Function.load("vgg16.onnx", device=C.device.cpu(), format=C.ModelFormat.ONNX)
   print("Loaded vgg16.onnx!")
   ```

   After you run this script, CNTK will have loaded the model\.

1. You may also try running inference with CNTK\. First, download a picture of a husky\.

   ```
   $ curl -O https://upload.wikimedia.org/wikipedia/commons/b/b5/Siberian_Husky_bi-eyed_Flickr.jpg
   ```

1. Next, download a list of classes will work with this model\.

   ```
   $ curl -O https://gist.githubusercontent.com/yrevar/6135f1bd8dcf2e0cc683/raw/d133d61a09d7e5a3b36b8c111a8dd5c4b5d560ee/imagenet1000_clsid_to_human.pkl
   ```

1. Edit the previously created script to have the following content\. This new version will use the image of the husky, get a prediction result, then look this up in the file of classes, returning a prediction result\.

   ```
   import cntk as C
   import numpy as np
   from PIL import Image
   from IPython.core.display import display
   import pickle
   
   # Import the model into CNTK via CNTK's import API
   z = C.Function.load("vgg16.onnx", device=C.device.cpu(), format=C.ModelFormat.ONNX)
   print("Loaded vgg16.onnx!")
   img = Image.open("Siberian_Husky_bi-eyed_Flickr.jpg")
   img = img.resize((224,224))
   rgb_img = np.asarray(img, dtype=np.float32) - 128
   bgr_img = rgb_img[..., [2,1,0]]
   img_data = np.ascontiguousarray(np.rollaxis(bgr_img,2))
   predictions = np.squeeze(z.eval({z.arguments[0]:[img_data]}))
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