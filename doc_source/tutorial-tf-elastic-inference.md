# Use Elastic Inference with TensorFlow<a name="tutorial-tf-elastic-inference"></a>

## About Elastic Inference<a name="tutorial-tf-elastic-inference-overview"></a>

[Elastic Inference](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/elastic-inference.html) \(EI\) is available only on instances that were launched with an Elastic Inference Accelerator\.

Review [Selecting the Instance Type for DLAMI](instance-select.md) to select your desired instance type, and also review [Elastic Inference Prerequisites](ei-prerequisites.md) for the instructions related to Elastic Inference\. For detailed instructions on how to launch a DLAMI with an Elastic Inference Accelerator, see the [Elastic Inference](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/elastic-inference.html) documentation\.

TensorFlow Serving and Predictor are the only inference modes that EI supports\. If you haven't tried TensorFlow Serving before, we recommend that you try the [TensorFlow Serving](tutorial-tfserving.md) tutorial first\. 

### Activate the TensorFlow Elastic Inference Environment<a name="tutorial-tf-elastic-inference-activate"></a>

1. 
   + \(Option for Python 3\) \- Activate the Python 3 TensorFlow EI environment:

     ```
     $ source activate amazonei_tensorflow_p36
     ```
   + \(Option for Python 2\) \- Activate the Python 2\.7 TensorFlow EI environment:

     ```
     $ source activate amazonei_tensorflow_p27
     ```

1. The remaining parts of this guide assume you are using the `amazonei_tensorflow_p27` environment\.

**Note**  
If you are switching between MXNet or TensorFlow Elastic Inference environments, you must Stop and then Start your instance to reattach the Elastic Inference Accelerator\. Rebooting is not sufficient since the process requires a complete shutdown\.

### Using Elastic Inference with TensorFlow<a name="ei-tf"></a>

The EI\-enabled version of TensorFlow lets you use EI seamlessly with few changes to your TensorFlow code\. You can use EI with TensorFlow in one of the following ways:
+ To load a saved model or frozen graph for predictions you can use the EIPredictor class\.
+ Serve models with EI using TensorFlow Serving\. For information about TensorFlow Serving, see [https://www\.tensorflow\.org/serving/](https://www.tensorflow.org/serving/)\.

#### Using EI TensorFlow Serving<a name="ei-tf-serving"></a>

Elastic Inference TensorFlow Serving uses the same API as normal TensorFlow Serving\. The only difference is that the entry point is a different binary named `amazonei_tensorflow_model_server`\. This file is found at `/usr/local/bin/amazonei_tensorflow_model_server`\. The following example shows commands to use TensorFlow serving\.

### EI TensorFlow Serving Examples<a name="ei-tf-serving-examples"></a>

The following is an example you can try for serving different models like ResNet using a Single Shot Detector \(SSD\)\. As a general rule, you need a servable model and client scripts to be already downloaded to your DLAMI\.

**Serve and Test Inference with an SSD Model**

1. Download the model\.

   ```
   $ curl -O https://s3-us-west-2.amazonaws.com/aws-tf-serving-ei-example/ssd_resnet.zip
   ```

1. Unzip the model\.

   ```
   $ unzip ssd_resnet.zip -d /tmp
   ```

1. Download a picture of three dogs to your home directory\.

   ```
   $ curl -O https://raw.githubusercontent.com/awslabs/mxnet-model-server/master/docs/images/3dogs.jpg
   ```

1. Launch the server\. Note, "model\_base\_path" must be an absolute path:

   ```
   $ amazonei_tensorflow_model_server --model_name=ssdresnet --model_base_path=/tmp/ssd_resnet50_v1_coco --port=9000
   ```

1. With the server running in the foreground you will need to launch another terminal session to continue\.

1. Activate your preferred TensorFlow EI environment\. For example:

   ```
   $ source activate amazonei_tensorflow_p27
   ```

1. Now open a text editor, such as vim, and paste the following inference script\. Save the file as `ssd_resnet_client.py`\.

   ```
   from __future__ import print_function
   
   import grpc
   import tensorflow as tf
   from PIL import Image
   import numpy as np
   import time
   import os
   from tensorflow_serving.apis import predict_pb2
   from tensorflow_serving.apis import prediction_service_pb2_grpc
   
   tf.app.flags.DEFINE_string('server', 'localhost:9000',
                              'PredictionService host:port')
   tf.app.flags.DEFINE_string('image', '', 'path to image in JPEG format')
   FLAGS = tf.app.flags.FLAGS
   
   coco_classes_txt = "https://raw.githubusercontent.com/amikelive/coco-labels/master/coco-labels-paper.txt"
   local_coco_classes_txt = "/tmp/coco-labels-paper.txt"
   # it's a file like object and works just like a file
   os.system("curl -o %s -O %s"%(local_coco_classes_txt, coco_classes_txt))
   NUM_PREDICTIONS = 5
   with open(local_coco_classes_txt) as f:
     classes = ["No Class"] + [line.strip() for line in f.readlines()]
   
   
   def main(_):
     channel = grpc.insecure_channel(FLAGS.server)
     stub = prediction_service_pb2_grpc.PredictionServiceStub(channel)
   
     # Send request
     with Image.open(FLAGS.image) as f:
       f.load()
       # See prediction_service.proto for gRPC request/response details.
       data = np.asarray(f)
       data = np.expand_dims(data, axis=0)
   
       request = predict_pb2.PredictRequest()
       request.model_spec.name = 'ssdresnet'
       request.inputs['inputs'].CopyFrom(
           tf.contrib.util.make_tensor_proto(data, shape=data.shape))
       result = stub.Predict(request, 60.0)  # 10 secs timeout
       outputs = result.outputs
       detection_classes = outputs["detection_classes"]
       detection_classes = tf.make_ndarray(detection_classes)
       num_detections = int(tf.make_ndarray(outputs["num_detections"])[0])
       print("%d detection[s]" % (num_detections))
       class_label = [classes[int(x)]
                      for x in detection_classes[0][:num_detections]]
       print("SSD Prediction is ", class_label)
   
   
   if __name__ == '__main__':
     tf.app.run()
   ```

1. Run the script passing the server location and port and the dog photo's filename as the parameters\.

   ```
   $ python ssd_resnet_client.py --server=localhost:9000 --image 3dogs.jpg
   ```

### Using EI TensorFlow Predictor<a name="ei-tf-predictor"></a>

The EIPredictor API provides simple interface to perform repeated inference on a pretrained model\.

The following code sample shows the available parameters\.

```
ei_predictor = EIPredictor(model_dir,
           signature_def_key=None,
           signature_def=None,
           input_names=None,
           output_names=None,
           tags=None,
           graph=None,
           config=None,
           use_ei=True)

output_dict = ei_predictor(feed_dict)
```

The usage of EIPredictor is similar to TF predictor for a [saved model](https://www.tensorflow.org/api_docs/python/tf/contrib/predictor/from_saved_model)\. EIPredictor can be used in the following ways:

```
//EIPredictor class picks inputs and outputs from default serving signature def with tag “serve”. (similar to TF predictor)
ei_predictor = EIPredictor(model_dir)

//EI Predictor class picks inputs and outputs from the signature def picked using the signtaure_def_key (similar to TF predictor)
ei_predictor = EIPredictor(model_dir, signature_def_key='predict')

// Signature_def can be provided directly (similar to TF predictor)
ei_predictor = EIPredictor(model_dir, signature_def= sig_def)

// You provide the input_names and output_names dict.
// similar to TF predictor
ei_predictor = EIPredictor(model_dir,
                             input_names,
                             output_names)

// tag is used to get the correct signature def. (similar to TF predictor)
ei_predictor = EIPredictor(model_dir, tags='serve')
```

Additional EI Predictor functionality:

1. Supports frozen models:

   ```
   // For Frozen graphs, model_dir takes a file name , input_names and output_names
   // input_names and output_names should be provided in this case.
   ei_predictor = EIPredictor(model_dir,
                                input_names=None,
                                output_names=None )
   ```

1. You can also disable usage of EI by using the `use_ei` flag which is defaulted to `True`\. This is useful to test EIPredictor against Tensorflow Predictor\.

1. EIPredictor can also be created from a TensorFlow Estimator\. Given a trained Estimator, you first export a SavedModel\. Refer to the [SavedModel documentation](https://github.com/tensorflow/tensorflow/tree/master/tensorflow/contrib/predictor#predictor-from-a-savedmodel) for more details\. Example usage:

   ```
   saved_model_dir = estimator.export_savedmodel(my_export_dir, serving_input_fn)
   ei_predictor = EIPredictor(export_dir=saved_model_dir)
   // Once the EIPredictor is created, inference is done using the following:
   output_dict = ei_predictor(feed_dict)
   ```

### EI TensorFlow Predictor Examples<a name="ei-tf-predictor-examples"></a>

The following is an example you can try for serving different models like ResNet using a Single Shot Detector \(SSD\)\. As a general rule, you need a servable model and client scripts to be already downloaded to your DLAMI\.

**Serve and Test Inference with an SSD Model**

1. Download the model\. If you already downloaded this model in the serving example, you may skip this step\.

   ```
   $ curl -O https://s3-us-west-2.amazonaws.com/aws-tf-serving-ei-example/ssd_resnet.zip
   ```

1. Unzip the model\. Again, you may skip this step if you already have the model\.

   ```
   $ unzip ssd_resnet.zip -d /tmp
   ```

1. Download a picture of three dogs to your current directory\.

   ```
   $ curl -O https://raw.githubusercontent.com/awslabs/mxnet-model-server/master/docs/images/3dogs.jpg
   ```

1. Now open a text editor, such as vim, and paste the following inference script\. Save the file as `ssd_resnet_predictor.py`\.

   ```
   from __future__ import absolute_import
   from __future__ import division
   from __future__ import print_function
   
   import os
   import sys
   import numpy as np
   import tensorflow as tf
   import matplotlib.image as mpimg
   from tensorflow.contrib.ei.python.predictor.ei_predictor import EIPredictor
   
   tf.app.flags.DEFINE_string('image', '', 'path to image in JPEG format')
   FLAGS = tf.app.flags.FLAGS
   
   coco_classes_txt = "https://raw.githubusercontent.com/amikelive/coco-labels/master/coco-labels-paper.txt"
   local_coco_classes_txt = "/tmp/coco-labels-paper.txt"
   # it's a file like object and works just like a file
   os.system("curl -o %s -O %s"%(local_coco_classes_txt, coco_classes_txt))
   NUM_PREDICTIONS = 5
   with open(local_coco_classes_txt) as f:
     classes = ["No Class"] + [line.strip() for line in f.readlines()]
   
   
   def get_output(eia_predictor, test_input):
     pred = None
     for curpred in range(NUM_PREDICTIONS):
       pred = eia_predictor(test_input)
   
     num_detections = int(pred["num_detections"])
     print("%d detection[s]" % (num_detections))
     detection_classes = pred["detection_classes"][0][:num_detections]
     print([classes[int(i)] for i in detection_classes])
   
   
   def main(_):
   
     img = mpimg.imread(FLAGS.image)
     img = np.expand_dims(img, axis=0)
     ssd_resnet_input = {'inputs': img}
   
     print('Running SSD Resnet on EIPredictor using specified input and outputs')
     eia_predictor = EIPredictor(
         model_dir='/tmp/ssd_resnet50_v1_coco/1/',
         input_names={"inputs": "image_tensor:0"},
         output_names={"detection_classes": "detection_classes:0", "num_detections": "num_detections:0",
                       "detection_boxes": "detection_boxes:0"},
     )
     get_output(eia_predictor, ssd_resnet_input)
   
     print('Running SSD Resnet on EIPredictor using default Signature Def')
     eia_predictor = EIPredictor(
         model_dir='/tmp/ssd_resnet50_v1_coco/1/',
     )
     get_output(eia_predictor, ssd_resnet_input)
   
   
   if __name__ == "__main__":
     tf.app.run()
   ```

1. Run the inference script\.

   ```
   $ python ssd_resnet_predictor.py --image 3dogs.jpg
   ```

### Considerations When Using EI\-enabled TensorFlow<a name="ei-tf-considerations"></a>
+ Warmup: Tensorflow Serving provides a warmup feature to pre\-load models and reduce the delay that is typical of the first inference request\. Amazon EI TensorFlow Serving only supports warming up the “serving\_default” signature definition\.
+ Signature Definitions: Using multiple signature definitions can have a multiplicative effect on the amount of accelerator memory consumed\. If you plan to exercise more than one signature definition for your inference calls, you should test these scenarios as you determine the accelerator type for your application\.
+ For large models, EI tends to have larger memory overhead, and might lead to an out of memory error\. If you get this error, try switching to next higher EI Accelerator type\.

### More Models and Resources<a name="tutorial-tf-elastic-inference-more"></a>

1. [TensorFlow Serving](tutorial-tfserving.md) \- TensorFlow Serving has inference examples that can be used with EI\.

For more tutorials and examples, see the framework's official Python documentation, the [TensorFlow Python API](https://www.tensorflow.org/api_docs/python/)\.