# Use Elastic Inference with TensorFlow<a name="tutorial-tf-elastic-inference"></a>

[Elastic Inference](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/elastic-inference.html) \(EI\) is available only on instances the were launched with an Elastic Inference Accelerator\.

Review [Selecting the Instance Type for DLAMI](instance-select.md) to select your desired instance type, and also review [Elastic Inference Prerequisites](ei-prerequisites.md) for the instructions related to Elastic Inference\. For detailed instructions on how to launch a DLAMI with an Elastic Inference Accelerator, see the [Elastic Inference](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/elastic-inference.html) documentation\.

TensorFlow Serving is the only inference mode that EI supports\. Predictor and Estimator are not supported\. If you haven't tried TensorFlow Serving before, we recommend that you try the [TensorFlow Serving](tutorial-tfserving.md) tutorial first\. 

## Activate the TensorFlow Elastic Inference Environment<a name="tutorial-tf-elastic-inference-activate"></a>

1. First, connect to your Deep Learning AMI with Conda and activate the Python 2\.7 TensorFlow environment\. The example scripts are not compatible with Python 3\.x\.

   ```
   $ source activate amazonei_tensorflow_p27
   ```

1. The remaining parts of this guide assume you are using the `amazonei_tensorflow_p27` environment\.

**Note**  
If you are switching between MXNet or TensorFlow Elastic Inference environments, you must Stop and then Start your instance to reattach the Elastic Inference Accelerator\. Rebooting is not sufficient since the process requires a complete shutdown\.

## Using Elastic Inference with TensorFlow<a name="ei-tf"></a>

The EI\-enabled version of TensorFlow lets you use EI seamlessly with few changes to your TensorFlow code\. You can use EI with TensorFlow in one of the following ways:
+ Serve models with EI using TensorFlow Serving\. For information about TensorFlow Serving, see [https://www\.tensorflow\.org/serving/](https://www.tensorflow.org/serving/)\.

### Using EI TensorFlow Serving<a name="ei-tf-serving"></a>

Elastic Inference TensorFlow Serving uses the same API as normal TensorFlow Serving\. The only difference is that the entry point is a different binary named `AmazonEI_TensorFlow_Serving_v1.11_v1`\. This file is found at `/usr/local/bin/AmazonEI_TensorFlow_Serving_v1.11_v1`\. The following example shows commands to use TensorFlow serving\.

## EI TensorFlow Serving Examples<a name="ei-tf-serving-examples"></a>

The following is an example you can try for serving different models like Inception\. As a general rule, you need a servable model and client scripts to be already downloaded to your DLAMI\.

**Serve and Test Inference with an Inception Model**

1. Download the model to your home directory\.

   ```
   $ curl -O https://s3-us-west-2.amazonaws.com/aws-tf-serving-ei-example/inception.zip
   ```

1. Untar the model\.

   ```
   $ unzip inception.zip
   ```

1. Download a picture of a husky to your home directory\.

   ```
   $ curl -O https://upload.wikimedia.org/wikipedia/commons/b/b5/Siberian_Husky_bi-eyed_Flickr.jpg
   ```

1. Launch the server\. Note, that for Amazon Linux, you must change the directory used for `model_base_path`, from `/home/ubuntu` to `/home/ec2-user`\.

   ```
   $ AmazonEI_TensorFlow_Serving_v1.11_v1 --model_name=inception --model_base_path=/home/ubuntu/SERVING_INCEPTION/SERVING_INCEPTION --port=9000
   ```

1. With the server running in the foreground you will need to launch another terminal session to continue\. Open a new terminal and activate TensorFlow with `source activate amazonei_tensorflow_p27`\. Then use your preferred text editor to create a script that has the following content\. Name it `inception_client.py`\. This script will take an image filename as a parameter, and get a prediction result from the pre\-trained model\.

   ```
   from __future__ import print_function
   
   import grpc
   import tensorflow as tf
   
   from tensorflow_serving.apis import predict_pb2
   from tensorflow_serving.apis import prediction_service_pb2_grpc
   
   
   tf.app.flags.DEFINE_string('server', 'localhost:9000',
                              'PredictionService host:port')
   tf.app.flags.DEFINE_string('image', '', 'path to image in JPEG format')
   FLAGS = tf.app.flags.FLAGS
   
   
   def main(_):
     channel = grpc.insecure_channel(FLAGS.server)
     stub = prediction_service_pb2_grpc.PredictionServiceStub(channel)
     # Send request
     with open(FLAGS.image, 'rb') as f:
       # See prediction_service.proto for gRPC request/response details.
       data = f.read()
       request = predict_pb2.PredictRequest()
       request.model_spec.name = 'inception'
       request.model_spec.signature_name = 'predict_images'
       request.inputs['images'].CopyFrom(
           tf.contrib.util.make_tensor_proto(data, shape=[1]))
       result = stub.Predict(request, 10.0)  # 10 secs timeout
       print(result)
     print("Inception Client Passed")
   
   if __name__ == '__main__':
     tf.app.run()
   ```

1. Now run the script passing the server location and port and the husky photo's filename as the parameters\.

   ```
   $ python inception_client.py --server=localhost:9000 --image Siberian_Husky_bi-eyed_Flickr.jpg
   ```

## Considerations When Using EI\-enabled TensorFlow<a name="ei-tf-considerations"></a>
+ Warmup: Tensorflow Serving provides a warmup feature to pre\-load models and reduce the delay that is typical of the first inference request\. Amazon EI TensorFlow Serving only supports warming up the “serving\_default” signature definition\.
+ Signature Definitions: Using multiple signature definitions can have a multiplicative effect on the amount of accelerator memory consumed\. If you plan to exercise more than one signature definition for your inference calls, you should test these scenarios as you determine the accelerator type for your application\.

## More Models and Resources<a name="tutorial-tf-elastic-inference-more"></a>

1. [TensorFlow Serving](tutorial-tfserving.md) \- TensorFlow Serving has inference examples that can be used with EI\.

For more tutorials and examples, see the framework's official Python documentation, the [TensorFlow Python API](https://www.tensorflow.org/api_docs/python/)\.