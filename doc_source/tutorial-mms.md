# Model Server for Apache MXNet \(MMS\)<a name="tutorial-mms"></a>

[Model Server for Apache MXNet \(MMS\)](https://github.com/awslabs/mxnet-model-server/)is a flexible tool for serving deep learning models that have been exported from [Apache MXNet \(incubating\)](http://mxnet.io/)\. MMS comes preinstalled with the DLAMI with Conda\. This tutorial for MMS will demonstrate how to serve an image classification model, and guide you to finding a Single Shot Detector \(SSD\) example that is included on the DLAMI with Conda\.

**Topics**
+ [Serve an Image Classification Model on MMS](#tutorial-mms-serve-mxnet-model)
+ [Multi\-class Detection Example](#tutorial-mms-ssd-example)
+ [More Info](#tutorial-mms-project)

## Serve an Image Classification Model on MMS<a name="tutorial-mms-serve-mxnet-model"></a>

This tutorial shows how to serve an image classification model with MMS\. The model is provided via the [MMS Model Zoo](https://github.com/awslabs/mxnet-model-server/blob/master/docs/model_zoo.md), and is automatically downloaded when you start MMS\. Once the server is running, it listens for prediction requests\. When you upload an image, in this case, an image of a kitten, the server returns a prediction of the top 5 matching classes out of the 1,000 classes that the model was trained on\. More information on the models, how they were trained, and how to test them can be found in the [MMS Model Zoo](https://github.com/awslabs/mxnet-model-server/blob/master/docs/model_zoo.md)\.

**To serve an example image classification model on MMS**

1. Connect to an Amazon Elastic Compute Cloud \(Amazon EC2\) instance of the Deep Learning AMI with Conda\. 

1. Activate an MXNet environment:

   ```
   $ source activate mxnet_p36
   ```

1. Run MMS with the following command\. This command also downloads the model and serves it\.

   ```
   $ mxnet-model-server \
   --models squeezenet=https://s3.amazonaws.com/model-server/models/squeezenet_v1.1/squeezenet_v1.1.model
   ```

   MMS is now running on your host, and is listening for inference requests\. 

1. To test MMS, in a new terminal window, connect to the instance that is running the DLAMI\. 

1. Download an image of a kitten and send it to the MMS predict endpoint: 

   ```
   $ curl -O https://s3.amazonaws.com/model-server/inputs/kitten.jpg
   $ curl -X POST http://127.0.0.1:8080/squeezenet/predict -F "data=@kitten.jpg"
   ```

   The predict endpoint returns a prediction in JSON similar to the following top five predictions, where the image has a 94% probability of containing an Egyptian cat, followed by a 5\.5% chance it has a lynx or catamount: 

   ```
   {
   	"prediction": [
   		[{
   				"class": "n02124075 Egyptian cat",
   				"probability": 0.940
   			},
   			{
   				"class": "n02127052 lynx, catamount",
   				"probability": 0.055
   			},
   			{
   				"class": "n02123045 tabby, tabby cat",
   				"probability": 0.002
   			},
   			{
   				"class": "n02123159 tiger cat",
   				"probability": 0.0003
   			},
   			{
   				"class": "n02123394 Persian cat",
   				"probability": 0.0002
   			}
   		]
   	]
   }
   ```

This tutorial focuses on basic model serving\. When you're ready to learn more about other MMS features, see the MMS documentation on GitHub\.

## Multi\-class Detection Example<a name="tutorial-mms-ssd-example"></a>

The DLAMI with Conda includes an example application that uses MMS to serve a Single Shot MultiBox Detector \(SSD\) model\. To see the example, open the DLAMI in a terminal, and navigate to the `~/tutorials/MXNet-Model-Server/ssd` folder\. For instructions on running the example, see the `README.md` file or the [latest version of the example](https://github.com/awslabs/mxnet-model-server/blob/master/examples/ssd/README.md) in the MMS GitHub repository\.

## More Info<a name="tutorial-mms-project"></a>

For more MMS examples—such as examples of exporting models and setting up MMS with Docker—or to take advantage of the latest MMS features, star [the MMS project page](https://github.com/awslabs/mxnet-model-server) on GitHub\.