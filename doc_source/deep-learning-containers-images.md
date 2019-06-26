# Deep Learning Containers Images<a name="deep-learning-containers-images"></a>

AWS Deep Learning Containers are available as Docker images in Amazon ECR\. Each Docker image is built for training or inference on a specific Deep Learning framework version, python version, with CPU or GPU support\. The following table lists the Docker image URLs that will be used by Amazon ECS in task definitions\.

AWS Deep Learning Containers Docker Images are available in the following regions: **us\-east\-1, **us\-east\-2, **us\-west\-1, ****us\-west\-2, ap\-northeast\-1, **ap\-northeast\-2,****ap\-south\-1, **ap\-southeast\-1, **ap\-southeast\-2, **sa\-east\-1, ca\-central\-1, **eu\-central\-1, ****eu\-north\-1, ****eu\-west\-1, **eu\-west\-2, eu\-west\-3**

ECR is a regional service and the Image table contains the URLs for **us\-east\-1** images\. To pull from one of the regions mentioned previously, insert the region in the repository URL following this example:

```
763104351884.dkr.ecr.<region>.amazonaws.com/tensorflow-training:1.13-cpu-py27-ubuntu16.04
```

**Important**  
You must login to access to the image repository before pulling the image\. Specify your region in the following command:  

```
$(aws ecr get-login --no-include-email --region us-east-1 --registry-ids 763104351884)
```

 You can then pull these Docker images from Amazon ECR by running:

```
docker pull <name of container image>
```


|  Framework  |  Training/Inference  |  Horovod  |  CPU/GPU  |  Python Version  |  URL  | 
| --- | --- | --- | --- | --- | --- | 
|  TensorFlow 1\.13\.1  |  Training  |  No  |  CPU  |  2\.7  | 763104351884\.dkr\.ecr\.us\-east\-1\.amazonaws\.com/tensorflow\-training:1\.13\-cpu\-py27\-ubuntu16\.04 | 
|  TensorFlow 1\.13\.1  |  Training  |  No  |  CPU  |  3\.6  | 763104351884\.dkr\.ecr\.us\-east\-1\.amazonaws\.com/tensorflow\-training:1\.13\-cpu\-py36\-ubuntu16\.04 | 
|  TensorFlow 1\.13\.1  |  Training  |  Yes  |  GPU  |  2\.7  |  763104351884\.dkr\.ecr\.us\-east\-1\.amazonaws\.com/tensorflow\-training:1\.13\-horovod\-gpu\-py27\-cu100\-ubuntu16\.04  | 
|  TensorFlow 1\.13\.1  |  Training  |  Yes  |  GPU  |  3\.6  | 763104351884\.dkr\.ecr\.us\-east\-1\.amazonaws\.com/tensorflow\-training:1\.13\-horovod\-gpu\-py36\-cu100\-ubuntu16\.04 | 
|  TensorFlow 1\.13\.0  |  Inference  |  No  |  CPU  |  2\.7  | 763104351884\.dkr\.ecr\.us\-east\-1\.amazonaws\.com/tensorflow\-inference:1\.13\-cpu\-py27\-ubuntu18\.04 | 
|  TensorFlow 1\.13\.0  |  Inference  |  No  |  CPU  |  3\.6  | 763104351884\.dkr\.ecr\.us\-east\-1\.amazonaws\.com/tensorflow\-inference:1\.13\-cpu\-py36\-ubuntu18\.04 | 
|  TensorFlow 1\.13\.0  |  Inference  |  No  |  GPU  |  2\.7  | 763104351884\.dkr\.ecr\.us\-east\-1\.amazonaws\.com/tensorflow\-inference:1\.13\-gpu\-py27\-cu100\-ubuntu16\.04 | 
|  TensorFlow 1\.13\.0  |  Inference  |  No  |  GPU  |  3\.6  | 763104351884\.dkr\.ecr\.us\-east\-1\.amazonaws\.com/tensorflow\-inference:1\.13\-gpu\-py36\-cu100\-ubuntu16\.04 | 
|  MXNet 1\.4\.1  |  Training  |  No  |  CPU  |  2\.7  | 763104351884\.dkr\.ecr\.us\-east\-1\.amazonaws\.com/mxnet\-training:1\.4\.1\-cpu\-py27\-ubuntu16\.04 | 
|  MXNet 1\.4\.1  |  Training  |  No  |  CPU  |  3\.6  | 763104351884\.dkr\.ecr\.us\-east\-1\.amazonaws\.com/mxnet\-training:1\.4\.1\-cpu\-py36\-ubuntu16\.04 | 
|  MXNet 1\.4\.1  |  Training  |  No  |  GPU  |  2\.7  | 763104351884\.dkr\.ecr\.us\-east\-1\.amazonaws\.com/mxnet\-training:1\.4\.1\-gpu\-py27\-cu100\-ubuntu16\.04 | 
|  MXNet 1\.4\.1  |  Training  |  No  |  GPU  |  3\.6  | 763104351884\.dkr\.ecr\.us\-east\-1\.amazonaws\.com/mxnet\-training:1\.4\.1\-gpu\-py36\-cu100\-ubuntu16\.04 | 
|  MXNet 1\.4\.1  |  Inference  |  No  |  CPU  |  2\.7  | 763104351884\.dkr\.ecr\.us\-east\-1\.amazonaws\.com/mxnet\-inference:1\.4\.1\-cpu\-py27\-ubuntu16\.04 | 
|  MXNet 1\.4\.1  |  Inference  |  No  |  CPU  |  3\.6  | 763104351884\.dkr\.ecr\.us\-east\-1\.amazonaws\.com/mxnet\-inference:1\.4\.1\-cpu\-py36\-ubuntu16\.04 | 
|  MXNet 1\.4\.1  |  Inference  |  No  |  GPU  |  2\.7  | 763104351884\.dkr\.ecr\.us\-east\-1\.amazonaws\.com/mxnet\-inference:1\.4\.1\-gpu\-py27\-cu100\-ubuntu16\.04 | 
|  MXNet 1\.4\.1  |  Inference  |  No  |  GPU  |  3\.6  | 763104351884\.dkr\.ecr\.us\-east\-1\.amazonaws\.com/mxnet\-inference:1\.4\.1\-gpu\-py36\-cu100\-ubuntu16\.04 | 

## Prior Container Versions<a name="deep-learning-containers-images-previous"></a>


|  Framework  |  Training/Inference  |  Horovod  |  CPU/GPU  |  Python Version  |  URL  | 
| --- | --- | --- | --- | --- | --- | 
|  TensorFlow 1\.13\.0  |  Inference  |  No  |  CPU  |  2\.7  | 763104351884\.dkr\.ecr\.us\-east\-1\.amazonaws\.com/tensorflow\-inference:1\.13\-cpu\-py27\-ubuntu16\.04 | 
|  TensorFlow 1\.13\.0  |  Inference  |  No  |  CPU  |  3\.6  | 763104351884\.dkr\.ecr\.us\-east\-1\.amazonaws\.com/tensorflow\-inference:1\.13\-cpu\-py36\-ubuntu16\.04 | 
|  MXNet 1\.4\.0   |  Training  |  No  |  CPU  |  2\.7  | 763104351884\.dkr\.ecr\.us\-east\-1\.amazonaws\.com/mxnet\-training:1\.4\.0\-cpu\-py27\-ubuntu16\.04 | 
|  MXNet 1\.4\.0   |  Training  |  No  |  CPU  |  3\.6  | 763104351884\.dkr\.ecr\.us\-east\-1\.amazonaws\.com/mxnet\-training:1\.4\.0\-cpu\-py36\-ubuntu16\.04 | 
|  MXNet 1\.4\.0   |  Training  |  No  |  GPU  |  2\.7  | 763104351884\.dkr\.ecr\.us\-east\-1\.amazonaws\.com/mxnet\-training:1\.4\.0\-gpu\-py27\-cu90\-ubuntu16\.04 | 
|  MXNet 1\.4\.0   |  Training  |  No  |  GPU  |  3\.6  | 763104351884\.dkr\.ecr\.us\-east\-1\.amazonaws\.com/mxnet\-training:1\.4\.0\-gpu\-py36\-cu90\-ubuntu16\.04 | 
|  MXNet 1\.4\.0   |  Inference  |  No  |  CPU  |  2\.7  | 763104351884\.dkr\.ecr\.us\-east\-1\.amazonaws\.com/mxnet\-inference:1\.4\.0\-cpu\-py27\-ubuntu16\.04 | 
|  MXNet 1\.4\.0   |  Inference  |  No  |  CPU  |  3\.6  | 763104351884\.dkr\.ecr\.us\-east\-1\.amazonaws\.com/mxnet\-inference:1\.4\.0\-cpu\-py36\-ubuntu16\.04 | 
|  MXNet 1\.4\.0   |  Inference  |  No  |  GPU  |  2\.7  | 763104351884\.dkr\.ecr\.us\-east\-1\.amazonaws\.com/mxnet\-inference:1\.4\.0\-gpu\-py27\-cu90\-ubuntu16\.04 | 
|  MXNet 1\.4\.0   |  Inference  |  No  |  GPU  |  3\.6  | 763104351884\.dkr\.ecr\.us\-east\-1\.amazonaws\.com/mxnet\-inference:1\.4\.0\-gpu\-py36\-cu90\-ubuntu16\.04 | 