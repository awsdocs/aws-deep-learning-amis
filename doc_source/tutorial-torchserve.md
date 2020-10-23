# TorchServe<a name="tutorial-torchserve"></a>

TorchServe is a flexible tool for serving deep learning models that have been exported from PyTorch\. TorchServe comes preinstalled with the Deep Learning AMI with Conda starting with v34\. 

For more information on using TorchServe, see [Model Server for PyTorch Documentation](https://github.com/pytorch/serve/blob/master/docs/README.md)\. 

 **Topics** 

## Serve an Image Classification Model on TorchServe<a name="tutorial-torchserve-serving"></a>

This tutorial shows how to serve an image classification model with TorchServe\. It uses a DenseNet\-161 model provided by PyTorch\. Once the server is running, it listens for prediction requests\. When you upload an image, in this case, an image of a kitten, the server returns a prediction of the top 5 matching classes out of the classes that the model was trained on\. 

**To serve an example image classification model on TorchServe**

1. Connect to an Amazon Elastic Compute Cloud \(Amazon EC2\) instance with Deep Learning AMI with Conda v34 or later\. 

1. Activate the `pytorch_latest_p36` environment\. 

   ```
   source activate pytorch_latest_p36
   ```

1. Clone the TorchServe repository, then create a directory to store your models\.  

   ```
   git clone https://github.com/pytorch/serve.git
   mkdir model_store
   ```

1. Archive the model using the model archiver\. The `extra-files` param uses a file from the `TorchServe` repo, so update the path if necessary\. For more information about the model archiver, see [Torch Model archiver for TorchServe\.](https://github.com/pytorch/serve/blob/master/model-archiver/README.md) 

   ```
   wget https://download.pytorch.org/models/densenet161-8d451a50.pth
   torch-model-archiver --model-name densenet161 --version 1.0 --model-file ./serve/examples/image_classifier/densenet_161/model.py --serialized-file densenet161-8d451a50.pth --export-path model_store --extra-files ./serve/examples/image_classifier/index_to_name.json --handler image_classifier
   ```

1. Run TorchServe to start an endpoint\. Adding `> /dev/null` quiets the log output\. 

   ```
   torchserve --start --ncs --model-store model_store --models densenet161.mar > /dev/null
   ```

1. Download an image of a kitten and send it to the TorchServe predict endpoint: 

   ```
   curl -O https://s3.amazonaws.com/model-server/inputs/kitten.jpg
   curl http://127.0.0.1:8080/predictions/densenet161 -T kitten.jpg
   ```

   The predict endpoint returns a prediction in JSON similar to the following top five predictions, where the image has a 47% probability of containing an Egyptian cat, followed by a 46% chance it has a tabby cat\. 

   ```
   {
    "tiger_cat": 0.46933576464653015,
    "tabby": 0.463387668132782,
    "Egyptian_cat": 0.0645613968372345,
    "lynx": 0.0012828196631744504,
    "plastic_bag": 0.00023323058849200606
   }
   ```

1. When you finish testing, stop the server: 

   ```
   torchserve --stop
   ```

 **Other Examples** 

TorchServe has a variety of examples that you can run on your DLAMI instance\. You can view them on [the TorchServe project repository examples page](https://github.com/pytorch/serve/tree/master/examples)\. 

 **More Info** 

 For more TorchServe documentation, including how to set up TorchServe with Docker and the latest TorchServe features, see [the TorchServe project page](https://github.com/pytorch/serve)on GitHub\. 