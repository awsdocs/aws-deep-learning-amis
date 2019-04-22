# AWS Deep Learning Containers Custom Images<a name="deep-learning-containers-custom-images"></a>

## How to Build Custom Images<a name="dlc-custom"></a>

We can easily customize both training and inference with AWS Deep Learning Containers to add custom frameworks, libraries, and packages using dockerfiles\.

### Training with TensorFlow<a name="dlc-custom-training"></a>

In the following example Dockerfile, the resulting Docker image will have TensorFlow v1\.13 optimized for GPUs and built to support Horovod and Python 3 for multi\-node distributed training\. It will also have the AWS samples GitHub repo which contains many deep learning model examples\.

```
#Take base container
FROM 763104351884.dkr.ecr.us-east-1.amazonaws.com/tensorflow-training:1.13-horovod-gpu-py36-cu100-ubuntu16.04

# Add custom stack of code
RUN git clone https://github.com/aws-samples/deep-learning-models
```

### Training with MXNet<a name="dlc-custom-inference"></a>

In the following example Dockerfile, the resulting Docker image will have MXNet v1\.4\.0 optimized for GPU inference using Python 3\. It will also have the MXNet GitHub repo which contains many deep learning model examples\.

```
# Take the base MXNet Container
FROM 763104351884.dkr.ecr.us-east-1.amazonaws.com/mxnet-training:1.4.0-gpu-py36-cu90-ubuntu16.04

# Add Custom stack of code
RUN git clone -b 1.4.0 https://github.com/apache/incubator-mxnet.git

ENTRYPOINT ["python", "/incubator-mxnet/example/image-classification/train_mnist.py"]
```

Build the Docker image, pointing to your personal Docker registry \(usually your username\), with the image's custom name and custom tag\. 

```
docker build -f Dockerfile -t <registry>/<any name>:<any tag>
```

Push to your personal Docker Registry:

```
docker push <registry>/<any name>:<any tag>
```

You can use the following command to run the container:

```
docker run -it < name or tag>
```

**Important**  
You may need to login to access to the image repository\. Specify your region and registry ID in the following command:  

```
$(aws ecr get-login --no-include-email --region us-east-1 --registry-ids YOUR_AWS_ACCOUNT_ID)
```