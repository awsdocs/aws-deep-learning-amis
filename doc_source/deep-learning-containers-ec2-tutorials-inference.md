# Inference<a name="deep-learning-containers-ec2-tutorials-inference"></a>

This section will guide you on how to run inference on Deep Learning Containers for EC2 using MXNet and TensorFlow\.

For a complete list of AWS Deep Learning Containers, refer to [Deep Learning Containers Images](deep-learning-containers-images.md)\. 

**Topics**
+ [TensorFlow Inference](#deep-learning-containers-ec2-tutorials-inference-tf)
+ [MXNet Inference](#deep-learning-containers-ec2-tutorials-inference-mxnet)

## TensorFlow Inference<a name="deep-learning-containers-ec2-tutorials-inference-tf"></a>

To demonstrate how we can use Deep Learning Containers for inference, we will use the mnist model example in TensorFlow Serving\. We recommend using the Deep Learning AMI with Conda for TensorFlow\. On your DLAMI:

```
source activate tensorflow_p27
git clone https://github.com/tensorflow/serving.git
cd serving
git checkout r1.12
```

Note that this example model with 1 epoch is for demonstration purposes and will not have high accuracy\. Run the following command to generate the model on your DLAMI\. 

```
python tensorflow_serving/example/mnist_saved_model.py models/mnist
```

Now we can start TensorFlow serving using the Deep Learning Containers for this model\. Note that unlike the Deep Learning Containers for training, model serving starts immediately upon running the container and runs as a background process\.
+ For CPU:

  ```
  docker run -p 8500:8500 --mount type=bind,source=$(pwd)/models/mnist,target=/models/mnist -e MODEL_NAME=mnist -t <cpu inference container> &
  ```
+ For GPU:

  ```
  nvidia-docker run -p 8500:8500 --mount type=bind,source=$(pwd)/models/mnist,target=/models/mnist -e MODEL_NAME=mnist -t <gpu inference container> &
  ```

In a new terminal for your DLAMI, we can run inference with the AWS Deep Learning Containers\. Remember to activate the same Conda environment \(e\.g\., tensorflow\_p27\):

```
python tensorflow_serving/example/mnist_client.py --num_tests=1000 --server=127.0.0.1:8500
```

## MXNet Inference<a name="deep-learning-containers-ec2-tutorials-inference-mxnet"></a>

To begin inference with MXNet, we will use a pre\-trained model from a public S3 bucket\.

For CPU instances run:

```
<docker> run -itd --name mms -p 80:8080  -p 8081:8081 <your container image id> \
mxnet-model-server --start --mms-config /home/model-server/config.properties \
--models squeezenet=https://s3.amazonaws.com/model-server/models/squeezenet_v1.1/squeezenet_v1.1.model
```

For GPU instances run:

```
<nvidia-docker> run -itd --name mms -p 80:8080  -p 8081:8081 <your container image id> \
mxnet-model-server --start --mms-config /home/model-server/config.properties \
--models squeezenet=https://s3.amazonaws.com/model-server/models/squeezenet_v1.1/squeezenet_v1.1.model
```

Note: The config file is included in the container\.

With your server started, you can now run inference from a different window using:

```
curl -O https://s3.amazonaws.com/model-server/inputs/kitten.jpg
curl -X POST http://127.0.0.1/predictions/squeezenet -T kitten.jpg
```

Once you are done using your container, you can remove it using:

```
docker rm -f mms
```