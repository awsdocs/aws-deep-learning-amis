# Chainer to ONNX to CNTK Tutorial<a name="tutorial-onnx-chainer-cntk"></a>

## ONNX Overview<a name="tutorial-onnx-overview"></a>

The Open Neural Network Exchange \([ONNX](http://onnx.ai/)\) is an open format used to represent deep learning models\. ONNX is supported by Amazon Web Services, Microsoft, Facebook, and several other partners\. You can design, train, and deploy deep learning models with any framework you choose\. The benefit of ONNX models is that they can be moved between frameworks with ease\.

This tutorial shows you how to use the Deep Learning AMI with Conda with ONNX\. By following these steps, you can train a model or load a pre\-trained model from one framework, export this model to ONNX, and then import the model in another framework\.

## ONNX Prerequisites<a name="tutorial-onnx-prereq"></a>

To use this ONNX tutorial, you must have access to a Deep Learning AMI with Conda version 12 or later\. For more information about how to get started with a Deep Learning AMI with Conda, see [Deep Learning AMI with Conda](overview-conda.md)\.

Launch a terminal session with your Deep Learning AMI with Conda to begin the following tutorial\.

## Convert a Chainer Model to ONNX, then Load the Model into CNTK<a name="tutorial-onnx-chainer-cntk-detail"></a>

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

After you run this script, you will see the newly created \.onnx file in the same directory\. Now, switch to the CNTK Conda environment to load the model with CNTK\.

Next, activate the CNTK environment:

```
$ source deactivate
$ source activate cntk_p36
```

Create a new file with your text editor, and use the following program in a script to open ONNX format file in CNTK\.

```
import cntk as C
# Import the Chainer model into CNTK via CNTK's import API
z = C.Function.load("vgg16.onnx", device=C.device.cpu(), format=C.ModelFormat.ONNX)
print("Loaded vgg16.onnx!")
```

After you run this script, CNTK will have loaded the model\.

You may also try running inference with CNTK\. First, download a picture of a husky\.

```
$ wget https://upload.wikimedia.org/wikipedia/commons/b/b5/Siberian_Husky_bi-eyed_Flickr.jpg
```

Next, download a list of classes will work with this model\.

```
$ wget https://gist.githubusercontent.com/yrevar/6135f1bd8dcf2e0cc683/raw/d133d61a09d7e5a3b36b8c111a8dd5c4b5d560ee/imagenet1000_clsid_to_human.pkl
```

Edit the previously created script to have the following content\. This new version will use the image of the husky, get a prediction result, then look this up in the file of classes, returning a prediction result\.

```
import cntk as C
import numpy as np
from PIL import Image
from IPython.core.display import display
import pickle

# Import the Chainer model into CNTK via CNTK's import API
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

Before you run the script, you need one more Python package\. Install Pillow with pip:

```
$ pip install pillow
```

Then run the script, and you should see a result as follows:

```
248
Eskimo dog, husky
```

## ONNX Tutorials<a name="tutorial-onnx-footer"></a>
+ [Chainer to ONNX to CNTK Tutorial](#tutorial-onnx-chainer-cntk)
+ [Chainer to ONNX to MXNet Tutorial](tutorial-onnx-chainer-mxnet.md)
+ [PyTorch to ONNX to MXNet Tutorial](tutorial-onnx-pytorch-mxnet.md)
+ [PyTorch to ONNX to CNTK Tutorial](tutorial-onnx-pytorch-cntk.md)