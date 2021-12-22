# Using the Graviton GPU TensorFlow DLAMI<a name="tutorial-graviton-tensorflow"></a>

The AWS Deep Learning AMI is ready to use with Arm processor\-based Graviton GPUs, and comes optimized for TensorFlow\. The Graviton GPU TensorFlow DLAMI includes a Python environment pre\-configured with [TensorFlow Serving](https://www.tensorflow.org/tfx/guide/serving) for deep learning inference use cases\. Check the [release notes](http://aws.amazon.com/releasenotes/aws-deep-learning-ami-graviton-gpu-tensorflow-2-6-ubuntu-20-04/) for additional details on the Graviton GPU TensorFlow DLAMI\.

**Topics**
+ [Verify TensorFlow Serving Availability](#tutorial-graviton-tensorflow-serving)
+ [Verify TensorFlow and TensorFlow Serving API Availability](#tutorial-graviton-tensorflow-api)
+ [Run Example Inference with TensorFlow Serving](#tutorial-graviton-tensorflow-inference)

## Verify TensorFlow Serving Availability<a name="tutorial-graviton-tensorflow-serving"></a>

Run the following command to verify the availability and version of TensorFlow Serving:

```
tensorflow_model_server --version
```

Your output should look similar to the following:

```
TensorFlow ModelServer: 0.0.0+dev.sha.3e05381e
TensorFlow Library: 2.8.0
```

## Verify TensorFlow and TensorFlow Serving API Availability<a name="tutorial-graviton-tensorflow-api"></a>

Run the following command to verify the availability of TensorFlow and the TensorFlow Serving API:

```
python3 -c "import tensorflow, tensorflow_serving"
```

If the command is successful, there is no output\.

## Run Example Inference with TensorFlow Serving<a name="tutorial-graviton-tensorflow-inference"></a>

Use the following commands to download a pre\-trained ResNet50 model and run inference using TensorFlow Serving:

```
# Clone the TensorFlow Serving repository
git clone https://github.com/tensorflow/serving

# Download pre-trained ResNet50 model
mkdir -p ${HOME}/resnet/1 && cd ${HOME}/resnet/1
wget https://tfhub.dev/tensorflow/resnet_50/classification/1?tf-hub-format=compressed -O resnet_50_classification_1.tar.gz
tar -xzvf resnet_50_classification_1.tar.gz && rm resnet_50_classification_1.tar.gz

# Start TensorFlow Serving
cd $HOME
tensorflow_model_server \
  --rest_api_port=8501 \
  --model_name="resnet" \
  --model_base_path="${HOME}/resnet" &
```

Your output should look similar to the following:

```
2021-11-10 06:18:51.028341: I tensorflow_serving/model_servers/server_core.cc:486] Finished adding/updating models
2021-11-10 06:18:51.028420: I tensorflow_serving/model_servers/server.cc:133] Using InsecureServerCredentials
2021-11-10 06:18:51.028460: I tensorflow_serving/model_servers/server.cc:383] Profiler service is enabled
2021-11-10 06:18:51.028889: I tensorflow_serving/model_servers/server.cc:409] Running gRPC ModelServer at 0.0.0.0:8500 ...
[evhttp_server.cc : 245] NET_LOG: Entering the event loop ...
2021-11-10 06:18:51.030985: I tensorflow_serving/model_servers/server.cc:430] Exporting HTTP/REST API at:localhost:8501 ...
```

Use the TensorFlow Serving `resnet_client` [example](https://github.com/tensorflow/serving/tree/master/tensorflow_serving/example) to run inference:

```
python3 serving/tensorflow_serving/example/resnet_client.py
```

Your output should look similar to the following:

```
2021-11-10 06:18:59.335327: I external/org_tensorflow/tensorflow/stream_executor/cuda/cuda_dnn.cc:368] Loaded cuDNN version 8204
2021-11-10 06:18:59.956156: I external/org_tensorflow/tensorflow/core/platform/default/subprocess.cc:304] Start cannot spawn child process
Prediction class: 285, avg latency: 111.4673 ms
```

Stop TensorFlow Serving with the following command:

```
kill $(pidof tensorflow_model_server)
```

**Next Up**  
[Using the Graviton GPU PyTorch DLAMI](tutorial-graviton-pytorch.md)