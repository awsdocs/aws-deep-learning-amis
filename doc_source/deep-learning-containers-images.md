# Deep Learning Containers Images<a name="deep-learning-containers-images"></a>

AWS Deep Learning Containers are available as Docker images in Amazon ECR\. Each Docker image is built for training or inference on a specific Deep Learning framework version, python version, with CPU or GPU support\. The following table lists the Docker image URLs that will be used by Amazon ECS in task definitions\.

AWS Deep Learning Containers Docker Images are available in the following regions: **us\-east\-1, **us\-east\-2, **us\-west\-1, ****us\-west\-2, ap\-northeast\-1, **ap\-northeast\-2,****ap\-south\-1, **ap\-southeast\-1, **ap\-southeast\-2, **sa\-east\-1, ca\-central\-1, **eu\-central\-1, ****eu\-north\-1, ****eu\-west\-1, **eu\-west\-2, eu\-west\-3**

Note: **eu\-north\-1** is available starting in the version 2\.0 release\. As a result, MXNet\-1\.4\.0 is not available in this region\.

ECR is a regional service and the Image table contains the URLs for **us\-east\-1** images\. To pull from one of the regions mentioned previously, insert the region in the repository URL following this example:

```
763104351884.dkr.ecr.<region>.amazonaws.com/tensorflow-training:1.15.0-cpu-py27-ubuntu18.04
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

## General Framework Containers<a name="deep-learning-containers-images-table"></a>

To use the following table, select your desired framework, as well as the kind of job you're starting, and the desired python version\. Plug this information into the replaceable portions of the URL as shown in the example URL\.


|  Framework  |  Job Type  |  Horovod Options  |  CPU/GPU  |  Python Version Options  |  Example URL  | 
| --- | --- | --- | --- | --- | --- | 
|  TensorFlow 1\.15\.0  |  Training, Inference  | Training:Yes, Inference: No |  CPU  |  2\.7 \(py27\), 3\.6 \(py36\)  | 763104351884\.dkr\.ecr\.us\-east\-1\.amazonaws\.com/tensorflow\-training:1\.15\.0\-cpu\-py27\-ubuntu18\.04 | 
|  TensorFlow 1\.15\.0  |  Training, Inference  |  Training: Yes, Inference: No  |  GPU  |  2\.7 \(py27\), 3\.6 \(py36\)  | 763104351884\.dkr\.ecr\.us\-east\-1\.amazonaws\.com/tensorflow\-training:1\.15\.0\-gpu\-py27\-cu100\-ubuntu18\.04 | 
|  MXNet 1\.6\.0rc0  |  Training, Inference  |  No  |  CPU  |  2\.7 \(py27\), 3\.6 \(py36\)  | 763104351884\.dkr\.ecr\.us\-east\-1\.amazonaws\.com/mxnet\-training:1\.6\.0\-cpu\-py27\-ubuntu16\.04 | 
|  MXNet 1\.6\.0rc0  |  Training, Inference  |  No  |  GPU  |  2\.7 \(py27\), 3\.6 \(py36\)  | 763104351884\.dkr\.ecr\.us\-east\-1\.amazonaws\.com/mxnet\-training:1\.6\.0\-gpu\-py27\-cu101\-ubuntu16\.04 | 
| PyTorch 1\.3\.1 |  Training, Inference  | No |  CPU  |  2\.7 \(py27\), 3\.6 \(py36\)  | 763104351884\.dkr\.ecr\.us\-east\-1\.amazonaws\.com/pytorch\-training:1\.3\.1\-cpu\-py27\-ubuntu16\.04 | 
| PyTorch 1\.3\.1 |  Training, Inference  |  Training:Yes, Inference: No  |  GPU  |  2\.7 \(py27\), 3\.6 \(py36\)  | 763104351884\.dkr\.ecr\.us\-east\-1\.amazonaws\.com/pytorch\-training:1\.3\.1\-gpu\-py27\-cu101\-ubuntu16\.04 | 

## Elastic Inference Containers<a name="deep-learning-containers-images-elastic"></a>


|  Framework  |  Job Type  |  Horovod Options  |  CPU/GPU  |  Python Version Options  |  Example URL  | 
| --- | --- | --- | --- | --- | --- | 
|  TensorFlow 1\.14\.0 with Elastic Inference  |  Inference  |  No  |  CPU  |  2\.7 \(py27\), 3\.6 \(py36\)  | 763104351884\.dkr\.ecr\.us\-east\-1\.amazonaws\.com/tensorflow\-inference\-eia:1\.14\.0\-cpu\-py27\-ubuntu16\.04 | 
|  MXNet 1\.4\.1 with Elastic Inference  |  Inference  |  No  |  CPU  |  2\.7 \(py27\), 3\.6 \(py36\)  | 763104351884\.dkr\.ecr\.us\-east\-1\.amazonaws\.com/mxnet\-inference\-eia:1\.4\.1\-cpu\-py27\-ubuntu16\.04 | 

## Prior Versions<a name="deep-learning-containers-images-previous"></a>


|  Framework  |  Job Type  |  Horovod Options  |  CPU/GPU  |  Python Version Options  |  Example URL  | 
| --- | --- | --- | --- | --- | --- | 
|  TensorFlow 1\.14\.0  |  Training, Inference  | Training:Yes, Inference: No |  CPU  |  2\.7 \(py27\), 3\.6 \(py36\)  | 763104351884\.dkr\.ecr\.us\-east\-1\.amazonaws\.com/tensorflow\-training:1\.14\.0\-cpu\-py27\-ubuntu16\.04 | 
|  TensorFlow 1\.14\.0  |  Training, Inference  |  Training: Yes, Inference: No  |  GPU  |  2\.7 \(py27\), 3\.6 \(py36\)  | 763104351884\.dkr\.ecr\.us\-east\-1\.amazonaws\.com/tensorflow\-training:1\.14\.0\-gpu\-py27\-cu100\-ubuntu16\.04 | 
|  MXNet 1\.4\.1  |  Training, Inference  |  No  |  CPU  |  2\.7 \(py27\), 3\.6 \(py36\)  | 763104351884\.dkr\.ecr\.us\-east\-1\.amazonaws\.com/mxnet\-training:1\.4\.1\-cpu\-py27\-ubuntu16\.04 | 
|  MXNet 1\.4\.1  |  Training, Inference  |  No  |  GPU  |  2\.7 \(py27\), 3\.6 \(py36\)  | 763104351884\.dkr\.ecr\.us\-east\-1\.amazonaws\.com/mxnet\-training:1\.4\.1\-gpu\-py27\-cu100\-ubuntu16\.04 | 
| PyTorch 1\.2\.0 |  Training, Inference  | No |  CPU  |  2\.7 \(py27\), 3\.6 \(py36\)  | 763104351884\.dkr\.ecr\.us\-east\-1\.amazonaws\.com/pytorch\-training:1\.2\.0\-cpu\-py27\-ubuntu16\.04 | 
| PyTorch 1\.2\.0 |  Training, Inference  |  Training:Yes, Inference: No  |  GPU  |  2\.7 \(py27\), 3\.6 \(py36\)  | 763104351884\.dkr\.ecr\.us\-east\-1\.amazonaws\.com/pytorch\-training:1\.2\.0\-gpu\-py27\-cu100\-ubuntu16\.04 | 