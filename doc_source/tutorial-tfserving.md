# TensorFlow Serving<a name="tutorial-tfserving"></a>

[TensorFlow Serving](https://www.tensorflow.org/tfx/guide/serving) is a flexible, high\-performance serving system for machine learning models\.

The `tensorflow-serving-api` is pre\-installed with Deep Learning AMI with Conda\! You will find an example scripts to train, export, and serve an MNIST model in `~/examples/tensorflow-serving/`\.

To run any of these examples, first connect to your Deep Learning AMI with Conda and activate the TensorFlow environment\.

```
$ source activate tensorflow_p37
```

Now change directories to the serving example scripts folder\.

```
$ cd ~/examples/tensorflow-serving/
```

## Serve a Pretrained Inception Model<a name="tf-serving-inception"></a>

The following is an example you can try for serving different models like Inception\. As a general rule, you need a servable model and client scripts to be already downloaded to your DLAMI\.

**Serve and Test Inference with an Inception Model**

1. Download the model\.

   ```
   $ curl -O https://s3-us-west-2.amazonaws.com/tf-test-models/INCEPTION.zip
   ```

1. Untar the model\.

   ```
   $ unzip INCEPTION.zip
   ```

1. Download a picture of a husky\.

   ```
   $ curl -O https://upload.wikimedia.org/wikipedia/commons/b/b5/Siberian_Husky_bi-eyed_Flickr.jpg
   ```

1. Launch the server\. Note, that for Amazon Linux, you must change the directory used for `model_base_path`, from `/home/ubuntu` to `/home/ec2-user`\.

   ```
   $ tensorflow_model_server --model_name=INCEPTION --model_base_path=/home/ubuntu/examples/tensorflow-serving/INCEPTION/INCEPTION --port=9000
   ```

1. With the server running in the foreground, you need to launch another terminal session to continue\. Open a new terminal and activate TensorFlow with `source activate tensorflow_p37`\. Then use your preferred text editor to create a script that has the following content\. Name it `inception_client.py`\. This script will take an image filename as a parameter, and get a prediction result from the pre\-trained model\.

   ```
   from __future__ import print_function
   
   import grpc
   import tensorflow as tf
   import argparse
   
   from tensorflow_serving.apis import predict_pb2
   from tensorflow_serving.apis import prediction_service_pb2_grpc
   
   parser = argparse.ArgumentParser(
       description='TF Serving Test',
       formatter_class=argparse.ArgumentDefaultsHelpFormatter
   )
   parser.add_argument('--server_address', default='localhost:9000',
                       help='Tenforflow Model Server Address')
   parser.add_argument('--image', default='Siberian_Husky_bi-eyed_Flickr.jpg',
                       help='Path to the image')
   args = parser.parse_args()
   
   
   def main():
     channel = grpc.insecure_channel(args.server_address)
     stub = prediction_service_pb2_grpc.PredictionServiceStub(channel)
     # Send request
     with open(args.image, 'rb') as f:
       # See prediction_service.proto for gRPC request/response details.
       request = predict_pb2.PredictRequest()
       request.model_spec.name = 'INCEPTION'
       request.model_spec.signature_name = 'predict_images'
   
       input_name = 'images'
       input_shape = [1]
       input_data = f.read()
       request.inputs[input_name].CopyFrom(
         tf.make_tensor_proto(input_data, shape=input_shape))
   
       result = stub.Predict(request, 10.0)  # 10 secs timeout
       print(result)
   
     print("Inception Client Passed")
   
   
   if __name__ == '__main__':
     main()
   ```

1. Now run the script passing the server location and port and the husky photo's filename as the parameters\.

   ```
   $ python3 inception_client.py --server=localhost:9000 --image Siberian_Husky_bi-eyed_Flickr.jpg
   ```

## Train and Serve an MNIST Model<a name="tutorial-tfserving-mnist"></a>

For this tutorial we will export a model then serve it with the `tensorflow_model_server` application\. Finally, you can test the model server with an example client script\.

Run the script that will train and export an MNIST model\. As the script's only argument, you need to provide a folder location for it to save the model\. For now we can just put it in `mnist_model`\. The script will create the folder for you\.

```
$ python mnist_saved_model.py /tmp/mnist_model
```

 Be patient, as this script may take a while before providing any output\. When the training is complete and the model is finally exported you should see the following: 

```
Done training!
Exporting trained model to mnist_model/1
Done exporting!
```

Your next step is to run `tensorflow_model_server` to serve the exported model\. 

```
$ tensorflow_model_server --port=9000 --model_name=mnist --model_base_path=/tmp/mnist_model
```

A client script is provided for you to test the server\.

**To test it out, you will need to open a new terminal window\.**

```
$ python mnist_client.py --num_tests=1000 --server=localhost:9000
```

## More Features and Examples<a name="tutorial-tfserving-project"></a>

If you are interested in learning more about TensorFlow Serving, check out the [TensorFlow website](https://www.tensorflow.org/serving/)\.

You can also use TensorFlow Serving with [Amazon Elastic Inference](https://docs.aws.amazon.com/elastic-inference/latest/developerguide/what-is-ei.html)\. Check out the guide on how to [Use Elastic Inference with TensorFlow Serving](https://docs.aws.amazon.com/elastic-inference/latest/developerguide/ei-tensorflow-python.html) for more info\.