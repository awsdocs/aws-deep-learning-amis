# Model Server for Apache MXNet \(MMS\)<a name="tutorial-mms"></a>

[Model Server for Apache MXNet \(MMS\)](https://github.com/awslabs/mxnet-model-server/) is a flexible tool for serving deep learning models that have been exported from [Apache MXNet \(incubating\)](http://mxnet.io/) or exported to an Open Neural Network Exchange \(ONNX\) model format\. MMS comes preinstalled with the DLAMI with Conda\. This tutorial for MMS will demonstrate how to serve an image classification model\.

**Topics**
+ [Serve an Image Classification Model on MMS](#tutorial-mms-serve-mxnet-model)
+ [Other Examples](#tutorial-mms-examples)
+ [More Info](#tutorial-mms-more)

## Serve an Image Classification Model on MMS<a name="tutorial-mms-serve-mxnet-model"></a>

This tutorial shows how to serve an image classification model with MMS\. The model is provided via the [MMS Model Zoo](https://github.com/awslabs/mxnet-model-server/blob/master/docs/model_zoo.md), and is automatically downloaded when you start MMS\. Once the server is running, it listens for prediction requests\. When you upload an image, in this case, an image of a kitten, the server returns a prediction of the top 5 matching classes out of the 1,000 classes that the model was trained on\. More information on the models, how they were trained, and how to test them can be found in the [MMS Model Zoo](https://github.com/awslabs/mxnet-model-server/blob/master/docs/model_zoo.md)\.

**To serve an example image classification model on MMS**

1. Connect to an Amazon Elastic Compute Cloud \(Amazon EC2\) instance of the Deep Learning AMI with Conda\. 

1. Activate an MXNet environment:
   + For MXNet and Keras 2 on Python 3 with CUDA 9\.0 and MKL\-DNN, run this command:

     ```
     $ source activate mxnet_p36
     ```
   + For MXNet and Keras 2 on Python 2 with CUDA 9\.0 and MKL\-DNN, run this command:

     ```
     $ source activate mxnet_p27
     ```

1. Run MMS with the following command\. Adding ` > /dev/null` will quiet the log output while you run further tests\. 

   ```
   $ mxnet-model-server --start > /dev/null
   ```

   MMS is now running on your host, and is listening for inference requests\. 

1. Next, use a curl command to administer MMS's management endpoints, and tell it what model you want it to serve\.

   ```
   $ curl -X POST "http://localhost:8081/models?url=https%3A%2F%2Fs3.amazonaws.com%2Fmodel-server%2Fmodels%2Fsqueezenet_v1.1%2Fsqueezenet_v1.1.model"
   ```

1. MMS needs to know the number of workers you would like to use\. For this test you can try 3\.

   ```
   $ curl -v -X PUT "http://localhost:8081/models/squeezenet_v1.1?min_worker=3"
   ```

1. Download an image of a kitten and send it to the MMS predict endpoint: 

   ```
   $ curl -O https://s3.amazonaws.com/model-server/inputs/kitten.jpg
   $ curl -X POST http://127.0.0.1:8080/predictions/squeezenet_v1.1 -T kitten.jpg
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

1. Test out some more images, or if you have finished testing, stop the server:

   ```
   $ mxnet-model-server --stop
   ```

This tutorial focuses on basic model serving\. MMS also supports using Elastic Inference with model serving\. For more information, see [ Model Serving with Amazon Elastic Inference](https://github.com/awslabs/mxnet-model-server/blob/master/docs/elastic_inference.md)

When you're ready to learn more about other MMS features, see the [MMS documentation on GitHub](https://github.com/awslabs/mxnet-model-server/blob/master/docs/README.md)\.

## Other Examples<a name="tutorial-mms-examples"></a>

MMS has a variety of examples that you can run on your DLAMI\. You can view them on the [MMS project repository](https://github.com/awslabs/mxnet-model-server/tree/master/examples)\.

## More Info<a name="tutorial-mms-more"></a>

For more MMS documentation, including how to set up MMS with Docker—or to take advantage of the latest MMS features, star [the MMS project page](https://github.com/awslabs/mxnet-model-server) on GitHub\.