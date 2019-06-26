# AWS Deep Learning Containers MKL Recommendations<a name="deep-learning-containers-mkl"></a>

## MKL Recommendation for CPU containers<a name="XPH9CAQN4za"></a>

**Topics**
+ [EC2 guide to set environment variables ](#XPH9CAKZARe)
+ [ECS guide to set environment variables ](#XPH9CAuufwx)
+ [EKS guide to set environment variables](#XPH9CAAktKY)

 The performance for training and inference workloads for a Deep Learning framework on CPU instances can vary and depend on a variety of configuration settings\. As an example, on AWS EC2 c5\.18xlarge instances the number of physical cores is 36 while the number of logical cores is 72\. MKL's configuration settings for training and inference are influenced by these factors\. By updating MKL's configuration to match your instance's capabilities, you may achieve performance improvements\. 

 ​ 

 Consider the following examples using an Intel\-MKL\-optimized TensorFlow binary: 
+ <a name="XPH9CAJEoFn"></a>A ResNet50v2 model, trained with TensorFlow and served for inference with TensorFlow Serving was observed to achieve 2x inference performance when the MKL settings were adjusted to match the instance's number cores\. The following settings were used on a c5\.18xlarge instance\. 

```
export TENSORFLOW_INTER_OP_PARALLELISM=2
# For an EC2 c5.18xlarge instance, number of logical cores = 72
export TENSORFLOW_INTRA_OP_PARALLELISM=72
# For an EC2 c5.18xlarge instance, number of physical cores = 36
export OMP_NUM_THREADS=36
export KMP_AFFINITY='granularity=fine,verbose,compact,1,0'
# For an EC2 c5.18xlarge instance, number of physical cores / 4 = 36 /4 = 9
export TENSORFLOW_SESSION_PARALLELISM=9
export KMP_BLOCKTIME=1
export KMP_SETTINGS=0
```
+ <a name="XPH9CA3E9Dp"></a>A ResNet50\_v1\.5 model, trained with TensorFlow on the ImageNet dataset and using a NHWC image shape,  the training throughput performance was observed to be around 9x faster\. This is compared to the binary without MKL optimizations and measured in terms of samples/second\. The following environment variables were used: 

```
export TENSORFLOW_INTER_OP_PARALLELISM=0
# For an EC2 c5.18xlarge instance, number of logical cores = 72
export TENSORFLOW_INTRA_OP_PARALLELISM=72
# For an EC2 c5.18xlarge instance, number of physical cores = 36
export OMP_NUM_THREADS=36
export KMP_AFFINITY='granularity=fine,verbose,compact,1,0'
# For an EC2 c5.18xlarge instance, number of physical cores / 4 = 36 /4 = 9
export KMP_BLOCKTIME=1
export KMP_SETTINGS=0
```

 The following links will help you learn how to use to tune Intel MKL and your Deep Learning framework's settings to optimize your deep learning workload: 

 ​ 
+ <a name="XPH9CAnBecL"></a>[General best practices for Intel\-optimized TensorFlow Serving](https://github.com/IntelAI/models/blob/master/docs/general/tensorflow_serving/GeneralBestPractices.md) 
+ <a name="XPH9CAmzAjI"></a>[TensorFlow performance](https://www.tensorflow.org/guide/performance/overview) 
+ <a name="XPH9CAZEy8l"></a>[Some Tips for improving Apache MXNet performance](http://mxnet.apache.org/versions/master/faq/perf.html) 
+ <a name="XPH9CAMJJgE"></a>[MXNet with Intel MKL\-DNN \- Performance Benchmarking](https://cwiki.apache.org/confluence/display/MXNET/MXNet+with+Intel+MKL-DNN+-+Performance+Benchmarking#MXNetwithIntelMKL-DNN-PerformanceBenchmarking-InferenceAccuracy) 

 ​ 

### EC2 guide to set environment variables <a name="XPH9CAKZARe"></a>

 Refer to **** `docker run` documentation on how to set environment variables when creating a container: [https://docs\.docker\.com/engine/reference/run/\#env\-environment\-variables](https://docs.docker.com/engine/reference/run/#env-environment-variables) 

 ​ 

 The following is an example on setting en environment variable called `OMP_NUM_THREADS` for docker run\. 

```
ubuntu@ip-172-31-95-248:~$ docker run -e OMP_NUM_THREADS=36 -it --entrypoint "" 999999999999.dkr.ecr.us-east-1.amazonaws.com/beta-tensorflow-inference:1.13-py2-cpu-build bash
root@d437faf9b684:/# echo $OMP_NUM_THREADS
36
```

### ECS guide to set environment variables <a name="XPH9CAuufwx"></a>

To specify the environment variables for a container at runtime in ECS, you must edit the **ECS task definition**\. Add the environment variables in the form of 'name' and 'value' key\-pairs in `containerDefinitions` part of the task definition\. The following is an example of setting `OMP_NUM_THREADS` and `KMP_BLOCKTIME` variables\. 

```
{
    "requiresCompatibilities": [
        "EC2"
    ],
    "containerDefinitions": [{
        "command": [
            "mkdir -p /test && cd /test && git clone -b r1.13 https://github.com/tensorflow/serving.git && tensorflow_model_server --port=8500 --rest_api_port=8501 --model_name=saved_model_half_plus_two_cpu --model_base_path=/test/serving/tensorflow_serving/servables/tensorflow/testdata/saved_model_half_plus_two_cpu"
        ],
        "entryPoint": [
            "sh",
            "-c"
        ],
        "name": "EC2TFInference",
        "image": "999999999999.dkr.ecr.us-east-1.amazonaws.com/tf-inference:1.12-cpu-py3-ubuntu16.04",
        "memory": 8111,
        "cpu": 256,
        "essential": true,
        "environment": [{
              "name": "OMP_NUM_THREADS",
              "value": "36"
            },
            {
              "name": "KMP_BLOCKTIME",
              "value": 1
            }
        ],
        "portMappings": [{
                "hostPort": 8500,
                "protocol": "tcp",
                "containerPort": 8500
            },
            {
                "hostPort": 8501,
                "protocol": "tcp",
                "containerPort": 8501
            },
            {
                "containerPort": 80,
                "protocol": "tcp"
            }
        ],
        "logConfiguration": {
            "logDriver": "awslogs",
            "options": {
                "awslogs-group": "/ecs/TFInference",
                "awslogs-region": "us-west-2",
                "awslogs-stream-prefix": "ecs",
                "awslogs-create-group": "true"
            }
        }
    }],
    "volumes": [],
    "networkMode": "bridge",
    "placementConstraints": [],
    "family": "Ec2TFInference"
}
```

### EKS guide to set environment variables<a name="XPH9CAAktKY"></a>

 To specify the environment variables for the container at runtime, edit the raw manifests of your EKS job \(\.yaml, \.json\) \. The following snippet of a manifest shows the definition of a container, with name `squeezenet-service`\. Along with other attributes such as args and ports, the environment variables are listed in the form of 'name' and 'value' key\-pairs\. 

```
      containers:
      - name: squeezenet-service
        image: 999999999999.dkr.ecr.us-east-1.amazonaws.com/beta-mxnet-inference:1.4.0-py3-gpu-build
        command:
        - mxnet-model-server
        args:
        - --start
        - --mms-config /home/model-server/config.properties
        - --models squeezenet=https://s3.amazonaws.com/model-server/models/squeezenet_v1.1/squeezenet_v1.1.model
        ports:
        - name: mms
          containerPort: 8080
        - name: mms-management
          containerPort: 8081
        imagePullPolicy: IfNotPresent
        env:
        - name: AWS_REGION``
          value: us-east-1
        - name: OMP_NUM_THREADS
          value: 36
        - name: TENSORFLOW_INTER_OP_PARALLELISM
          value: 0
        - name: KMP_AFFINITY
          value: 'granularity=fine,verbose,compact,1,0'
        - name: KMP_BLOCKTIME
          value: 1
```