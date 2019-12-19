# Inference<a name="deep-learning-containers-ec2-tutorials-inference"></a>

This section will guide you on how to run inference on Deep Learning Containers for EC2 using MXNet, PyTorch, and TensorFlow\.

For a complete list of AWS Deep Learning Containers, refer to [Deep Learning Containers Images](deep-learning-containers-images.md)\. 

**Note**  
MKL users: read the [AWS Deep Learning Containers MKL Recommendations](deep-learning-containers-mkl.md) to get the best training or inference performance\.

**Topics**
+ [TensorFlow Inference](#deep-learning-containers-ec2-tutorials-inference-tf)
+ [MXNet Inference](#deep-learning-containers-ec2-tutorials-inference-mxnet)
+ [PyTorch Inference](#deep-learning-containers-ec2-tutorials-inference-pytorch)

## TensorFlow Inference<a name="deep-learning-containers-ec2-tutorials-inference-tf"></a>

To demonstrate how we can use Deep Learning Containers for inference, we will use a simple "half plus two" model with TensorFlow Serving\. We recommend using the [Deep Learning Base AMI](overview-base.md) for TensorFlow\. After logging into your instance run the following:

```
$ git clone -b r1.14 https://github.com/tensorflow/serving.git
$ cd serving
$ git checkout r1.14
```

Now we can start TensorFlow Serving using the Deep Learning Containers for this model\. Note that unlike the Deep Learning Containers for training, model serving starts immediately upon running the container and runs as a background process\.
+ For CPU:

  ```
  $ docker run -p 8500:8500 -p 8501:8501 --name tensorflow-inference --mount type=bind,source=$(pwd)/tensorflow_serving/servables/tensorflow/testdata/saved_model_half_plus_two_cpu,target=/models/saved_model_half_plus_two -e MODEL_NAME=saved_model_half_plus_two -d <cpu inference container>
  ```

  For example:

  ```
  $ docker run -p 8500:8500 -p 8501:8501 --name tensorflow-inference --mount type=bind,source=$(pwd)/tensorflow_serving/servables/tensorflow/testdata/saved_model_half_plus_two_cpu,target=/models/saved_model_half_plus_two -e MODEL_NAME=saved_model_half_plus_two -d 763104351884.dkr.ecr.us-east-1.amazonaws.com/tensorflow-inference:1.15.0-cpu-py36-ubuntu18.04
  ```
+ For GPU:

  ```
  $ nvidia-docker run -p 8500:8500 -p 8501:8501 --name tensorflow-inference --mount type=bind,source=$(pwd)/tensorflow_serving/servables/tensorflow/testdata/saved_model_half_plus_two_gpu,target=/models/saved_model_half_plus_two -e MODEL_NAME=saved_model_half_plus_two -d <gpu inference container>
  ```

  For example:

  ```
  $ nvidia-docker run -p 8500:8500 -p 8501:8501 --name tensorflow-inference --mount type=bind,source=$(pwd)/tensorflow_serving/servables/tensorflow/testdata/saved_model_half_plus_two_gpu,target=/models/sad_model_half_plus_two -e MODEL_NAME=saved_model_half_plus_two -d 763104351884.dkr.ecr.us-east-1.amazonaws.com/tensorflow-inference:1.15.0-gpu-py36-cu100-ubuntu18.04
  ```

Now we can run inference with the AWS Deep Learning Containers\.

```
$ curl -d '{"instances": [1.0, 2.0, 5.0]}' -X POST http://127.0.0.1:8501/v1/models/saved_model_half_plus_two:predict
```

The output will look similar to the following:

```
{
    "predictions": [2.5, 3.0, 4.5
    ]
}
```

**Note**  
If you want to debug the container's output, you can attach to it using the name:  

```
$ docker attach <your docker container name>
```
In this example we used `tensorflow-inference`\.

## MXNet Inference<a name="deep-learning-containers-ec2-tutorials-inference-mxnet"></a>

To begin inference with MXNet, we will use a pre\-trained model from a public S3 bucket\.

For CPU instances run:

```
$ docker run -itd --name mms -p 80:8080  -p 8081:8081 <your container image id> \
mxnet-model-server --start --mms-config /home/model-server/config.properties \
--models squeezenet=https://s3.amazonaws.com/model-server/models/squeezenet_v1.1/squeezenet_v1.1.model
```

For GPU instances run:

```
$ nvidia-docker run -itd --name mms -p 80:8080  -p 8081:8081 <your container image id> \
mxnet-model-server --start --mms-config /home/model-server/config.properties \
--models squeezenet=https://s3.amazonaws.com/model-server/models/squeezenet_v1.1/squeezenet_v1.1.model
```

Note: The config file is included in the container\.

With your server started, you can now run inference from a different window using:

```
$ curl -O https://s3.amazonaws.com/model-server/inputs/kitten.jpg
curl -X POST http://127.0.0.1/predictions/squeezenet -T kitten.jpg
```

Once you are done using your container, you can remove it using:

```
$ docker rm -f mms
```

## PyTorch Inference<a name="deep-learning-containers-ec2-tutorials-inference-pytorch"></a>

To begin inference with PyTorch, we will use a model pre\-trained on Imagenet from a public S3 bucket\. Similar to MXNet containers, inference is served using mxnet\-model\-server, which can support any framework as the backend\. For more information, see [Model Server for Apache MXNet](https://github.com/awslabs/mxnet-model-server) and this blog on [Deploying PyTorch inference with MXNet Model Server](https://aws.amazon.com/blogs/machine-learning/deploying-pytorch-inference-with-mxnet-model-server/)\.

For CPU instances, run:

```
$ docker run -itd --name mms -p 80:8080  -p 8081:8081 <your container image id> \
mxnet-model-server --start --mms-config /home/model-server/config.properties \
--models densenet=https://dlc-samples.s3.amazonaws.com/pytorch/multi-model-server/densenet/densenet.mar
```

For GPU instances, run:

```
$ nvidia-docker run -itd --name mms -p 80:8080  -p 8081:8081 <your container image id> \
mxnet-model-server --start --mms-config /home/model-server/config.properties \
--models densenet=https://dlc-samples.s3.amazonaws.com/pytorch/multi-model-server/densenet/densenet.mar
```

If you have docker\-ce version >=19\.03, you can use the \-\-gpus flag with docker run\.

Note: The config file is included in the container\.

With your server started, you can now run inference from a different window using:

```
$ curl -O https://s3.amazonaws.com/model-server/inputs/flower.jpg
curl -X POST http://127.0.0.1/predictions/densenet -T flower.jpg
```

Once you are done using your container, you can remove it using:

```
$ docker rm -f mms
```